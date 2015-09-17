---
title : UITabbarController 的诡异行为
---

* UITabbarController 和其他的ViewController 行为不太一样。生命周期大体相同，但是加载顺序不同。

* UITabbarController 子类的 initializer 在执行 [super init] 时， tabbarController 会立即调用 loadView，视图加载完毕后，导致viewDidLoad的调用。

* 导致的表面现象是 子类的 initializer 触发了 viewDidLoad，即viewDidLoad逻辑在初始化方法未执行完毕时，就会被调用。


* 猜测是UITabbarController.m init 的实现里面，需要把UITabbar 加载到 self.view 上，从而提前加载了视图）

* 如果你现在tabbarVC 里面，提供一些初始化逻辑，并且希望在view load 之前完成，那么你会就踩到这个坑。




* 附：大家来温习一下 viewDidLoad 基本概念


 
 		- (void)viewDidLoad; 
 		// Called after the view has been loaded. For view controllers created in code, this is after -loadView. For view controllers unarchived from a nib, this is after the view is set.

		Called after the controller's view is loaded into memory.This method is called after the view controller has loaded its view hierarchy into memory. This method is called regardless of whether the view hierarchy was loaded from a nib file or created programmatically in the loadView method. You usually override this method to perform additional initialization on views that were loaded from nib files.
