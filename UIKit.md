# UIKit

- [UIImage](#uiimage)
- [UIView](#uiview)

---

### UIImage

#####+ (UIImage *)imageNamed:(NSString *)fileName

If displaying a bundle image only once, it is recommended to use `+ (UIImage *)imageWithContentsOfFile:(NSString *)path` instead so as not to have the system cache the image. Per Apple documentation:
> If you have an image file that will only be displayed once and wish to ensure that it does not get added to the system’s cache, you should instead create your image using imageWithContentsOfFile:. This will keep your single-use image out of the system image cache, potentially improving the memory use characteristics of your app.

[Source](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImage_Class/Reference/Reference.html#//apple_ref/occ/clm/UIImage/imageNamed:)

### UIView

#####@property(nonatomic) CGRect frame

When setting the frame property of a `UIView` item, it is valuable to ensure that the coordinates correspond to pixel locations, otherwise anti-aliasing will occur, reducing performance and potentially causing blurry edges in your interface. One straightforward solution is to use `CGRectIntegral()` to automatically round the `CGRect` values to the nearest point. For screens with a higher pixel density than 1 pixel per point, the coordinates only need to be rounded to the nearest `1.0f / screen.scale`
