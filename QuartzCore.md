# QuartzCore

- [CALayer](#calayer)

---

### CALayer

#####@property CGPathRef shadowPath

When applying shadow properties to a `CALayer`, it is recommended to set the `shadowPath` of the layer so as to allow the system to cache the shadow and reduce the amount of drawing necessary. When modifying the layer's bounds, the `shadowPath` should be re-set.

```objective-c
CALayer *layer = view.layer;
layer.shadowOpacity = 0.5f;
layer.shadowRadius = 10.0f;
layer.shadowOffset = CGSizeMake(0.0f, 10.0f);
UIBezierPath *bezierPath = [UIBezierPath bezierPathWithRect:layer.bounds];
layer.shadowPath = bezierPath.CGPath;
```

> Letting Core Animation determine the shape of a shadow can be expensive and impact your app’s performance. Rather than letting Core Animation determine the shape of the shadow, specify the shadow shape explicitly using the shadowPath property of CALayer. When you specify a path object for this property, Core Animation uses that shape to draw and cache the shadow effect. For layers whose shape never changes or rarely changes, this greatly improves performance by reducing the amount of rendering done by Core Animation.

[Source](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/ImprovingAnimationPerformance/ImprovingAnimationPerformance.html)

#####@property BOOL shouldRasterize

Setting a `CALayer`'s `shouldRasterize` property to `YES` can improve performance for layers that need to be drawn only once. This layer can still be moved, scaled, and transformed. If a layer needs to be redrawn often, then setting `shouldRasterize` to `TRUE` can actually hurt drawing performance, because the system will attempt to rasterize the layer after each draw.

> When the value of this property is YES, the layer is rendered as a bitmap in its local coordinate space and then composited to the destination with any other content. Shadow effects and any filters in the filters property are rasterized and included in the bitmap.

[Source](https://developer.apple.com/library/mac/documentation/GraphicsImaging/Reference/CALayer_class/Introduction/Introduction.html#//apple_ref/occ/instp/CALayer/shouldRasterize)
