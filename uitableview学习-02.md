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
##折叠效果

---

## UISearchBar 和  UISearchDisplayController 的使用