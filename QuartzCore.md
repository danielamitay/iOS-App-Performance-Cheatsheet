# QuartzCore

- [CALayer](#calayer)

---

### CALayer

#####@property BOOL allowsGroupOpacity

As of iOS7, this property indicates whether or not the layer's sublayers inherit the layer's opacity. The primary use case for this functionality is when animating a layer's transparency (causing the opacity of subviews to be visible). However, if this rendering style is not needed for your use case, it can be disabled to improve performance.

> When true, and the layer's opacity property is less than one, the layer is allowed to composite itself as a group separate from its parent. This gives the correct results when the layer contains multiple opaque components, but may reduce performance. 
> 
> The default value of the property is read from the boolean UIViewGroupOpacity property in the main bundle's Info.plist. If no value is found in the Info.plist the default value is YES for applications linked against the iOS 7 SDK or later and NO for applications linked against an earlier SDK.

Source unavailable at this time. See `CALayer.h` in Xcode.

> (Default on iOS 7 and later) Inherit the opacity of the superlayer. This option allows for more sophisticated rendering in the simulator but can have a noticeable impact on performance.

[Source](https://developer.apple.com/library/ios/documentation/general/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW6)

#####@property BOOL drawsAsynchronously

The `drawsAsynchronously` property causes the layer's `CGContext` to defer drawing to a background thread. This property will provide the greatest benefit when enabled on layers that are redrawn frequently.

> When this property is set to YES, the graphics context used to draw the layer’s contents queues drawing commands and executes them on a background thread rather than executing them synchronously. Performing these commands asynchronously can improve performance in some apps. However, you should always measure the actual performance benefits before enabling this capability.

[Source](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/Introduction/Introduction.html#//apple_ref/occ/instp/CALayer/drawsAsynchronously)

> Any drawing that you do in your delegate’s drawLayer:inContext: method or your view’s drawRect: method normally occurs synchronously on your app’s main thread. In some situations, though, drawing your content synchronously might not offer the best performance. If you notice that your animations are not performing well, you might try enabling the drawsAsynchronously property on your layer to move those operations to a background thread. If you do so, make sure your drawing code is thread safe.

[Source](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/ImprovingAnimationPerformance/ImprovingAnimationPerformance.html)

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

Setting a `CALayer`'s `shouldRasterize` property to `YES` can improve performance for layers that need to be drawn only once. This layer can still be moved, scaled, and transformed. If a layer needs to be redrawn often, then setting `shouldRasterize` to `YES` can actually hurt drawing performance, because the system will attempt to rasterize the layer after each draw.

> When the value of this property is YES, the layer is rendered as a bitmap in its local coordinate space and then composited to the destination with any other content. Shadow effects and any filters in the filters property are rasterized and included in the bitmap.

[Source](https://developer.apple.com/library/mac/documentation/GraphicsImaging/Reference/CALayer_class/Introduction/Introduction.html#//apple_ref/occ/instp/CALayer/shouldRasterize)
