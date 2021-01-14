#  IOS开发

# UI

## UIViewController
其生命周期及其回调时机

## view

## UITabBarController

## UINavigationController

## present,push区别


# 线程

# 编译、连接、运行

# IO

# NetWork

# 优化
## 内存泄漏

## UI卡顿

# 注意事项

# 其他

# IDFA, SKAdNetwork

# 流媒体

# 图片

# json,序列化 

# 反射
```swift
extension UIViewController {
    
    // We cannot override load like we could in Objective-C, so override initialize instead
    public override static func initialize() {
        
        // Make a static struct for our dispatch token so only one exists in memory
        struct Static {
            static var token: dispatch_once_t = 0
        }
        
        // Wrap this in a dispatch_once block so it is only run once
        dispatch_once(&Static.token) {
            // Get the original selectors and method implementations, and swap them with our new method
            let originalSelector = #selector(UIViewController.viewDidLoad)
            let swizzledSelector = #selector(UIViewController.myViewDidLoad)
            
            let originalMethod = class_getInstanceMethod(self, originalSelector)
            let swizzledMethod = class_getInstanceMethod(self, swizzledSelector)
            
            let didAddMethod = class_addMethod(self, originalSelector, method_getImplementation(swizzledMethod), method_getTypeEncoding(swizzledMethod))
            
            // class_addMethod can fail if used incorrectly or with invalid pointers, so check to make sure we were able to add the method to the lookup table successfully
            if didAddMethod {
                class_replaceMethod(self, swizzledSelector, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod))
            } else {
                method_exchangeImplementations(originalMethod, swizzledMethod);
            }
        }
    }
    
    // Our new viewDidLoad function
    // In this example, we are just logging the name of the function, but this can be used to run any custom code
    func myViewDidLoad() {
        // This is not recursive since we swapped the Selectors in initialize().
        // We cannot call super in an extension.
        self.myViewDidLoad()
        print("do what you want here") // logs myViewDidLoad()
    }
}
```
