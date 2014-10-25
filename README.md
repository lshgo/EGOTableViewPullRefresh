# Whats different on this fork:
- Easy integration, the table view can be added and configured using interface builder.
- Easy configuration, the arrow image, background color and text color can simply be changed by properties of the PullTableView class.
- *Pull to reload more data* functionality at the bottom of the table.
- Possibility to trigger the *refreshing* and *loading more* animations by code.

# The fast setup:
- Add QuartzCore.framework to your project
- Drag drop EGOTableViewPullRefresh directory to your project.
- Look at the PullTableView.h file for available properties.
- Add a PullTableView to your code and implement the PullTableViewDelegate methods.
- Enjoy!

# The detailed setup (Walk through for creating the demo project):
- Create a new Xcode project
- Choose *View Based Application*
- Product name: EGOTableViewPullRefreshDemo
- Create it in desired folder
- Add *QuartzCore.framework* to the project

**Adding the PullTableView to the project:**

- Drag drop EGOTableViewPullRefresh directory to the Supporting Files group in the project, make sure items are copied into destination groups folder.

**Adding the PullTable to the view `EGOTableViewPullRefreshDemoViewController.xib`:**

- Drag drop a UITableView to the view.
- Open the *Identity inspector* and change the Class from 'UITableView' to PullTableView
- Connect the dataSource and pullDelegate outlets of the PullTableView to File's owner

**Configuring view controller Header `EGOTableViewPullRefreshDemoViewController.h`:**

- Add `#import "PullTableView.h"`
- Make it conform to PullTableViewDelegate and UITableViewDataSource
- Create an outlet property named pullTableView and connect it to the table in interface builder.

**Configuring view controller Footer `EGOTableViewPullRefreshDemoViewController.m`**

- Add the following code to the m file.

        #pragma mark - Refresh and load more methods
        
        - (void) refreshTable
        {
            /*
             
                 Code to actually refresh goes here.
             
             */
            self.pullTableView.pullLastRefreshDate = [NSDate date];
            self.pullTableView.pullTableIsRefreshing = NO;
        }
        
        - (void) loadMoreDataToTable
        {
            /*
             
             Code to actually load more data goes here.
             
             */
            self.pullTableView.pullTableIsLoadingMore = NO;
        }
        
        #pragma mark - UITableViewDataSource
        
        - (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
        {
            return 5;
        }
        
        - (NSInteger) tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
        {
            return 10;
        }
        
        - (UITableViewCell *) tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
        {
            static NSString *cellIdentifier = @"Cell";
            
            UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
            
            if(!cell) {
                cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];
            }
            cell.textLabel.text = [NSString stringWithFormat:@"Row %i", indexPath.row];
            
            return cell;
        }
        
        - (NSString *) tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section
        {
            return [NSString stringWithFormat:@"Section %i begins here!", section];
        }
        
        - (NSString *) tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section
        {
            return [NSString stringWithFormat:@"Section %i ends here!", section];
        }
        
        #pragma mark - PullTableViewDelegate
        
        - (void)pullTableViewDidTriggerRefresh:(PullTableView *)pullTableView
        {
    
            [self performSelector:@selector(refreshTable) withObject:nil afterDelay:3.0f];
        }
        
        - (void)pullTableViewDidTriggerLoadMore:(PullTableView *)pullTableView
        {
            [self performSelector:@selector(loadMoreDataToTable) withObject:nil afterDelay:3.0f];
        }
    
    
- For UI configuration add the following code inside viewDidLoad

        self.pullTableView.pullArrowImage = [UIImage imageNamed:@"blackArrow"];
        self.pullTableView.pullBackgroundColor = [UIColor yellowColor];
        self.pullTableView.pullTextColor = [UIColor blackColor];

- For manually triggering animation use the pullTableIsRefreshing and pullTableIsLoadingMore properties. For example add the following code to viewWillAppear:

        if(!self.pullTableView.pullTableIsRefreshing) {
            self.pullTableView.pullTableIsRefreshing = YES;
            [self performSelector:@selector(refreshTable) withObject:nil afterDelay:3];
        }
        
        
        
        
        
        
        在一个项目开发过程中为了更好的体验经常会用到下拉刷新更新数据，当然也伴随一些上拉加载更多数据的情况；当前比较火的EGOTableViewPullRefresh只实现了下拉功能，而没有上拉的功能。这里介绍一个同时集成下拉刷新和上拉加载更多的类库EGOTableViewPullRefresh
英文原文和类库下载地址:https://github.com/emreberge/EGOTableViewPullRefresh    

附带 Demo效果
  

Whats different on this fork:
容易集成，使用interface builder 添加tableView进行配置。
配置简单, 箭头头像，背景颜色和文本颜色都能通过PullTableView类的属性很容易的更改。  
上拉加载更多数据功能在Table的底部。
可以通过代码修改刷新和加载更多动画。
The fast setup:
添加 QuartzCore.framework 到你的工程中。
将 EGOTableViewPullRefresh 拖到你的工程目录下。
查看 PullTableView.h 文件可用的属性。
添加一个PullTableView 到你代码中，实现PullTableViewDelegate委托方法。
欣赏吧。
The detailed setup (Walk through for creating the demo project):
创建一个新的xcode工程
选择 View Based Application 模板(xcode 4.2以后版本是 Single View Application模板)
工程名字 EGOTableViewPullRefreshDemo
在工程文件下创建EGOTableViewPullRefreshDemoViewController控制器类(Single View Application模板不需这步)
添加 QuartzCore.framework 到工程中
添加 PullTableView 到工程里:
拖拽 EGOTableViewPullRefresh 目录下文件到工程支持的文件组下，确保(EGOTableViewPullRefresh)下文件都拷贝到目标文件组下。 
添加 PullTable 视图到 EGOTableViewPullRefreshDemoViewController.xib上:
拖一个UITableView控件到View视图上.
打开 Identity inspector 将Table 的继承类由  UITableView 改成PullTableView
连接 dataSources数据源和 pullDelegate协议到PullTableView的 File's owner上
配置视图控制器的头文件 EGOTableViewPullRefreshDemoViewController.h:
添加 #import "PullTableView.h"
声明 PullTableViewDelegate 和 UITableViewDataSource协议
创建一个属性名为pullTableView的输出口连接到interface Builder上的tableView上
配置视图控制器和页脚 EGOTableViewPullRefreshDemoViewController.m
在.m文件中添加下面代码

[cpp] view plaincopy
#pragma mark - Refresh and load more methods  
  
- (void) refreshTable  
{  
    /* 
 
         Code to actually refresh goes here.  刷新代码放在这 
 
     */  
    self.pullTableView.pullLastRefreshDate = [NSDate date];  
    self.pullTableView.pullTableIsRefreshing = NO;  
}  
  
- (void) loadMoreDataToTable  
{  
    /* 
 
     Code to actually load more data goes here.  加载更多实现代码放在在这 
 
     */  
    self.pullTableView.pullTableIsLoadingMore = NO;  
}  
  
#pragma mark - UITableViewDataSource  
  
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView  
{  
    return 5;  
}  
  
- (NSInteger) tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section  
{  
    return 10;  
}  
  
- (UITableViewCell *) tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath  
{  
    static NSString *cellIdentifier = @"Cell";  
  
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];  
  
    if(!cell) {  
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];  
    }  
    cell.textLabel.text = [NSString stringWithFormat:@"Row %i", indexPath.row];  
  
    return cell;  
}  
  
- (NSString *) tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section  
{  
    return [NSString stringWithFormat:@"Section %i begins here!", section];  
}  
  
- (NSString *) tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section  
{  
    return [NSString stringWithFormat:@"Section %i ends here!", section];  
}  
  
#pragma mark - PullTableViewDelegate  
  
- (void)pullTableViewDidTriggerRefresh:(PullTableView *)pullTableView  
{  
  
    [self performSelector:@selector(refreshTable) withObject:nil afterDelay:3.0f];  
}  
  
- (void)pullTableViewDidTriggerLoadMore:(PullTableView *)pullTableView  
{  
    [self performSelector:@selector(loadMoreDataToTable) withObject:nil afterDelay:3.0f];  
}  

对于UI的配置，在ViewDidLoad()方法里面添加下面代码(比如 修改刷新和上拉的背景色箭头头像等)
[cpp] view plaincopy
self.pullTableView.pullArrowImage = [UIImage imageNamed:@"blackArrow"];  
self.pullTableView.pullBackgroundColor = [UIColor yellowColor];  
self.pullTableView.pullTextColor = [UIColor blackColor];  

对于手动设置动画可使用 pullTableIsRefreshing 和pullTableIsLoadingMore 属性. 比如在 viewWillAppear:方法里面添加下面的代码

[cpp] view plaincopy
if(!self.pullTableView.pullTableIsRefreshing) {  
    self.pullTableView.pullTableIsRefreshing = YES;  
    [self performSelector:@selector(refreshTable) withObject:nil afterDelay:3];  
}  
