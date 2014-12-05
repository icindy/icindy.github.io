UITableView的数据源(dataSource)和代理(delegate)
 
UITableView需要一个数据源(dataSource)来显示数据，UITableView会向数据源查询一共有多少行数据以及每一行显示什么数据等。没有设置数据源的UITableView只是个空壳。凡是遵守UITableViewDataSource协议的OC对象，都可以是UITableView的数据源。
通常都要为UITableView设置代理对象(delegate)，以便在UITableView触发一下事件时做出相应的处理，比如选中了某一行。凡是遵守了UITableViewDelegate协议的OC对象，都可以是UITableView的代理对象。一般会让控制器充当UITableView的dataSource和delegate
 
UITableViewDataSource
 
@required
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;
第section分区一共有多少行
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath;
创建第section分区第row行的UITableViewCell对象(indexPath包含了section和row)
@optional
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView;
一共有多少个分区
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section;
第section分区的头部标题
- (NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section;
第section分区的底部标题
- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath;
某一行是否可以编辑(删除)
- (BOOL)tableView:(UITableView *)tableView canMoveRowAtIndexPath:(NSIndexPath *)indexPath;
某一行是否可以移动来进行重新排序
- (NSArray *)sectionIndexTitlesForTableView:(UITableView *)tableView;
UITableView右边的索引栏的内容
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
选中了UITableView的某一行
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
某一行的高度
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section
第section分区头部的高度
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section
第section分区尾部的高度
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
第section分区头部显示的视图
- (UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section
第section分区尾部显示的视图
- (NSInteger)tableView:(UITableView *)tableView indentationLevelForRowAtIndexPath:(NSIndexPath *)indexPath
设置每一行的等级缩进(数字越小，等级越高)
 
UITableViewCell
 
UITableView的每一行都是一个UITableViewCell,通过dataSource的
tableView:cellForRowAtIndexPath:方法来初始化每一行
UITableViewCell是UIView的子类，内部有个默认的子视图:contentView。contentView是UITableViewCell所显示内容的父视图，并负责显示一些辅助指示视图。辅助指示视图的作用是显示一个表示动作的图标，可以通过设置UITableViewCell的accessoryType来显示，默认是UITableViewCellAccessoryNone(不显示辅助指示视图)，其他值如下:
UITableViewCellAccessoryDisclosureIndicator 
UITableViewCellAccessoryDetailDisclosureButton
UITableViewCellAccessoryCheckmark
 
UITableViewCell的contentView
 
contentView下默认有3个子视图，其中的2个是UILabel(通过UITableViewCell的textLabel和detailTextLabel属性访问)，第3个是UIImageView(通过UITableViewCell的imageView属性访问)
UITableViewCell还有一个UITableViewCellStyle属性，用于决定使用contentView的哪些子视图，以及这些子视图在contentView中的位置
 
UITableViewCell对象的重用原理
 
iOS设备的内存有限，如果用UITableView显示成千上万条数据，就需要成千上万个UITableViewCell对象的话，那将会耗尽iOS设备的内存。要解决该问题，需要重用UITableViewCell对象
重用原理：当滚动列表时，部分UITableViewCell会移出窗口，UITableView会将窗口外的UITableViewCell放入一个对象池中，等待重用。当UITableView要求dataSource返回UITableViewCell时，dataSource会先查看这个对象池，如果池中有未使用的UITableViewCell，dataSource会用新的数据配置这个UITableViewCell，然后返回给UITableView，重新显示到窗口中，从而避免创建新对象
还有一个非常重要的问题：有时候需要自定义UITableViewCell(用一个子类继承UITableViewCell)，而且每一行用的不一定是同一种UITableViewCell(如短信聊天布局)，所以一个UITableView可能拥有不同类型的UITableViewCell，对象池中也会有很多不同类型的UITableViewCell，那么UITableView在重用UITableViewCell时可能会得到错误类型的UITableViewCell
解决方案：UITableViewCell有个NSString *reuseIdentifier属性，可以在初始化UITableViewCell的时候传入一个特定的字符串标识来设置reuseIdentifier(一般用UITableViewCell的类名)。当UITableView要求dataSource返回UITableViewCell时，先通过一个字符串标识到对象池中查找对应类型的UITableViewCell对象，如果有，就重用，如果没有，就传入这个字符串标识来初始化一个UITableViewCell对象
 
重用UITableViewCell对象
 
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath 
{
    static NSString *identifier = @"UITableViewCell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:identifier];
    if (cell == nil) {
        cell = [[[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:identifier] autorelease];
     }
    cell.textLabel.text = [NSString stringWithFormat:@"Text %i", indexPath.row];
    return cell;
}
 
UITableViewCell的常用属性
 
设置背景
backgroundView
设置被选中时的背景视图
selectedBackgroundView
 
selectionStyle属性可设置UITableViewCell被选中时的背景颜色:
UITableViewCellSelectionStyleNone  没有颜色
UITableViewCellSelectionStyleBlue  蓝色（默认）
UITableViewCellSelectionStyleGray  灰色
 
自定义UITableViewCell
 
一般有两种方式:
用一个xib文件来描述UITableViewCell的内容
通过代码往UITableViewCell的contentView中添加子视图，在初始化方法(比如init、initWithStyle:reuseIdentifier:)中添加子控件，在layoutSubviews方法中分配子控件的位置和大小
 
UITableView的编辑模式
 
UITableView有个editing属性，设置为YES时，可以进入编辑模式。在编辑模式下，可以管理表格中的行，比如改变行的排列顺序、增加行、删除行，但不能修改行的内容
多种方式开启编辑模式
@property(nonatomic,getter=isEditing) BOOL editing
- (void)setEditing:(BOOL)editing animated:(BOOL)animated
 
删除UITableView的行
 
首先要开启编辑模式
实现UITableViewDataSource的如下方法：
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath 
{
    // 如果UITableView提交的是删除指令
    if (editingStyle == UITableViewCellEditingStyleDelete) {
        // 删除真实数据
         // [self.data removeObjectAtIndex:indexPath.row];
        // 删除UITableView中的某一行(带动画效果)
        [tableView deleteRowsAtIndexPaths:[NSArray arrayWithObject:indexPath] withRowAnimation:UITableViewRowAnimationLeft];
        // 如果不考虑动画效果，也可以直接[tableView reload];
    }
}
 
移动UITableView的行
 
首先要开启编辑模式
实现UITableViewDataSource的如下方法(如果没有实现此方法，将无法换行)
- (void)tableView:(UITableView *)tableView moveRowAtIndexPath:(NSIndexPath *)sourceIndexPath toIndexPath:(NSIndexPath *)destinationIndexPath 
{
    int from = sourceIndexPath.row;
    int to = destinationIndexPath.row;
    if (from == to) return;
    // 交换数据
    // [self.data exchangeObjectAtIndex:from withObjectAtIndex:to];
}
 
选中UITableView的行
 
当某行被选中时会调用此方法(UITableViewDelegate的方法)
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath 
{
    //取消选中某一行,让被选中行的高亮颜色消失(带动画效果)
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
}
 
UITableView常用方法
 
- (id)initWithFrame:(CGRect)frame style:(UITableViewStyle)style
初始化一个UITableView，并且设置表格样式
- (void)reloadData   重新访问数据源，刷新界面
- (NSInteger)numberOfSections  分区的个数
- (NSInteger)numberOfRowsInSection:(NSInteger)section
第section分区的行数
- (UITableViewCell *)cellForRowAtIndexPath:(NSIndexPath *)indexPath
通过indexPath找到对应的UITableViewCell对象
- (void)setEditing:(BOOL)editing animated:(BOOL)animated
是否要开启编辑模式
- (void)deselectRowAtIndexPath:(NSIndexPath *)indexPath animated:(BOOL)animated
取消选中某一行,让被选中行的高亮颜色消失(带动画效果)
- (id)dequeueReusableCellWithIdentifier:(NSString *)identifier
通过identifier在(缓存)池中找到对应的UITableViewCell对象
- (void)deleteRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation
移除indexPaths范围内的所有行
@property(nonatomic,readonly) UITableViewStyle style 表格样式
@property(nonatomic,assign) id <UITableViewDataSource> dataSource
数据源
@property(nonatomic,assign) id <UITableViewDelegate> delegate 代理
@property(nonatomic,getter=isEditing) BOOL editing 是否为编辑模式
@property(nonatomic) UITableViewCellSeparatorStyle separatorStyle
设置分隔线的样式
@property(nonatomic,retain) UIColor *separatorColor
设置分隔线的颜色
@property(nonatomic,retain) UIView *tableHeaderView
表头显示的视图
@property(nonatomic,retain) UIView *tableFooterView
表尾显示的视图
@property(nonatomic) BOOL allowsSelection
是否允许选中行
@property(nonatomic) BOOL allowsSelectionDuringEditing
是否允许在编辑模式下选中行
@property(nonatomic) BOOL allowsMultipleSelection
是否允许选中多行
@property(nonatomic) BOOL allowsMultipleSelectionDuringEditing
是否允许在编辑模式下选中多行
 
UITableViewController
 
是UIViewController的子类，UITableViewController默认扮演了3种角色:视图控制器、UITableView的数据源和代理
UITableViewController的view是个UITablView,由UITableViewController负责设置和显示这个对象。UITableViewController对象被创建后，会将这个UITableView对象的dataSource和delegate指向UITableViewController自己
 
 
 
 
一、UITableView
1.数据展示的条件
1> UITableView的所有数据都是由数据源（dataSource）提供的，所以要想在UITableView展示数据，必须设置UITableView的dataSource数据源对象
2> 要想当UITableView的dataSource对象，必须遵守UITableViewDataSource协议，实现相应的数据源方法
3> 当UITableView想要展示数据的时候，就会给数据源发送消息（调用数据源方法），UITableView会根据方法返回值决定展示怎样的数据
2.数据展示的过程
1> 先调用数据源的
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
得知一共有多少组
2> 然后调用数据源的
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
得知第section组一共有多少行
3> 然后调用数据源的
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
得知第indexPath.section组 第indexPath.row 行显示怎样的cell（显示什么内容）
3.常见数据源方法
1> 一共有多少组
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
2> 第section组一共有多少行
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
3> 第indexPath.section组 第indexPath.row行显示怎样的cell（显示什么内容）
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
4> 第section组显示怎样的头部标题
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section;
5> 第section组显示怎样的尾部标题
- (NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section;
4.tableView刷新数据的方式
1> 修改模型数据
2> 刷新表格
* reloadData 整体刷新（每一行都会刷新）
* - (void)reloadRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation
局部刷新
5.性能优化
1> 定义一个循环利用标识
static NSString *ID = @"C1";
2> 从缓存池中取出可循环利用的cell
UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];
3> 如果缓存池中没有可循环利用的cell
if (cell == nil) 
{
    cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:ID];
}
4> 覆盖cell上面的数据
cell.textLabel.text = [NSString stringWithFormat:@"第%d行数据", indexPath.row];
 
 
 
1.UITableView继承UIScrollView.
2.实例一城市列表思路：
步骤一：创建UITableView。UITableView样式为组
步骤二：设置UITableView的数据源方法。
步骤三：实现UITableView的数据源方法，此方法会自动调用。
返回有多少组
返回一组有多少行
返回每一行显示的UITableViewCell(继承UIView),initWithStyle使用这个方法调用。
注意UITableView的数据源的方法普遍都是以tableView开头。
步骤四：用数组管理数据。
步骤五：每个数组中都是一个字典，key:（header,footer,cityes）.
3.扩展性好：指的是需求改了，代码不需要怎么改。
4.创建模型的时候，自定义一个工厂方法（类方法）接口给外界调用。
工厂方法好处：简化对象的实例化，快速创建对象。
5.UITableViewCell
UITableViewCellStyleDefault 
UITableViewCellStyleValue1
UITableViewCellStyleSubtitle
UITableViewCellStyleValue2
6.UITableViewCell设置右边辅助视图accessoryType；
UITableViewCellAccessoryDisclosureIndicator
UITableViewCellAccessoryDetailDisclosureButton
UITableViewCellAccessoryCheckmark
7.快捷创建代码方法中<#   #>用法。
8.中文字前面不要加\，会把\后面的中文转义，正确描述:图书/音像
9.UIAlertView
UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"产品信息展示" message:@"哈哈哈哈哈" delegate:nil cancelButtonTitle:@"ABC" otherButtonTitles:@"123",@"456", nil];
// 弹出UIAlertView
[alert show];
UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"产品信息展示" message:@"哈哈哈哈哈" delegate:nil cancelButtonTitle:@"取消" otherButtonTitles:@"确定", nil];
// 弹出UIAlertView
[alert show];
UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"产品信息展示" message:p.name delegate:nil cancelButtonTitle:@"取消" otherButtonTitles:@"确定", nil];
alert.alertViewStyle = UIAlertViewStyleLoginAndPasswordInput;
// 弹出UIAlertView
[alert show];
UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"产品信息展示" message:p.name delegate:nil cancelButtonTitle:@"取消" otherButtonTitles:@"确定", nil];
alert.alertViewStyle = UIAlertViewStylePlainTextInput;
// 弹出UIAlertView
[alert show];
UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"产品信息展示" message:p.name delegate:nil cancelButtonTitle:@"取消" otherButtonTitles:@"确定", nil];
alert.alertViewStyle = UIAlertViewStyleSecureTextInput;
// 弹出UIAlertView
[alert show];
10.监听UIAlertView的事件.
步骤一：设置UIAlertView的代理
步骤二：遵守UIAlertView的协议
步骤三：实现UIAlertView的按钮点击协议方法。
11．按钮方法内部实现
步骤一： 取出文本框文字 
注意UIAlertView中的文本框的角标是根据UIAlertView从上到下第几个文本框决定的。最上面的文本框角标为0.
步骤二:  修改模型数据
步骤三:  刷新表格
12.UITableView中的reloadData,会重新整个表格。
13.-(void)reloadRowsAtIndexPaths:(NSArray*)indexPaths withRowAnimation:(UITableViewRowAnimation)animation
// 指定刷新indexPaths数组里的行数。
注意indexPaths里存储的是NSIndexPath对象
14.UITableView的性能优化
UITableView默认只会加载出现在屏幕上面的cell，没当有一个cell移除屏幕，就会存储到缓存池里找。
性能优化步骤：
步骤一：定义cell的标识（不需要每次都创建cell标识，因此需要使用static，static标识只会在第一次创建，以后都不会创建了。）
步骤二：从缓存池里取cell
步骤三：判断取出cell是否为空，如果为空就手动创建cell。