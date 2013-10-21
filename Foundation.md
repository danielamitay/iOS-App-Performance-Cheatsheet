# Foundation

- [NSDateFormatter](#nsdateformatter)
- [NSFileManager](#nsfilemanager)

---

### NSDateFormatter

`NSDateFormatter` is expensive to setup, so it is advisable to reuse instances where possible.

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
