# iOS App Performance Cheatsheet

**A collection of code substitutions that can improve the performance of Objective-C code on iOS.**

Most code substitutions documented here consist of replacing high-level APIs that were made for flexibility over performance with lower-level alternatives for the task at hand. Good architecture and proper threading is always important for app performance, but sometimes we need to use specialized implementations.

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

### Why is this on GitHub?

- Great format for saving to disk or accessing via the web
- Encourages contributions and improvements
- [GitHub Flavored Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) looks nice

### Contributions

Pull requests are welcome! The best contributions will consist of substitutions for classes/methods known to block the main thread during a typical app lifecycle.

### Contact

- hello@danielamitay.com
- http://www.danielamitay.com
