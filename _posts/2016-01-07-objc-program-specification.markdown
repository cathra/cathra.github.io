---
layout: post
title: "Objective-C 编码规范"
date: 2016-01-07 17:24:35
author: "cathra"
catalog: true
tags:
    - iOS
	- ObjC
    - 规范
---


前言
===
***

统一编程风格，提高代码的可读性与编程效率，避免团队开发可能带来的混乱。

适用范围
===
***

本规范适用于公司项目产品运用Objective-C作为开发语言的编码活动。

<!-- more -->

定义
===
***

* 规则 : 编程时必须遵守的约定
* 建议 : 编程时需要考虑的约定
* 绿色代码 : 对此规则或建议给出的正确例子
* 红色代码: 对此规则或建议给出的反面例子
* 匈牙利命名法：是一种编程时的命名规范。基本原则是：变量名＝属性＋类型＋对象描述，其中每一对象的名称都要求有明确含义，可以取对象名字全称或名字的一部分
* 驼峰命名法：就是当变量名或函式名是由一个或多个单字连结在一起，而构成的唯一识别字时，驼峰命名法第一个单字以小写字母开始；第二个单字的首字母大写或每一个单字的首字母都采用大写字母

示例
===
***

先看一个实例对代码的基本格式有大致了解，可以看到基本的间距，命名等等。

下例是一份头文件，展示对@interface声明正确的注释和留间距。

``` ObjC
 	//  GTMFoo.h
 	//  FooProject
 	//
 	//  Created by Greg Miller on 6/13/08.
 	//  Copyright 2008 Google, Inc. All rights reserved.
 	//		
 	
 	#import <Foundation/Foundation.h>
 	
	 // A sample class demonstrating good Objective-C style. All interfaces,
	 // categories, and protocols (read: all top-level declarations in a header)
	 // MUST be commented. Comments must also be adjacent to the object they're
	 // documenting.
	 //
	 // (no blank line between this comment and the interface)
 	@interface GTMFoo : NSObject {
	@private
   		NSString *foo_;
   		NSString *bar_;
 	}
 	
 	// Returns an autoreleased instance of GMFoo. See -initWithString: for details
 	// about the argument.
 	+ (id)fooWithString:(NSString *)string;
 	
 	// Designated initializer. |string| will be copied and assigned to |foo_|.
	- (id)initWithString:(NSString *)string;
 
 	// Gets and sets the string for |foo_|.
	- (NSString *)foo;
	- (void)setFoo:(NSString *)newFoo;
 	
 	// Does some work on |blah| and returns YES if the work was completed
 	// successfuly, and NO otherwise.
 	- (BOOL)doWorkWithString:(NSString *)blah;
 
 	@end
```

下例是一份源文件，展示对接口的@implementation的实现的正确注释和留间隔。它也包括了主要方法如getters，setters，init和dealloc的相关实现。

``` ObjC
	//
	//  GTMFoo.m
 	//  FooProject
 	//
 	//  Created by Greg Miller on 6/13/08.
 	//  Copyright 2008 Google, Inc. All rights reserved.
 	//
 
 	#import "GTMFoo.h"
 
 
 	@implementation GTMFoo
 
 	+ (id)fooWithString:(NSString *)string {
   		return [[[self alloc] initWithString:string] autorelease];
 	}
 
 	// Must always override super's designated initializer.
 	- (id)init {
  		return [self initWithString:nil];
 	}
 
 	- (id)initWithString:(NSString *)string {
   		if ((self = [super init])) {
     		foo_ = [string copy];
     		bar_ = [[NSString alloc] initWithFormat:@"hi %d", 3];
   		}
   		return self;  
 	}
 
 	- (void)dealloc {
   		[foo_ release];
   		[bar_ release];
   		[super dealloc];
 	}
 
 	- (NSString *)foo {
   		return foo_;
 	}
 
 	- (void)setFoo:(NSString *)newFoo {
   		[foo_ autorelease];
   		foo_ = [newFoo copy];  
 	}
 
 	- (BOOL)doWorkWithString:(NSString *)blah {
   		// ...
   		return NO;
 	}
 
 	@end
```

 代码布局
 ===
 ***

* 间隔与格式化

空格对tab键，仅使用空格，缩进四个字符。

使用空格用于缩进，不要在编码时使用tab键。如果要使用应该设置你的编辑器将tab键转换成对应的空格。

* 行长度

代码中的每行文本不要超过80个字符的长度。

尽管Objective-C正变得比C++更加繁冗，为了保持规范的互通性，还是决定保持80字符长度的限制。
你可以在Xcode里清楚地发现代码中的违规，设置Xcode>Preferences>TextEditing>Showpageguide。(之后就可以在代码编辑区域里看到一条指定字符长度的指示线了)

* 方法声明与定义

留一个空格在-或+和返回类型之间，但参数列表里的参数之间不要留间隔。

参数对象的星号前需要加空格。

方法应该写成这样:

``` ObjC
	- (void)doSomethingWithString:(NSString *)theString {
  		...
	}
```

如果参数过多，推荐每个参数各占一行。使用多行的情况下，在参数前加冒号用于对齐:

``` ObjC
	- (void)doSomethingWith:(GTMFoo *)theFoo
    	               rect:(NSRect)theRect
        	       interval:(float)theInterval {
  		...
	}
```

当第一个关键字比其他的短时，后续行至少缩进四个空格。这样你可以让后续的关键字垂直对齐，而不是用冒号对齐:

``` ObjC
	- (void)short:(GTMFoo *)theFoo
    	longKeyword:(NSRect)theRect
    	evenLongerKeyword:(float)theInterval {
  	...
	}
```

* 方法

方法调用的格式和方法声明时的格式时一致的，如果格式风格可选，遵从原有代码的风格。

调用应该将所有参数写在一行:

``` ObjC
	[myObject doFooWith:arg1 name:arg2 error:arg3];
```

或者每个参数一行，用冒号对齐(对齐效果如前说明)：

``` ObjC
	[myObject doFooWith:arg1
    	           name:arg2
        	      error:arg3];
```

不要使用如下风格的写法：

``` ObjC
	[myObject doFooWith:arg1 name:arg2  // some lines with >1 arg
    	          error:arg3];

	[myObject doFooWith:arg1
    	           name:arg2 error:arg3];

	[myObject doFooWith:arg1
          	name:arg2  // aligning keywords instead of colons
          	error:arg3];
```

在声明和定义时，如果因为关键字长度使就算有四个空格在前仍然无法用冒号对齐，那么就让后续行缩进四个空格而不是冒号对齐:

``` ObjC
	[myObj short:arg1
    	longKeyword:arg2
    	evenLongerKeyword:arg3];
```

* @public和@private

权限控制符@public和@private缩进一个空格：

``` ObjC
	@interface MyClass : NSObject {
		@public
  		...
 		@private
  		...
	}
	@end
```

* 异常

每个异常标签的@和开括号({)写在统一行，标签和开括号间隔一个空格，同样适用于@catch语句。

``` ObjC
	@interface MyClass : NSObject {
 		@public
  		...
 		@private
  		...
	}
	@end
```

* Protocols

在类型定义和被中括号括起来的Protocols名称之间不需要空格。
本规矩适用于类定义，变量声明和方法声明时使用。参考代码：

``` ObjC
	@interface MyProtocoledClass : NSObject<NSWindowDelegate> {
 		@private
  		id<MyFancyDelegate> delegate_;
	}
	- (void)setDelegate:(id<MyFancyDelegate>)aDelegate;
	@end
```

命名
===
***

所有类，类别，方法，以及变量如包括缩写，则缩写部分使用全大写的缩写(Initialisms)形式。这遵守Apple的标准，比如URL，TIFF以及EXIF。
当写Objective-C++代码时，情况就不是那么单一了。许多项目需要实现带一些Objective-C代码的跨平台的C++APIs或者连接后台的C++代码与前台的原生Cocoa代码，这会造成两种规范直接冲突。
解决方法是根据方法/函数风格来决定。如果在@implementation块，就使用Objective-C的命名规则；如果在C++的方法实现块，就使用C++的命名规则。避免了实体变量和本地变量在一个函数内命名规则冲突的情况，而这种情况是对可读性的极大损害。


* 文件名

文件名反映了它所包含的实现类的名字，遵从你所在项目的习惯。
文件扩展名使用如下规则：

|     |                                    |
|-----|-----------------------------------:|
|.h   | C/C++/Objective-C header file      |
|.m   | Objective-C implementation file    |
|.mm  | Objective-C++ implementation file  |
|.cc  | Pure C++ implementation file       |
|.c   | C implementation file              |

类别的文件名应该包含扩展类的名字，比如GTMNSString+Utils.h or GTMNSTextView+Autocomplete.h。


* Objective-C++

在一份源文件里，Objective-C++代码遵守当前方法/函数的风格
为了尽量减少不同命名风格间的冲突，使用当前方法的风格。如果在@implementation块，使用Objective-C命名规则，如果在C++类的函数实现块，使用C++命名规则。

``` ObjC
	// file: cross_platform_header.h

	class CrossPlatformAPI {
		public:
  		...
  		int DoSomethingPlatformSpecific();  // impl on each platform
 		private:
  		int an_instance_var_;
	};

	// file: mac_implementation.mm
	#include "cross_platform_header.h"

	// A typical Objective-C class, using Objective-C naming.
	@interface MyDelegate : NSObject {
 		@private
  		int instanceVar_;
  		CrossPlatformAPI* backEndObject_;
	}
	- (void)respondToSomething:(id)something;
	@end

	@implementation MyDelegate
	- (void)respondToSomething:(id)something {
  		// bridge from Cocoa through our C++ backend
  		instanceVar_ = backEndObject->DoSomethingPlatformSpecific();
  		NSString* tempString = [NSString stringWithInt:instanceVar_];
  		NSLog(@"%@", tempString);
	}
	@end

	// The platform-specific implementation of the C++ class, using
	// C++ naming.
	int CrossPlatformAPI::DoSomethingPlatformSpecific() {
  		NSString* temp_string = [NSString stringWithInt:an_instance_var_];
  		NSLog(@"%@", temp_string);
  		return [temp_string intValue];
	}
```

* 类命名

类名(不包括类别和协议名)应该用大写开头的驼峰命名法。
在应用级别的代码里，尽量不要使用带前缀的类名。每个类都有相同的前缀不能提高可读性。不过如果是编写多个应用间的共享代码，前缀就是可接受并推荐的做法了(型如GTMSendMessage)。


* 类别命名

类别命名应该以两三个字符的分类前缀作为一个项目或通用的公用部分。类别名应该包含类的扩展。
举个例子，如果我们想要创建一个基于NSString的类别用于解析，我们应该把类别放到名字是GTMNSString+Parsing.h的文件里，而类别本身的名字则是GTMStringParsingAdditions(是的，我们明白这个类别和其文件名字不匹配，但这个文件可能还包括其他用于解析相关的类别)。类别的方法应该都使用一个前缀(型如gtm_myCategoryMethodOnAString)，以防止Objective-C代码在单名空间里冲突。如果代码本来就不考虑共享或在不同的地址空间(address-space)，方法命名规则就没必要恪守了。

* Objective-C 方法命名

方法使用小写开头的驼峰法命名，每个参数都应该小写开头。
方法名应该尽可能读起来像一句话，参数名就相对方法名的补充说明(比如convertPoing:fromRect:或者replaceCharactersInRange:withString:)。
存取(Accessor)方法应该一致的在"取(getting)"的时候直接用变量名而不是在签名加"get"，如下:

``` ObjC
	- (id)getDelegate;  // AVOID
	- (id)delegate;    // GOOD
```

不过这仅针对Objective-C代码，C++代码仍然遵循自己的代码规范。

* 变量命名

变量名使用小写开头的驼峰法，类成员变量名最后加一个下划线，比如:myLovalVariable，myInstanceVariable_。
不要使用匈牙利命名法去标记语法，比如静态类型或变量类型(int或pointer之类的)。使变量名尽量可以推测其用途属性具有描述性。别一心想着少打几个字母，让你的代码可以迅速被理解更加重要。比如:


``` ObjC
	int w;
	int nerr;
	int nCompConns;
	tix = [[NSMutableArray alloc] init];
	obj = [someObject object];
	p = [network port];


	int numErrors;
	int numCompletedConnections;
	tickets = [[NSMutableArray alloc] init];
	userInfo = [someObject object];
	port = [network port];
```

* 实体变量

实体变量用驼峰法命名并后缀下划线，就像usernameTextField_。允许一种例外就是用KVO/KVC去绑定一个实体变量而Objective-C2.0不能用(因为操作系统的限制)的情况，此时也可用前缀下划线的方法给每个变量命名。如果可以使用Objective-C2.0，@property和@synthesize提供了遵守命名规范的解决方法。


* 常量

常量(预定义，枚举，局部常量等)使用小写k开头的驼峰法，比如kInvalidHandle，kWritePerm。


注释
===
***

注释是使代码保持可读性的极端重要的方式。下面的条款描述了你应该注释什么以及在哪里做注释。但是记住:即使注释是如此重要，最好的代码还是自说明式的。起一个有意义的名字比起一个晦涩的名字然后在用注释去解释它好的多。

写注释的时候，记住注释是写给读者，即下一个要理解你的代码并继续开发的人。"下一个"完全可能就是你自己。

同样，所有C++编程规范的条款仍然适用，只是增加了一些条款，如下：
文件注释：
每个文件的开头都是版权声明，接着是文件内容的描述。
法律声明和作者栏：
每个文件都必须包含如下信息:
本文件内容描述和作者栏
如果你把别人写的文件做了相当大的改动，就把自己添加到作者栏去。这样别的开发者就方便联系这个文件的实际开发人员了。


＊ 声明注释

每个接口，类别，协议都必须伴随描述它的用途以及如何整合的注释。

``` ObjC
	// A delegate for NSApplication to handle notifications about app
	// launch and shutdown. Owned by the main app controller.
	@interface MyAppDelegate : NSObject {
  		...
	}
	@end
```

如果已经在文件的顶部写了接口的详细描述，你也可以简单的写如"见文件顶部的完整描述"，当然要有这些注释的顺序安排。

此外public接口的每个方法都应该添加关于函数，参数，返回值以及副作用的注释。

文档默认类都是同步的，如果类实例可以多线程访问，必须要加上额外的说明。


* 注释内容

使用竖线引用变量或符号，而不是用引号。

这可以减少歧义，特别是当符号本身就是个常见的词时，可能使句子显得支离破碎，比如符号是"count":

``` ObjC
	// Sometimes we need |count| to be less than zero.
```

或者是对于那些已经存在引号的情况：

``` ObjC
	// Remember to call |StringWithoutSpaces("foo bar baz")|
```

Cocoa和Objective-C特性
===
***

* 成员变量应该定义为@private

参考代码：

``` ObjC
	@interface MyClass : NSObject {
 		@private
  		id myInstanceVariable_;
	}
	// public accessors, setter takes ownership
	- (id)myInstanceVariable;
	- (void)setMyInstanceVariable:(id)theVar;
	@end
```

* 明确指定初始化

注释并说明指定的初始化。
明确指定初始化对想要子类化你的类的时候时很重要的。那样，子类化时只需要做一个或多个初始化去保证初值即可。这也有助于在以后调试你的类时明了初始化流程。


* 重写指定初始化

当重写一个子类并需要init...方法，注意要重写父类的指定初始化方法。

如果你没有正确重写父类的指定初始化方法，你的初始化方法可能不会被调用，这会导致很多微妙而难以排除的错误。


* 重写NSObject的方法

强烈建议在@implementation之后就立即重写NSObject 的方法。建议重写 init...，copyWithZone:和 dealloc 方法。init...相关的方法写在一起， 接下来是 copyWithZone: ，最后是 dealloc。


* 避免调用new方法

不要调用NSObject 的new方法，也不要在子类中重写它，而是应该使用 alloc 和 init 方法来初始化retained的对象。

Objective-C代码显式调用 alloc 和 init 方法来创建和retain一个对象。new 的方法可能会带来内存上调试的麻烦。

* 初始化变量

没必要在初始化方法里把变量初始化为0或者nil，这是多余的。

所有新分配内存的对象内容都初始化为0(除了isa)，所以不要在init方法里做无谓的重初始化为0的操作。


* 保持公有API简明

保持你的类简单，如果一个方法没必要公开就不要公开。使用私有类别保证公开头文件的简洁。

和C++不同，Objective-C无法区分公有私有方法，因为它全是公有的。因此，除非就是为了让用户调用所设计，不要把其他的方法放到公有API里。这样可以减少不期调用的可能性。这还包括重写父类的方法。对于那些内部实现的方法，在实现文件里使用类别而不是将方法定义在公有头文件里。

``` ObjC
	// GTMFoo.m
	#import "GTMFoo.h"

	@interface GTMFoo (PrivateDelegateHandling)
	- (NSString *)doSomethingWithDelegate;  // Declare private method
	@end

	@implementation GTMFoo(PrivateDelegateHandling)
	...
	- (NSString *)doSomethingWithDelegate {
  		// Implement this method
	}
	...
	@end
```

* #import和#include


用#import导入Objective-C或Objective-C++头文件，用#include导入C或C++头文件

根据头文件的语言去选择合适的导入方式。

当导入的头文件使用Objective-C或Objective-C++语言时，使用#import。

当导入标准C或C++头文件时，使用#include。头文件应该使用自己的#define重加载保护。

有些Objective-C头文件没有#define重加载保护，所以只应该用#import导入。因此Objective-C头文件只应该被Objective-C源文件或其他的Objective-C头文件所导入。这种情况下全部使用#import是合适的。

标准C和C++头文件不包含任何Objective-C元素都可以被一般的C或C++文件导入。因为标准C和C++里根本没有#import，所以也只能用#include导入。在Objective-C代码中使用#include一致的导入这些头文件。

本条款有助于跨平台项目的无意错误。一位Mac开发者引入一份新C或C++头文件时可能会忘记添加#define重加载保护，因为在Mac上用#import导入文件不会引发问题，但在别的使用#include的平台就可能出问题。在所有平台一致的使用#include意味着要么全部成功要么全部失败，避免了那种一些平台上可以运作而另一些不行的情况。

``` ObjC
	#import <Cocoa/Cocoa.h>
	#include <CoreFoundation/CoreFoundation.h>
	#import "GTMFoo.h"
	#include "base/basictypes.h"
```

* 使用根框架


导入框架根的头文件而不是分别导入框架头文件

看起来从Cocoa或Foundation这些框架里导入个别的文件很不错，但实际上你直接导入框架根头文件效率更高。框架根已经被预编译故可更快的被加载。还有，记住用#import指令而不是#include导入Objective-C的框架。

``` ObjC
	#import <Foundation/Foundation.h>     // good

	#import <Foundation/NSArray.h>        // avoid
	#import <Foundation/NSString.h>
```

* 构建是即设定autorelease

当创建新的临时对象时，在同一行代码里就设定autorelease而不是写到这个方法的后面几行去

即使这样可能会造成一些轻微的延迟，但这样避免了谁不小心把release去掉，或在release之前就return而造成的内存泄露，如下：

``` ObjC
	// AVOID (unless you have a compelling performance reason)
	MyController* controller = [[MyController alloc] init];
	// ... code here that might return ...
	[controller release];

	// BETTER
	MyController* controller = [[[MyController alloc] init] autorelease];
```

* 优先autorelease而非retain

对象赋值时尽量采用autorelease而不是retian模式。

当把一个新创建的对象赋予一个变量的时候，第一件要做的事情就是先释放原来变量指向的对象以防止内存泄露。这里也有很多"正确的"方法去做这件事。我们选择autorelease时因为它更不倾向于出错。小心在密集的循环里可能会很快填满autorelease池，而且它也确实会降低效率，但权衡下来还是可以接受的。

``` ObjC
	- (void)setFoo:(GMFoo *)aFoo {
  		[foo_ autorelease];  // Won't dealloc if |foo_| == |aFoo|
  		foo_ = [aFoo retain];
	}
```

* 以声明时的顺序dealloc处理实例变量

dealloc应该用在@interface声明时同样的顺序处理实例变量，这也有助于评审者鉴别。

代码评审者检查或修正dealloc的实现要确保所有retain的实例变量都获得了释放。

为了简化评审dealloc，将释放retain的实例变量代码保持和@interface里声明的顺序一致。如果dealloc调用了其他方法去释放实例变量，添加注释说明那些实例变量被这些方法所处理了。


* Setters copy NSStrings

在NSString上调用Setters方法时，永远使用copy方式。永远不要retain一个字符串，这可以防止调用者在你不知到的情况下修改了字符串。不要以为你可以改变NSString的值，只有NSMutableString才能做到。

``` ObjC
	- (void)setFoo:(NSString *)aFoo {
  		[foo_ autorelease];
  		foo_ = [aFoo copy];
	}
```

* 避免抛出异常

不要@throwObjective-C的异常，不过你还是要做好准备捕获第三方以及系统调用抛出的异常。

我们的确在编译时加入了-fobjc-exceptions指令(主要是为了获得@synchronized)，但我们并不@throw。当然在使用第三方库的时候是允许使用@try，@catch，以及@finally的。如果你确实使用了，请务必明确到文档中哪个方向你想抛出什么异常。

除非你写的代码想要泡在MacOS10.2或更之前，否则不要使用NS_DURING，NS_HANDLER，NS_ENDHANDLER，NS_VALUERETURNandNS_VOIDRETURN这些宏。

另外你要小心当写Objective-C++代码的时候，如果抛出Objective-C异常，那些栈上的对象不会被清理。示例:

``` ObjC
	class exceptiontest {
 		public:
  		exceptiontest() { NSLog(@"Created"); }
  		~exceptiontest() { NSLog(@"Destroyed"); }
	};

	void foo() {
  		exceptiontest a;
  		NSException *exception = [NSException exceptionWithName:@"foo"
  	                                                     reason:@"bar"
                                                       userInfo:nil];
  		@throw exception;
	}

	int main(int argc, char *argv[]) {
  		GMAutoreleasePool pool;
  		@try {
    		foo();
  		}
  		@catch(NSException *ex) {
    		NSLog(@"exception raised");
  		}
  		return 0;
	}
```

将会有如下输出:

``` bash
	2006-09-28 12:34:29.244 exceptiontest[23661] Created
	2006-09-28 12:34:29.244 exceptiontest[23661] exception raised
```

注意这里的析构函数永远没有机会被调用。这是在你想用栈上的智能指针比如shared_ptr，linked_ptr，还有STL对象的时候不得不关注的一个核心问题。如果你一定要在Objective-C++代码里抛出异常，那就请一定使用C++的异常。永远不要重新抛出一个Objective-C的异常，也不允许在异常块即@try，@catch，@finally里生成栈上的C++对象(比如std::string，std::vector等)。


* nil检查

仅在校验逻辑流程时做nil检查。

使用nil检查不是为了防止程序崩溃，而是校验逻辑流程。向一个空对象发送一条消息是由Objective-C运行时处理的。方法没有返回结果，你也可以安心走下去。

注意这里和C/C++的空指针检查是完全不同的，在那些环境里，并不处理空指针情况并可能导致你的应用程序崩溃。不过你仍要自己确保提领的指针不为空。


* BOOL类型陷阱

整形的转换为BOOL型的时候要小心。不要直接和YES做比较。

BOOL在Objective-C里被定义为unsignedchar，这意味着它不仅仅只有YES(1)和NO(0)两个值。不要直接把整形强制转换为BOOL型。常见的错误发生在把数组大小，指针的值或者逻辑位运算的结果赋值到BOOL型中，而这样就导致BOOL值的仅取决于之前整形值的最后一个字节，有可能出现整形值不为0但被转为NO的情况。应此把整形转为BOOL型的时候请使用ternery操作符，保证返回YES或NO值。

在BOOL，_BOOL以及bool(见C++Std4.7.4，4.12以及C99Std6.3.1.2)之间可以安全的交换值或转型。但BOOL和Boolean之间不可，所以对待Boolean就像上面讲的整形一样就可以了。在Objective-C函数签名里仅使用BOOL。

对BOOL值使用逻辑运算(&&，||，!)都是有效的，返回值也可以安全的转为BOOL型而不需要ternery操作符。

``` ObjC
	- (BOOL)isBold {
  		return [self fontTraits] & NSFontBoldTrait;
	}
	- (BOOL)isValid {
  		return [self stringValue];
	}	
	- (BOOL)isBold {
  		return ([self fontTraits] & NSFontBoldTrait) ? YES : NO;
	}
	- (BOOL)isValid {
  		return [self stringValue] != nil;
	}
	- (BOOL)isEnabled {
  		return [self isValid] && [self isBold];	
	}
```

还有，不要把BOOL型变量直接与YES比较。这样不仅对于精通C的人很有难度，而且此条款的第一点也说明了这样做未必能得到你想要的结果。

``` ObjC
	BOOL great = [foo isGreat];
	if (great == YES)
  	// ...be great!

	BOOL great = [foo isGreat];
	if (great)
	// ...be great!
```

* 属性

属性遵循如下规则:属性是Objective-C2.0的特性，所以只能跑在iPhone以及MacOSX10.5(leopard)或更高的版本。

一个有属性关联实例变量都要在后面加下划线，而该属性的名称就是实例变量不加尾部的下划线的名字。

使用@synthesize标识以正确的重命名属性。

``` ObjC
	@interface MyClass : NSObject {
 		@private
  		NSString *name_;
	}
	@property(copy, nonatomic) NSString *name;
	@end

	@implementation MyClass
	@synthesize name = name_;

	@end
```

属性的声明必须紧接变量申明的括号后。属性的定义应该紧接@implementation模块后面。它和@interface 或者@implementation 的缩进是相同的。

``` ObjC
	@interface MyClass : NSObject {
 		@private
  		NSString *name_;
	}
	@property(copy, nonatomic) NSString *name;
	@end

	@implementation MyClass
	@synthesize name = name_;
	- (id)init {
	...
	}
	@end
```

* 为NSString使用Copy属性

NSString的属性定义为copy。

如果自己实现setters方法，也请使用copy而不是retain。



