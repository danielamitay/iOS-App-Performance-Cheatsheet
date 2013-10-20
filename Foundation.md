# Foundation

- [NSFileManager](#nsfilemanager)

---

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
