---
layout: post
title:  ReactiveCocoa 1-官方readme文档翻译
date:   2014-12-01 23:33:36
categories: ReactiveCocoa
tag: ios
---

前言
---
>	说起ReactiveCocoa，必须要先了解mvvm模式，与常规的mvc模式的区别
>	但是说的很简单，看了两天后，感觉大概懂了。但是还是不会用。
>	所以开始前，我先尝试翻译下github上ReactiveCocoa的readme。
>	英语能力不好，请大家多包涵。

原文地址
---
[ReactiveCocoa readme原文地址](https://github.com/ReactiveCocoa/ReactiveCocoa#reactivecocoa)

开始翻译
---

# ReactiveCocoa

ReactiveCocoa (RAC)是由 [函数式响应编程](http://en.wikipedia.org/wiki/Functional_reactive_programming) 灵感而来的ob-c框架. 框架为**合成和转化价值流**提供API接口。

如果你已经熟悉了函数式响应编程或者了解了基本的ReactiveCocoa理论，请下载 [文档](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/master/Documentation) 文件夹下的预览和了解更多实际应用中的深入解析。 

## 发现新的ReactiveCocoa?

ReactiveCocoa被疯狂的记录，并且这里有丰富的介绍材料来解释你RAC是什么以及你如何使用它。

如果你想了解更多，我们建议阅读下面几条：

 1. [简介](#简介)
 1. [使用场景](#使用场景)
 1. [框架预览](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/master/Documentation/FrameworkOverview.md)
 1. [基本操作](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/master/Documentation/BasicOperators.md)
 1. [头文件](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/master/ReactiveCocoaFramework/ReactiveCocoa)
 1. 以前的回答 [Stack Overflow](https://github.com/ReactiveCocoa/ReactiveCocoa/wiki)
    问题 和 [GitHub issues](https://github.com/ReactiveCocoa/ReactiveCocoa/issues?labels=question&state=closed)
 1. 最近 [文档](https://github.com/ReactiveCocoa/ReactiveCocoa/tree/master/Documentation)
 1. [ios函数式响应编程](https://leanpub.com/iosfrp/) 
    (eBook)
 
如果你有任何问题欢迎提问：[file an issue](https://github.com/ReactiveCocoa/ReactiveCocoa/issues/new). 

## 简介

ReactiveCocoa是由[函数式响应编程](http://blog.maybeapps.com/post/42894317939/input-and-output)灵感而来.
RAC提供了信号量的概念用于捕捉当前和未来变量值的变化，而不是使用手动替换或者修改变量时再对应修改对应的变量值。

RAC不需要通过代码去持续的观察和更新变量值，而是通过一系列的链接(chaining)，结合(combining)，和响应信号(sigles)去重写和声明变量。

	例如：一个UITextField被声明后，一旦它发生改变，RAC不会通过传统的每时每刻检测和更新UITextField，而是通过类似KVO的方式，使用blocks的方式取代传统的`-observeValueForKeyPath:ofObject:change:context:`方式。

信号也可以表示异步操作，就像[futures and promises](http://en.wikipedia.org/wiki/Futures_and_promises)。这大大简化了异步软件，包括网络代码。
一个RAC的主要优点是它提供了一个单一的，统一的方法来处理异步行为，包括委托的回调方法，块，目标的行动机制，通知，并保持程序通畅。


下面时一个例子:

{% highlight objc %}
// 当self.username发生变化, 打印变化信息到控制台.
//
// RACObserve(self, username)建立一个信号，不论self.username任何时候发生变化时，都会传递回来.
// 每当信号发送一个值 -subscribeNext: 将执行block解析这个值.
[RACObserve(self, username) subscribeNext:^(NSString *newName) {
	NSLog(@"%@", newName);
}];
{% endhighlight %}

但是不像KVO notifications，信号可以串联在一块执行：

{% highlight objc %}
// 只有开头有"j"是才打印
//
// -filter returns a new RACSignal that only sends a new value when its block
// returns YES.
[[RACObserve(self, username)
	filter:^(NSString *newName) {
		return [newName hasPrefix:@"j"];
	}]
	subscribeNext:^(NSString *newName) {
		NSLog(@"%@", newName);
	}];
{% endhighlight %}


信号也可以是用来推导出状态。不需要不断的观察和另外验证信息，RAC可以用于输出信号和对信号进行验证操作：

{% highlight objc %}
//创建一个单向针对self.createEnabled的绑定，如果两次密码一致时，返回true
//
// RAC() 很擅长让绑定结合看起来更好
// 
// +combineLatest:reduce: 可以针对不定时变更的信号量数组进行处理，并返回一个block处理过的新的信号量

RAC(self, createEnabled) = [RACSignal 
	combineLatest:@[ RACObserve(self, password), RACObserve(self, passwordConfirmation) ] 
	reduce:^(NSString *password, NSString *passwordConfirm) {
		return @([passwordConfirm isEqualToString:password]);
	}];
{% endhighlight %}

信号量可以建立随时间变化的变量值，不仅仅针对KVO,例如：

{% highlight objc %}
// Logs a message whenever the button is pressed.
//
// RACCommand creates signals to represent UI actions. Each signal can
// represent a button press, for example, and have additional work associated
// with it.
//
// -rac_command is an addition to NSButton. The button will send itself on that
// command whenever it's pressed.
self.button.rac_command = [[RACCommand alloc] initWithSignalBlock:^(id _) {
	NSLog(@"button was pressed!");
	return [RACSignal empty];
}];
{% endhighlight %}

或者是针对异步的网络请求：

{% highlight objc %}
// Hooks up a "Log in" button to log in over the network.
//
// This block will be run whenever the login command is executed, starting
// the login process.
self.loginCommand = [[RACCommand alloc] initWithSignalBlock:^(id sender) {
	// The hypothetical -logIn method returns a signal that sends a value when
	// the network request finishes.
	return [client logIn];
}];

// -executionSignals returns a signal that includes the signals returned from
// the above block, one for each time the command is executed.
[self.loginCommand.executionSignals subscribeNext:^(RACSignal *loginSignal) {
	// Log a message whenever we log in successfully.
	[loginSignal subscribeCompleted:^{
		NSLog(@"Logged in successfully!");
	}];
}];

// Executes the login command when the button is pressed.
self.loginButton.rac_command = self.loginCommand;
{% endhighlight %}

信号也可以代表其他用户界面事件，定时器，或随时间改变的任何事情。

使用异步的信号操作可以通过链式和转换信号建立更加灵活的行为。可以让一系列操作完成后更加容易出发后续操作：


{% highlight objc %}
// Performs 2 network operations and logs a message to the console when they are
// both completed.
//
// +merge: takes an array of signals and returns a new RACSignal that passes
// through the values of all of the signals and completes when all of the
// signals complete.
//
// -subscribeCompleted: will execute the block when the signal completes.
[[RACSignal 
	merge:@[ [client fetchUserRepos], [client fetchOrgRepos] ]] 
	subscribeCompleted:^{
		NSLog(@"They're both done!");
	}];
{% endhighlight %}

信号可以被顺序执行异步操作，而不是嵌套使用块.这类似于[futures and promises](http://en.wikipedia.org/wiki/Futures_and_promises)工作机制：

{% highlight objc %}
// Logs in the user, then loads any cached messages, then fetches the remaining
// messages from the server. After that's all done, logs a message to the
// console.
//
// The hypothetical -logInUser methods returns a signal that completes after
// logging in.
//
// -flattenMap: will execute its block whenever the signal sends a value, and
// returns a new RACSignal that merges all of the signals returned from the block
// into a single signal.
[[[[client 
	logInUser] 
	flattenMap:^(User *user) {
		// Return a signal that loads cached messages for the user.
		return [client loadCachedMessagesForUser:user];
	}]
	flattenMap:^(NSArray *messages) {
		// Return a signal that fetches any remaining messages.
		return [client fetchMessagesAfterMessage:messages.lastObject];
	}]
	subscribeNext:^(NSArray *newMessages) {
		NSLog(@"New messages: %@", newMessages);
	} completed:^{
		NSLog(@"Fetched all messages.");
	}];
{% endhighlight %}

RAC甚至可以使异步操作的结果更容易被绑定：

{% highlight objc %}
// Creates a one-way binding so that self.imageView.image will be set the user's
// avatar as soon as it's downloaded.
//
// The hypothetical -fetchUserWithUsername: method returns a signal which sends
// the user.
//
// -deliverOn: creates new signals that will do their work on other queues. In
// this example, it's used to move work to a background queue and then back to the main thread.
//
// -map: calls its block with each user that's fetched and returns a new
// RACSignal that sends values returned from the block.
RAC(self.imageView, image) = [[[[client 
	fetchUserWithUsername:@"joshaber"]
	deliverOn:[RACScheduler scheduler]]
	map:^(User *user) {
		// Download the avatar (this is done on a background queue).
		return [[NSImage alloc] initWithContentsOfURL:user.avatarURL];
	}]
	// Now the assignment will be done on the main thread.
	deliverOn:RACScheduler.mainThreadScheduler];
{% endhighlight %}

That demonstrates some of what RAC can do, but it doesn't demonstrate why RAC is
so powerful. It's hard to appreciate RAC from README-sized examples, but it
makes it possible to write code with less state, less boilerplate, better code
locality, and better expression of intent.

这表明，RAC可以做些什么，但这并不说明为什么RAC如此强大。欣赏RAC自述文件大小的例子，是很难的，但它可以用较少的状态，写更少的代码模板，更好的代码的地方，和更好的表达意图。

寻找更多的实际代码，可以查看 [C-41](https://github.com/AshFurrow/C-41) 或者 [GroceryList](https://github.com/jspahrsummers/GroceryList),他们都是使用ReactiveCocoa编写的ios app。


## 使用场景


乍一看，reactivecocoa是很抽象的，它可能很难了解如何将它应用到具体的问题。

下面使可以使用RAC的场景展示：

### 处理异步或事件驱动的数据源

Cocoa编程的重点是用户的事件或变化的反应应用程序状态。这样的事件处理代码就会变得很复杂的并且代码很冗长，用回调和状态变量的大量处理排序问题。

看起来不同的模式，如UI回调，网络响应，而KVO通知，实际上有很多共同之处。[racsignal](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/master/ReactiveCocoaFramework/ReactiveCocoa/RACSignal.h)结合所有这些不同的API，这样他们可以组合在一起，相同的方式操纵。

例如下面的代码

{% highlight objc %}

static void *ObservationContext = &ObservationContext;

- (void)viewDidLoad {
	[super viewDidLoad];

	[LoginManager.sharedManager addObserver:self forKeyPath:@"loggingIn" options:NSKeyValueObservingOptionInitial context:&ObservationContext];
	[NSNotificationCenter.defaultCenter addObserver:self selector:@selector(loggedOut:) name:UserDidLogOutNotification object:LoginManager.sharedManager];

	[self.usernameTextField addTarget:self action:@selector(updateLogInButton) forControlEvents:UIControlEventEditingChanged];
	[self.passwordTextField addTarget:self action:@selector(updateLogInButton) forControlEvents:UIControlEventEditingChanged];
	[self.logInButton addTarget:self action:@selector(logInPressed:) forControlEvents:UIControlEventTouchUpInside];
}

- (void)dealloc {
	[LoginManager.sharedManager removeObserver:self forKeyPath:@"loggingIn" context:ObservationContext];
	[NSNotificationCenter.defaultCenter removeObserver:self];
}

- (void)updateLogInButton {
	BOOL textFieldsNonEmpty = self.usernameTextField.text.length > 0 && self.passwordTextField.text.length > 0;
	BOOL readyToLogIn = !LoginManager.sharedManager.isLoggingIn && !self.loggedIn;
	self.logInButton.enabled = textFieldsNonEmpty && readyToLogIn;
}

- (IBAction)logInPressed:(UIButton *)sender {
	[[LoginManager sharedManager]
		logInWithUsername:self.usernameTextField.text
		password:self.passwordTextField.text
		success:^{
			self.loggedIn = YES;
		} failure:^(NSError *error) {
			[self presentError:error];
		}];
}

- (void)loggedOut:(NSNotification *)notification {
	self.loggedIn = NO;
}

- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context {
	if (context == ObservationContext) {
		[self updateLogInButton];
	} else {
		[super observeValueForKeyPath:keyPath ofObject:object change:change context:context];
	}
}
{% endhighlight %}

… 转换成RAC写法：

{% highlight objc %}
- (void)viewDidLoad {
	[super viewDidLoad];

	@weakify(self);

	RAC(self.logInButton, enabled) = [RACSignal
		combineLatest:@[
			self.usernameTextField.rac_textSignal,
			self.passwordTextField.rac_textSignal,
			RACObserve(LoginManager.sharedManager, loggingIn),
			RACObserve(self, loggedIn)
		] reduce:^(NSString *username, NSString *password, NSNumber *loggingIn, NSNumber *loggedIn) {
			return @(username.length > 0 && password.length > 0 && !loggingIn.boolValue && !loggedIn.boolValue);
		}];

	[[self.logInButton rac_signalForControlEvents:UIControlEventTouchUpInside] subscribeNext:^(UIButton *sender) {
		@strongify(self);

		RACSignal *loginSignal = [LoginManager.sharedManager
			logInWithUsername:self.usernameTextField.text
			password:self.passwordTextField.text];

			[loginSignal subscribeError:^(NSError *error) {
				@strongify(self);
				[self presentError:error];
			} completed:^{
				@strongify(self);
				self.loggedIn = YES;
			}];
	}];

	RAC(self, loggedIn) = [[NSNotificationCenter.defaultCenter
		rac_addObserverForName:UserDidLogOutNotification object:nil]
		mapReplace:@NO];
}
{% endhighlight %}

### 链接依赖操作

相关依赖往往是在网络请求时体现，当下一步网络请求执行时，需要上一步的结构才可以，这样的例子还有很多：

{% highlight objc %}
[client logInWithSuccess:^{
	[client loadCachedMessagesWithSuccess:^(NSArray *messages) {
		[client fetchMessagesAfterMessage:messages.lastObject success:^(NSArray *nextMessages) {
			NSLog(@"Fetched all messages.");
		} failure:^(NSError *error) {
			[self presentError:error];
		}];
	} failure:^(NSError *error) {
		[self presentError:error];
	}];
} failure:^(NSError *error) {
	[self presentError:error];
}];
{% endhighlight %}

ReactiveCocoa让这种模式变得简单:

{% highlight objc %}
[[[[client logIn]
	then:^{
		return [client loadCachedMessages];
	}]
	flattenMap:^(NSArray *messages) {
		return [client fetchMessagesAfterMessage:messages.lastObject];
	}]
	subscribeError:^(NSError *error) {
		[self presentError:error];
	} completed:^{
		NSLog(@"Fetched all messages.");
	}];
{% endhighlight %}

### 并行独立工作


与独立的数据集的并行工作，然后把它们合并成一个最终的结果，往往涉及到很多的同步：

{% highlight objc %}
__block NSArray *databaseObjects;
__block NSArray *fileContents;
 
NSOperationQueue *backgroundQueue = [[NSOperationQueue alloc] init];
NSBlockOperation *databaseOperation = [NSBlockOperation blockOperationWithBlock:^{
	databaseObjects = [databaseClient fetchObjectsMatchingPredicate:predicate];
}];

NSBlockOperation *filesOperation = [NSBlockOperation blockOperationWithBlock:^{
	NSMutableArray *filesInProgress = [NSMutableArray array];
	for (NSString *path in files) {
		[filesInProgress addObject:[NSData dataWithContentsOfFile:path]];
	}

	fileContents = [filesInProgress copy];
}];
 
NSBlockOperation *finishOperation = [NSBlockOperation blockOperationWithBlock:^{
	[self finishProcessingDatabaseObjects:databaseObjects fileContents:fileContents];
	NSLog(@"Done processing");
}];
 
[finishOperation addDependency:databaseOperation];
[finishOperation addDependency:filesOperation];
[backgroundQueue addOperation:databaseOperation];
[backgroundQueue addOperation:filesOperation];
[backgroundQueue addOperation:finishOperation];
{% endhighlight %}

上面的代码可以清理和通过简单的构成信号优化：

{% highlight objc %}
RACSignal *databaseSignal = [[databaseClient
	fetchObjectsMatchingPredicate:predicate]
	subscribeOn:[RACScheduler scheduler]];

RACSignal *fileSignal = [RACSignal startEagerlyWithScheduler:[RACScheduler scheduler] block:^(id<RACSubscriber> subscriber) {
	NSMutableArray *filesInProgress = [NSMutableArray array];
	for (NSString *path in files) {
		[filesInProgress addObject:[NSData dataWithContentsOfFile:path]];
	}

	[subscriber sendNext:[filesInProgress copy]];
	[subscriber sendCompleted];
}];

[[RACSignal
	combineLatest:@[ databaseSignal, fileSignal ]
	reduce:^ id (NSArray *databaseObjects, NSArray *fileContents) {
		[self finishProcessingDatabaseObjects:databaseObjects fileContents:fileContents];
		return nil;
	}]
	subscribeCompleted:^{
		NSLog(@"Done processing");
	}];
{% endhighlight %}

### 简化采集转换

Higher-order functions like `map`, `filter`, `fold`/`reduce` are sorely missing
from Foundation, leading to loop-focused code like this:

高阶函数像`地图`，`滤波器`，`褶皱` / `减少`非常缺少基础，导致环聚焦这样的代码：

{% highlight objc %}
NSMutableArray *results = [NSMutableArray array];
for (NSString *str in strings) {
	if (str.length < 2) {
		continue;
	}

	NSString *newString = [str stringByAppendingString:@"foobar"];
	[results addObject:newString];
}
{% endhighlight %}

[RACSequence][] 允许任何Cocoa collection在一个统一的声明的方式操纵：

{% highlight objc %}
RACSequence *results = [[strings.rac_sequence
	filter:^ BOOL (NSString *str) {
		return str.length >= 2;
	}]
	map:^(NSString *str) {
		return [str stringByAppendingString:@"foobar"];
	}];
{% endhighlight %}

## System Requirements(不翻译)

ReactiveCocoa supports OS X 10.7+ and iOS 5.0+.

## Importing ReactiveCocoa(不翻译)

To add RAC to your application:

 1. Add the ReactiveCocoa repository as a submodule of your application's
    repository.
 1. Run `script/bootstrap` from within the ReactiveCocoa folder.
 1. Drag and drop `ReactiveCocoaFramework/ReactiveCocoa.xcodeproj` into your
    application's Xcode project or workspace.
 1. On the "Build Phases" tab of your application target, add RAC to the "Link
    Binary With Libraries" phase.
    * **On iOS**, add `libReactiveCocoa-iOS.a`.
    * **On OS X**, add `ReactiveCocoa.framework`. RAC must also be added to any
      "Copy Frameworks" build phase. If you don't already have one, simply add
      a "Copy Files" build phase and target the "Frameworks" destination.
 1. Add `"$(BUILD_ROOT)/../IntermediateBuildFilesPath/UninstalledProducts/include"
    $(inherited)` to the "Header Search Paths" build setting (this is only
    necessary for archive builds, but it has no negative effect otherwise).
 1. **For iOS targets**, add `-ObjC` to the "Other Linker Flags" build setting.
 1. **If you added RAC to a project (not a workspace)**, you will also need to
    add the appropriate RAC target to the "Target Dependencies" of your
    application.

If you would prefer to use [CocoaPods](http://cocoapods.org), there are some
[ReactiveCocoa
podspecs](https://github.com/CocoaPods/Specs/tree/master/Specs/ReactiveCocoa) that
have been generously contributed by third parties.

To see a project already set up with RAC, check out [C-41][] or [GroceryList][],
which are real iOS apps written using ReactiveCocoa.

## Standalone Development

If you’re working on RAC in isolation instead of integrating it into another project, you’ll want to open `ReactiveCocoaFramework/ReactiveCocoa.xcworkspace` and not the `.xcodeproj`.

## 参考文献

ReactiveCocoa is based on .NET's [Reactive
Extensions](http://msdn.microsoft.com/en-us/data/gg577609) (Rx). Most of the
principles of Rx apply to RAC as well. There are some really good Rx resources
out there:

* [Reactive Extensions MSDN entry](http://msdn.microsoft.com/en-us/library/hh242985.aspx)
* [Reactive Extensions for .NET Introduction](http://leecampbell.blogspot.com/2010/08/reactive-extensions-for-net.html)
* [Rx - Channel 9 videos](http://channel9.msdn.com/tags/Rx/)
* [Reactive Extensions wiki](http://rxwiki.wikidot.com/)
* [101 Rx Samples](http://rxwiki.wikidot.com/101samples)
* [Programming Reactive Extensions and LINQ](http://www.amazon.com/Programming-Reactive-Extensions-Jesse-Liberty/dp/1430237473)

RAC and Rx are both frameworks inspired by functional reactive programming. Here 
are some resources related to FRP:

* [What is FRP? - Elm Language](http://elm-lang.org/learn/What-is-FRP.elm)
* [What is Functional Reactive Programming - Stack Overflow](http://stackoverflow.com/questions/1028250/what-is-functional-reactive-programming/1030631#1030631)
* [Specification for a Functional Reactive Language - Stack Overflow](http://stackoverflow.com/questions/5875929/specification-for-a-functional-reactive-programming-language#5878525)
* [Escape from Callback Hell](http://elm-lang.org/learn/Escape-from-Callback-Hell.elm)
* [Principles of Reactive Programming on Coursera](https://www.coursera.org/course/reactive)

[Basic Operators]: Documentation/BasicOperators.md
[Documentation]: Documentation
[Framework Overview]: Documentation/FrameworkOverview.md
[Functional Reactive Programming]: http://en.wikipedia.org/wiki/Functional_reactive_programming
[GroceryList]:  https://github.com/jspahrsummers/GroceryList
[Memory Management]: Documentation/MemoryManagement.md
[NSObject+RACLifting]: ReactiveCocoaFramework/ReactiveCocoa/NSObject+RACLifting.h
[RACDisposable]: ReactiveCocoaFramework/ReactiveCocoa/RACDisposable.h
[RACEvent]: ReactiveCocoaFramework/ReactiveCocoa/RACEvent.h
[RACMulticastConnection]: ReactiveCocoaFramework/ReactiveCocoa/RACMulticastConnection.h
[RACScheduler]: ReactiveCocoaFramework/ReactiveCocoa/RACScheduler.h
[RACSequence]: ReactiveCocoaFramework/ReactiveCocoa/RACSequence.h
[RACSignal+Operations]: ReactiveCocoaFramework/ReactiveCocoa/RACSignal+Operations.h
[RACSignal]: ReactiveCocoaFramework/ReactiveCocoa/RACSignal.h
[RACStream]: ReactiveCocoaFramework/ReactiveCocoa/RACStream.h
[RACSubscriber]: ReactiveCocoaFramework/ReactiveCocoa/RACSubscriber.h
[RAC]: ReactiveCocoaFramework/ReactiveCocoa/RACSubscriptingAssignmentTrampoline.h
[futures and promises]: http://en.wikipedia.org/wiki/Futures_and_promises
[C-41]: https://github.com/AshFurrow/C-41
