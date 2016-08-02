#UITableView学习-02

---

##多选编辑
####需要实现的代理方法
```
-(void)mulDelete:(UIBarButtonItem *)barbutton
```
####需要设置的tableView的属性
```
self.tableView.allowsMultipleSelectionDuringEditing = YES;
```
####删除数据的思路
    1、获取数据源（最好用model）
    2、获取到要删除的行和组
    3、要从序号大的开始删，这样不影响前面序号小的数据
    4、刷新数据页面
```
//删除数据
-(void)deleteData:(UIBarButtonItem *)batbutton{
 NSArray<NSIndexPath *> * pathS = [self.tableView indexPathsForSelectedRows];
// NSLog(@"%ld", pathS.count);
 pathS = [pathS sortedArrayUsingSelector:@selector(compare:)];
// 逆序删除,从序号大的删，这样就不需要对数组进行排序
 for (NSInteger i = pathS.count-1; i >=0 ; i--) {
 [self.dataSource[pathS[i].section] removeObjectAtIndex:pathS[i].row];
 }
// 重新加载一次tableView的数据(方法)
 [self.tableView reloadData];
}
```
####右侧快速定位的索引设置
######常用设置的颜色属性
```
// TODO:sectionIndexColor设置文字颜色
 self.tableView.sectionIndexColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:0.5];
// sectionIndexBackgroundColor设置背景颜色
 self.tableView.sectionIndexBackgroundColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:0.8];
// 轨迹颜色
 self.tableView.sectionIndexTrackingBackgroundColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:0.5];
```
######设置索引的内容
```
-(NSArray<NSString *> *)sectionIndexTitlesForTableView:(UITableView *)tableView{
 return self.sectionTitles;
}
```
    返回值为header的标题的数组
```
-(NSInteger)tableView:(UITableView *)tableView sectionForSectionIndexTitle:(NSString *)title atIndex:(NSInteger)index{
 return index;
}
```
##折叠效果
####思路解析
    1、要对每个section中的数据设置BOOL值标记
    2、在代理方法中动态显示row的数量
    3、自定义headerView，添加Button事件
######设是否要折叠的标识
```
//属性
@property(nonatomic, strong)NSMutableArray * showSection;
//添加BOOL值
[self.showSection addObject:@(YES)];

```
######显示每组的row的数量、分组的数量
```
-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
 if ([self.showSection[section] boolValue]) {
 return [self.dataSource[section] count];
 }else
 return 0;
}
-(NSInteger)numberOfSectionsInTableView:(UITableView *)tableView{
 return self.dataSource.count;
}
```
######设置headerView
    注意，要设定高度，实现代理中的方法
```
-(UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section{
 // 添加一个Button
 UIButton * button = [[UIButton alloc ] init];
 [button setTitle:self.sectionTitles[section] forState:UIControlStateNormal];
 [button setTitleColor:[UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:1] forState:UIControlStateHighlighted];
 button.backgroundColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:1] ;
 button.layer.borderWidth = 3;
 button.layer.cornerRadius = 25;
// button.contentHorizontalAlignment = UIControlContentHorizontalAlignmentCenter;
 [button addTarget:self action:@selector(foldSection:) forControlEvents:UIControlEventTouchUpInside
 ];
 button.tag = 100+section;

 return button;
}
```
```
-(CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section{
 return 50;
}
```
######BUTTON事件响应
```
-(void)foldSection:(UIButton *)button{
// 折叠
 // 1、要获取是哪一个组
 NSInteger section = button.tag-100;
 if ([self.showSection[section] boolValue] == YES) {
 // 2、希望对应的section的行为0
 [self.showSection replaceObjectAtIndex:section withObject:@(NO)];
 }else{
 // 2、希望对应的section的行为0
 [self.showSectiosin replaceObjectAtIndex:section withObject:@(YES)];
 }
 // 3、刷新分区的数据
 [self.tableView reloadSections:[NSIndexSet indexSetWithIndex:section] withRowAnimation:UITableViewRowAnimationTop];

 NSLog(@"");
}
```

####小技巧
######对于默认出现的footer的高度，可以设置它为很小的值
```
-(CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section{
 return 0.01;
}
```
######内容的缩进
```
-(NSInteger)tableView:(UITableView *)tableView indentationLevelForRowAtIndexPath:(NSIndexPath *)indexPath{
 return 3;
}
```
####全部代码
```
#import "ViewController.h"
#import "ImageModel.h"
@interface ViewController ()<UITableViewDelegate, UITableViewDataSource>
@property(nonatomic, strong)UITableView * tableView;
//数据源
@property(nonatomic, strong)NSMutableArray * dataSource;
//sectionTitle
@property(nonatomic, strong)NSArray * sectionTitles;
//是否显示section下的数据
@property(nonatomic, strong)NSMutableArray * showSection;
@end

@implementation ViewController

- (void)viewDidLoad {
 [super viewDidLoad];
 // Do any additional setup after loading the view, typically from a nib.

 self.view.backgroundColor = [UIColor orangeColor];
 self.dataSource = [NSMutableArray array];
 self.showSection = [NSMutableArray array];

// 添加
 [self createBarItem];
// 初始化数据
 [self initForData];
// 创建tableView
 [self createBarItem];

 [self createTableView];
}

#pragma mark
#pragma mark =========== 初始化数据
-(void)initForData{

 _sectionTitles = @[@"圣斗士", @"海贼王", @"火影忍者", @"美女"];

// 读取文件
// 获取工程中所有的plist文件
 NSArray * paths = [[NSBundle mainBundle] pathsForResourcesOfType:@"plist" inDirectory:nil];
 for (NSString * path in paths) {
// 去除info的文件
// lastPathComponent 获取路径最后的内容
 if ([[path lastPathComponent] containsString:@"Info"]) {
 continue;
 }
// 读取文件内容
 NSArray * models = [NSArray arrayWithContentsOfFile:path];
// 把数据封装转换成数据模型
 NSMutableArray * imageInfo = [NSMutableArray array];
 for (NSDictionary * dic in models) {
 ImageModel * model = [[ImageModel alloc] initWithDictionary:dic];
 [imageInfo addObject:model];
 }
// 把某一类数据模型添加到数据源DataSourse
 [self.dataSource addObject:imageInfo];
 [self.showSection addObject:@(YES)];
 }
}

#pragma mark
#pragma mark =============== createTableView
-(void)createTableView{
 _tableView = [[UITableView alloc] initWithFrame:self.view.frame style:UITableViewStyleGrouped];
 _tableView.delegate = self;
 _tableView.dataSource = self;

 [self.tableView setBackgroundView:[[UIImageView alloc] initWithImage:[UIImage imageNamed:@"圣斗士01"]]];

 [self.view addSubview:_tableView];

// 右侧导航栏
// TODO:sectionIndexColor设置文字颜色
 self.tableView.sectionIndexColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:0.5];
// sectionIndexBackgroundColor设置背景颜色
 self.tableView.sectionIndexBackgroundColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:0.8];
// 轨迹颜色
 self.tableView.sectionIndexTrackingBackgroundColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:0.5];


}

#pragma mark
#pragma mark ============ createBarItem
-(void)createBarItem{
 UIBarButtonItem * left = [[UIBarButtonItem alloc] initWithTitle:@"多行删除" style:UIBarButtonItemStylePlain target:self action:@selector(mulDelete:)];
 self.navigationItem.leftBarButtonItem = left;

}

-(void)mulDelete:(UIBarButtonItem *)barbutton{
 static BOOL flg = YES;
 if (flg == YES) {
 barbutton.title = @"完成";
 UIBarButtonItem * deleteItem = [[UIBarButtonItem alloc] initWithTitle:@"删除" style:UIBarButtonItemStylePlain target:self action:@selector(deleteData:)];
 self.navigationItem.leftBarButtonItems = @[barbutton, deleteItem];
// 编辑状态下允许选择多行
// self.tableView.allowsSelection = YES;
// self.tableView.allowsMultipleSelection = YES;


 [self.tableView setEditing:YES animated:YES];
 }else{
 barbutton.title = @"多选删除";
 self.navigationItem.leftBarButtonItems = @[barbutton];
 [self.tableView setEditing:NO animated:YES];
 }
 flg = !flg;
}

//删除数据
-(void)deleteData:(UIBarButtonItem *)batbutton{
 NSArray<NSIndexPath *> * pathS = [self.tableView indexPathsForSelectedRows];
// NSLog(@"%ld", pathS.count);
 pathS = [pathS sortedArrayUsingSelector:@selector(compare:)];
// 逆序删除,从序号大的删，这样就不需要对数组进行排序
 for (NSInteger i = pathS.count-1; i >=0 ; i--) {
 [self.dataSource[pathS[i].section] removeObjectAtIndex:pathS[i].row];
 }
// 重新加载一次tableView的数据(方法)
 [self.tableView reloadData];
}


#pragma mark
#pragma mark ================= UITableViewDataSourseDelegate

//设置sectionIndex的数据
-(NSArray<NSString *> *)sectionIndexTitlesForTableView:(UITableView *)tableView{
 return self.sectionTitles;
}
//设置sectionTitles的index与分区关联起来
-(NSInteger)tableView:(UITableView *)tableView sectionForSectionIndexTitle:(NSString *)title atIndex:(NSInteger)index{
 return index;
}

//通过数据源数组内的个数确定创建每组多少个row
-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
 if ([self.showSection[section] boolValue]) {
 return [self.dataSource[section] count];
 }else
 return 0;
}

-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
 static NSString * identifer = @"cellID";
 UITableViewCell * cell = [tableView dequeueReusableCellWithIdentifier:identifer];
 if (nil == cell) {
 cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:identifer];
 }
 ImageModel * imgInfo = self.dataSource[indexPath.section][indexPath.row];

 cell.textLabel.text = imgInfo.imageInfo;
 cell.detailTextLabel.text = imgInfo.imageName;

 NSArray * names = [imgInfo.imageName componentsSeparatedByString:@"."];

 cell.imageView.image = [UIImage imageWithContentsOfFile:[[NSBundle mainBundle] pathForResource:names[0] ofType:names[1]]];


 return cell;
}
//将要选择row
//-(NSIndexPath *)tableView:(UITableView *)tableView willSelectRowAtIndexPath:(NSIndexPath *)indexPath{
// NSLog(@"willSelectRow %ld %ld", indexPath.section, indexPath.row);
// return indexPath;
//}
//已经选择
-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath{

 NSLog(@"didSelectRow %ld %ld", indexPath.section, indexPath.row);
}
//
//-(NSIndexPath *)tableView:(UITableView *)tableView willDeselectRowAtIndexPath:(NSIndexPath *)indexPath{
// NSLog(@"willDeselectRow %ld %ld", indexPath.section, indexPath.row);
// return indexPath;
//}
//-(void)tableView:(UITableView *)tableView didDeselectRowAtIndexPath:(NSIndexPath *)indexPath{
// NSLog(@"didDeselectRow %ld %ld", indexPath.section, indexPath.row);
//}

//通过数据源内的数组数确定组数
-(NSInteger)numberOfSectionsInTableView:(UITableView *)tableView{
 return self.dataSource.count;
}
- (void)didReceiveMemoryWarning {
 [super didReceiveMemoryWarning];
 // Dispose of any resources that can be recreated.
}

#pragma mark
#pragma mark =============== UITableViewDelegate
-(CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{
 return 40.0;
}
//自定义headerSection
//设置section的view一定要设置高度
-(UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section{
 // 添加一个Button
 UIButton * button = [[UIButton alloc ] init];
 [button setTitle:self.sectionTitles[section] forState:UIControlStateNormal];
 [button setTitleColor:[UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:1] forState:UIControlStateHighlighted];
 button.backgroundColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:1] ;
 button.layer.borderWidth = 3;
 button.layer.cornerRadius = 25;
// button.contentHorizontalAlignment = UIControlContentHorizontalAlignmentCenter;
 [button addTarget:self action:@selector(foldSection:) forControlEvents:UIControlEventTouchUpInside
 ];
 button.tag = 100+section;

 return button;
}

-(void)foldSection:(UIButton *)button{
// 折叠
 // 1、要获取是哪一个组
 NSInteger section = button.tag-100;
 if ([self.showSection[section] boolValue] == YES) {
 // 2、希望对应的section的行为0
 [self.showSection replaceObjectAtIndex:section withObject:@(NO)];
 }else{
 // 2、希望对应的section的行为0
 [self.showSectiosin replaceObjectAtIndex:section withObject:@(YES)];
 }
 // 3、刷新分区的数据
 [self.tableView reloadSections:[NSIndexSet indexSetWithIndex:section] withRowAnimation:UITableViewRowAnimationTop];

 NSLog(@"");
}

-(CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section{
 return 0.01;
}

-(UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section{

 return nil;
}

-(CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section{
 return 50;
}
//设置内容缩进多少
-(NSInteger)tableView:(UITableView *)tableView indentationLevelForRowAtIndexPath:(NSIndexPath *)indexPath{
 return 3;
}
@end
```
---

## UISearchBar 和  UISearchDisplayController 的使用

####功能
    1、实现查找
    2、通过代理实现不同方式的查找
####UISearchBar
#####创建
```
_searchBar = [[UISearchBar alloc] initWithFrame:CGRectMake(0, 0, CGRectGetWidth(self.view.frame), 70)];
```
#####基本设置属性
######提示文字
```
_searchBar.placeholder = @"请输入查询内容";
```
######提示信息
```
_searchBar.prompt = @"提示";
```
######搜索内容（一般不写）
```
_searchBar.text = @"猪猪侠";
```
######搜索分组
```
_searchBar.scopeButtonTitles = @[@"按姓", @"按名"];
```

####UISearchDisplayController
#####创建
```
_searchDisplayController = [[UISearchDisplayController alloc] initWithSearchBar:_searchBar contentsController:self];
```
#####设置代理
######searchDisplayController 的代理
```
 _searchDisplayController.delegate = self;
```
######这两个代理都是UITableViewDelegate
```
 _searchDisplayController.searchResultsDelegate = self;
 _searchDisplayController.searchResultsDataSource = self;
```
#####代理方法
###### 搜索开始状态，要设置搜索的数据内容为YES 
```
- (void) searchDisplayControllerWillBeginSearch:(UISearchDisplayController *)controller{
 self.showFilterData = YES;
 NSLog(@"搜索将要开始");
}
```
###### 搜索结束后。不让搜索的数据内容能够显示 
```
- (void) searchDisplayControllerDidEndSearch:(UISearchDisplayController *)controller{
 self.showFilterData = NO;
 NSLog(@"搜索结束");
}
```
###### 当搜索内容发生变化时，会回调这个方法。 
```
- (BOOL)searchDisplayController:(UISearchDisplayController *)controller shouldReloadTableForSearchString:(nullable NSString *)searchString{
    [self filterDataByTextString:searchString optionScope:[controller.searchBar.scopeButtonTitles         objectAtIndex:controller.searchBar.selectedScopeButtonIndex]];
     return YES;
}
```
###### 当搜索过滤器发生变化时会回调这个方法 
```
- (BOOL)searchDisplayController:(UISearchDisplayController *)controller shouldReloadTableForSearchScope:(NSInteger)searchOption{
 // controller.searchBar.text 是搜索的内容
 // searchOption选中的第几个按钮
 [self filterDataByTextString:controller.searchBar.text optionScope:[controller.searchBar.scopeButtonTitles objectAtIndex:searchOption]];
 NSLog(@"%s", __func__);
// NSLog(@"%ld", searchOption);
 return YES;
}
```
######核心搜索代码
    1、根据不同的类型，选不同的查询方式
    2、通过匹配字符串来返回类型
```
-(void)filterDataByTextString:(NSString *)text optionScope:(NSString *)scopeTitle{
// 先删除全部的查找出来的元素
 [self.filterArray removeAllObjects];
// TODO:两种查找方式
 if ([scopeTitle isEqualToString:@"按姓"]) {
 for (NSDictionary * dic in self.dataSource) {
 if ([dic[@"姓"] containsString:text]) {
 [self.filterArray addObject:dic];
 }
 }
 }else{
 for (NSDictionary * dic in self.dataSource) {
 if ([dic[@"名"] containsString:text]) {
 [self.filterArray addObject:dic];
 }
 }
 }

}
```
######数据的显示
    1、自己设定的tableView和搜索出来的tableView不是同一个view
    2、通过判断是哪个tableView，之后确认上面的数据
```
-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
 static NSString * identifer = @"cellID";
 UITableViewCell * cell = [tableView dequeueReusableCellWithIdentifier:identifer];

// searchResultTableView是获取显示搜索结果的tableView
// 通过不同的tableView的地址确定显示的该是哪一个tableView
 if (tableView == self.searchDisplayController.searchResultsTableView) {
 if (cell == nil) {
     cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleValue1 reuseIdentifier:identifer];
 }
 cell.textLabel.text = self.filterArray[indexPath.row][@"姓"];
 cell.detailTextLabel.text = self.filterArray[indexPath.row][@"名"];
 }else{
     if (cell == nil) {
         cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleValue2 reuseIdentifier:identifer];
 }

 cell.textLabel.text = self.dataSource[indexPath.row][@"姓"];
 cell.detailTextLabel.text = self.dataSource[indexPath.row][@"名"];
 }

 return cell;
}
```
######通过不同的数组中的数据个数获取ROWS
```
-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
 if (self.showFilterData) {
// 返回的rows的数量
     return self.filterArray.count;
 }else
     return self.dataSource.count;
}
```