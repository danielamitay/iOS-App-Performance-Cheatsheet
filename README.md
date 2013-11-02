# iOS App Performance Cheatsheet

**A collection of code substitutions and configurations that can improve the performance of Objective-C code on iOS.**

Most code substitutions and configurations documented here consist of replacing high-level APIs that were made for flexibility over performance with lower-level alternatives for the task at hand, or class properties that affect drawing performance. Good architecture and proper threading is always important for app performance, but sometimes we need to use specialized implementations.

### Table of Contents

**iOS App Performance Cheatsheet** is sectioned into Markdown files by Objective-C framework:
- [**Foundation**](Foundation.md)
	- [NSDateFormatter](Foundation.md#nsdateformatter)
	- [NSFileManager](Foundation.md#nsfilemanager)
	- [NSObjCRuntime](Foundation.md#nsobjcruntime)
	- [NSString](Foundation.md#nsstring)
- [**UIKit**](UIKit.md)
	- [UIImage](UIKit.md#uiimage)
	- [UIView](UIKit.md#uiview)
- [**QuartzCore**](QuartzCore.md)
	- [CALayer](QuartzCore.md#calayer)

### Why is this on GitHub?

- Great format for saving to disk or accessing via the web
- Encourages contributions and improvements
- [GitHub Flavored Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) looks nice

### Disclaimer

There is a reason why this is called a *cheatsheet*. You should avoid premature optimizations, and only seek out code replacements when you have determined that a specific code path has become a performance bottleneck. Hopefully, however, this cheatsheet will provide a bit of insight into some of the bottlenecks that usually arise, and some of the options available to you.

### Contributions

Pull requests are welcome! The best contributions will consist of substitutions or configurations for classes/methods known to block the main thread during a typical app lifecycle.

### Contact

- hello@danielamitay.com
- http://www.danielamitay.com
