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
######


---

## UISearchBar 和  UISearchDisplayController 的使用