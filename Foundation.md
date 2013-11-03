# Foundation

- [NSDateFormatter](#nsdateformatter)
- [NSFileManager](#nsfilemanager)
- [NSObjCRuntime](#nsobjcruntime)
- [NSString](#nsstring)

---

### NSDateFormatter

`NSDateFormatter` is not the only class that is expensive to setup, but it is expensive and utilized enough that Apple specifically recommends caching and reusing instances where possible.

> Creating a date formatter is not a cheap operation. If you are likely to use a formatter frequently, it is typically more efficient to cache a single instance than to create and dispose of multiple instances. One approach is to use a static variable.

[Source](https://developer.apple.com/library/ios/documentation/cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html)

A common method of caching `NSDateFormatter`s is to use `-[NSThread threadDictionary]` (because `NSDateFormatter` is not thread-safe):

```objective-c
+ (NSDateFormatter *)cachedDateFormatter {
	NSMutableDictionary *threadDictionary = [[NSThread currentThread] threadDictionary];
	NSDateFormatter *dateFormatter = [threadDictionary objectForKey:@"cachedDateFormatter"];
    if (dateFormatter == nil) {
        dateFormatter = [[NSDateFormatter alloc] init];
        [dateFormatter setLocale:[NSLocale currentLocale]];
        [dateFormatter setDateFormat: @"YYYY-MM-dd HH:mm:ss"];
        [threadDictionary setObject:dateFormatter forKey:@"cachedDateFormatter"];
    }
    return dateFormatter;
}
```

#####- (NSDate *)dateFromString:(NSString *)string

This is potentially the most common iOS performance bottleneck. As a result, much effort has been dedicated to discovering alternatives. Below are the best known `NSDateFormatter` substitutions for [ISO8601](http://en.wikipedia.org/wiki/ISO_8601) to `NSDate` conversion.

######strptime 

```objective-c
//#include <time.h>

time_t t;
struct tm tm;
strptime([iso8601String cStringUsingEncoding:NSUTF8StringEncoding], "%Y-%m-%dT%H:%M:%S%z", &tm);
tm.tm_isdst = -1;
t = mktime(&tm);
[NSDate dateWithTimeIntervalSince1970:t + [[NSTimeZone localTimeZone] secondsFromGMT]];
```

[Source](http://sam.roon.io/how-to-drastically-improve-your-app-with-an-afternoon-and-instruments)

######sqlite3

```objective-c
//#import "sqlite3.h"

sqlite3 *db = NULL;
sqlite3_open(":memory:", &db);
sqlite3_stmt *statement = NULL;
sqlite3_prepare_v2(db, "SELECT strftime('%s', ?);", -1, &statement, NULL);
sqlite3_bind_text(statement, 1, [iso8601String UTF8String], -1, SQLITE_STATIC);
sqlite3_step(statement);
int64_t value = sqlite3_column_int64(statement, 0);
NSDate *date = [NSDate dateWithTimeIntervalSince1970:value];
sqlite3_clear_bindings(statement);
sqlite3_reset(statement);
```

[Source](http://vombat.tumblr.com/post/60530544401/date-parsing-performance-on-ios-nsdateformatter-vs)


### NSFileManager

#####- (NSDictionary *)attributesOfItemAtPath:(NSString *)filePath error:(NSError *)error

When attempting to retrieve an attribute of a file on disk, using `–[NSFileManager attributesOfItemAtPath:error:]` will expend an excessive amount of time fetching additional attributes of the file that you may not need. Instead of using `NSFileManager`, you can directly query the file properties using `stat`:

```objective-c
//#import <sys/stat.h>

struct stat statbuf;
const char *cpath = [filePath fileSystemRepresentation];
if (cpath && stat(cpath, &statbuf) == 0) {
    NSNumber *fileSize = [NSNumber numberWithUnsignedLongLong:statbuf.st_size];
    NSDate *modificationDate = [NSDate dateWithTimeIntervalSince1970:statbuf.st_mtime];
    NSDate *creationDate = [NSDate dateWithTimeIntervalSince1970:statbuf.st_ctime];
    // etc
}
```

### NSObjCRuntime

#####NSLog(NSString *format, ...)

`NSLog()` writes messages to the Apple System Log facility. Written messages are presented in the debugger console when built and run via Xcode, in addition to the device's console log even in production. Additionally, `NSLog()` statements are serialized by the system and performed on the main thread. Even on fairly new iOS hardware, `NSLog()` takes a non-negligible amount of time while only providing debug value. As a result, it is recommended to use `NSLog()` as sparingly as possible in production.

> Calling `NSLog` makes a new calendar for each line logged. Avoid calling `NSLog` excessively.

[Source](https://developer.apple.com/videos/wwdc/2012/?id=235)

The following are commonly used log definitions that are used to selectively perform `NSLog()` in debug/production:

```objective-c
#ifdef DEBUG
// Only log when attached to the debugger
#    define DLog(...) NSLog(__VA_ARGS__)
#else
#    define DLog(...) /* */
#endif
// Always log, even in production
#define ALog(...) NSLog(__VA_ARGS__)
```

[Source](http://iphoneincubator.com/blog/debugging/the-evolution-of-a-replacement-for-nslog)

### NSString

#####+ (instancetype)stringWithFormat:(NSString *)format,, ...

`NSString` creation is not particularly expensive, but when used in a tight loop (as dictionary keys, for example), `+[NSString stringWithFormat:]` performance can be improved dramatically by being replaced with `asprintf` or similar functions in C.

```objective-c
NSString *firstName = @"Daniel";
NSString *lastName = @"Amitay";
char *buffer;
asprintf(&buffer, "Full name: %s %s", [firstName UTF8String], [lastName UTF8String]);
NSString *fullName = [NSString stringWithCString:buffer encoding:NSUTF8StringEncoding];
free(buffer);
```

#####- (instancetype)initWithFormat:(NSString *)format, ...

See [`+[NSString stringWithFormat:]`](#-instancetypestringwithformatnsstring-format-)
