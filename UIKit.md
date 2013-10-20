# UIKit

- [UIImage](#uiimage)

---

### UIImage

#####+ (UIImage *)imageNamed:(NSString *)fileName

If displaying a bundle image only once, it is recommended to use `+ (UIImage *)imageWithContentsOfFile:(NSString *)path` instead so as not to have the system cache the image. Per Apple documentation:
> If you have an image file that will only be displayed once and wish to ensure that it does not get added to the system’s cache, you should instead create your image using imageWithContentsOfFile:. This will keep your single-use image out of the system image cache, potentially improving the memory use characteristics of your app.

[Source](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImage_Class/Reference/Reference.html#//apple_ref/occ/clm/UIImage/imageNamed:)
