###关于 AOP
* [<font color=red>百度百科</font>](https://baike.baidu.com/item/AOP/1332219?fr=aladdin): 在软件业，AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。
* [<font color=red>维基百科</font>](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E4%BE%A7%E9%9D%A2%E7%9A%84%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1): 面向侧面的程序设计（aspect-oriented programming，AOP，又译作面向方面的程序设计、观点导向编程、剖面导向程序设计）是计算机科学中的一个术语，指一种程序设计范型。该范型以一种称为侧面（aspect，又译作方面）的语言构造为基础，侧面是一种新的模块化机制，用来描述分散在对象、类或函数中的横切关注点（crosscutting concern）。
侧面的概念源于对面向对象的程序设计的改进，但并不只限于此，它还可以用来改进传统的函数。与侧面相关的编程概念还包括元对象协议、主题（subject）、混入（mixin）和委托。
* <font color = red>语言实现</font>: 最广为人知的面向侧面的程序设计语言是由施乐帕洛阿尔托研究中心开发的AspectJ，该语言可以和Java编程语言结合在一起使用。

![AOP 概念图](https://raw.githubusercontent.com/geys1991/ImageRepository/master/AOP/AOP%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5.jpeg)
<center><font color=gray>------------------------     </font>AOP 概念图<font color=gray>     ------------------------</font></center>

### AOP 的基本概念
	1. 主关注点: 项目中的功能模块。
	2. 关注点分离: 将主关注点和横切关注点或者其他关注点分离 
	2. 横切: 对于关注点有交集的部分，既可以提取整合的功能代码。
	3. 支配性分解: 项目模块化的方式。项目的主要分解方式
	4. 横切关注点: 分散在项目各处的关注点(区别于类这样的关注点)。切面的关注点
	5. 侧面: 这个理解为一种方式，方法，用来捕捉，发现横切关注点。
	6. 将软件分解成模块的主要方式。传统的程序设计语言是以一种线性的文本来描述软件的，只采用一种方式（比如：类）将软件分解成模块；这导致某些关注点比较好的被捕捉，容易进一步组合、扩展；但还有一些关注点没有被捕捉，弥散在整个软件内部。支配性分解一般是按主关注点进行模块分解的。

### AOP 的理解
AOP把软件系统分为两个部分：主关注点和横切关注点，业务处理的主要流程是主关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处都基本相似。比如权限认证、日志、事务处理。AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来。<font color=red>AOP的核心思想就是“将应用程序中的商业逻辑同对其提供支持的通用服务进行分离。“</font>

 一般而言，我们管切入到指定类指定方法的代码片段称为切面，而切入到哪些类、哪些方法则叫切入点。有了AOP，我们就可以把几个类共有的代码，抽取到一个切片中，等到需要时再切入对象中去，从而改变其原有的行为。
 
  OOP 高内聚，低耦合，功能分散，类的复用性，但是增加代码重复性（如果增加一个类就增加了耦合度）。
 
 AOP使OOP由原来的二维变为三维了，由平面变成立体了。
 
 面向切面的编程，就是利用抽象出来的切面(aspect) 来缓解代码的散布(scattering)与纠缠(tangling)的编程方法。
 
 	1. 散布：对于一个给定的概念，与其相关的功能由多个类来实现。
	2. 纠缠：类本身解决不止一个的关注点。
 
	1. 不增加耦合度
	2. 添加统一的功能


### 项目中的应用
	1. 埋点
	2. 登录



### AOP 在 iOS 中的实现

* Method Swizzling
	1. 方案1： 

```
+(void)load
{
    Method originMethod     = class_getInstanceMethod([self class], @selector(sendAction:to:forEvent:));
    Method swizzlingMethod  = class_getInstanceMethod([self class], @selector(YS_sendAction:to:forEvent:)); 
    method_exchangeImplementations(originMethod, swizzlingMethod);
}
-(void)YS_sendAction:(SEL)action to:(id)target forEvent:(UIEvent *)event
{
    NSLog(@"class: %@,  method: %@", NSStringFromClass([self class]), NSStringFromSelector(action));
    [self YS_sendAction: action to: target forEvent: event];
}
```
输出：
> class: UIButton,  method: click: 
	
	
* Aspects

```
+(void)load
{
    [self aspect_hookSelector: @selector(sendAction:to:forEvent:) withOptions:AspectPositionAfter usingBlock:^(id<AspectInfo> aspects, SEL action, id target, UIEvent *event)
     {
     // do something
     } error:nil];
}
```

### 总结

|		类名							|		Detail
|		:---							|		:---
| UIViewController					| 		生命周期函数，viewWillAppear...
| UIGestureRecognizer				| 		手势捕获
| UITableView, UICollectionView	| 		didSelectedAtIndex...
| UIControl							| 		UIButton,UITextField...
| UIAlertController					|		类似 删除晒单这种点击 alertView 跳转的情况
| 网络请求								|		



### 参考链接:
* [面向切面编程](http://wiki.jikexueyuan.com/project/objc-zen-book/aop.html)
* [An Aspect Oriented Programming Approach to iOS Analytics](http://albertodebortoli.com/blog/2014/03/25/an-aspect-oriented-approach-programming-to-ios-analytics/)
* [AOP 关于埋点的实践](http://www.vienta.me/2016/09/21/AOP%E5%9C%A8iOS%E4%B8%AD%E7%9A%84%E5%AE%9E%E8%B7%B5%E4%B8%80%E7%BB%9F%E8%AE%A1%E5%9F%8B%E7%82%B9/)
* [Method Swizzling和AOP(面向切面编程)实践](http://www.cocoachina.com/ios/20150120/10959.html)
* [面向切面编程之 Aspects 源码解析及应用](https://wereadteam.github.io/2016/06/30/Aspects/)






