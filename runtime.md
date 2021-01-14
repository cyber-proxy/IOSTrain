# runtime
[运行时](https://www.jianshu.com/p/adf0d566c887)
在 <objc/runtime.h> 中有一个method_exchangeImplementations方法，可以改变selector指向的IMP'，，说白了，我们就是要改变selector的实现。比如在友盟统计中，我们需要在 - (void)viewWillAppear:(BOOL)animated 中打点。其实我们可以把打点的代码写在父类中，然后让需要打点的页面都继承这个父类，但是工作量就比较大，而且代码恶心。
　最优解就是我们定义一个Category，在这个Category中，偷偷- (void)viewWillAppear:(BOOL)animated中的实现指向另一个我们预想的IMP。 
