#UITableView的自定义

---
##MVC模式下的自定义
    1、创建model层(放创建的模型层)
    2、创建controller层(控制层)
    3、创建view层(显示层)
    4、创建source层(放图片、plist文件)
    5、Supporting Files (系统自带目录，放一些其它的内容)


###model层的设置
######1、通过对要解析的内容设置属性值
######2、-(instancetype)initWithDictionary:(NSDictionary *)dic;这个方法，把对应的值附到属性上。通过[self setValuesForKeysWithDictionary:dic];这个方法完成赋值，然后再把对象传到数组中去。
######3、如果要是找不到对应的key，需要用系统的方法进行来判断
```
//找不到则执行这个方法
-(void)setValue:(id)value forUndefinedKey:(NSString *)key{
// 可以对对象找不到的key进行一定的处理
}

```
```
//对找的到的属性的key进行处理，如果是空实现，那么所有的属性也无法赋值
-(void)setValue:(id)value forKey:(NSString *)key{
    如果是数组的话就在这里赋值
}
```
###Controller层
######1、控制层，主要是和model层与view层的联系
######2、完成数据初始化和界面上数据显示的赋值
```
#import "ViewController.h"

#import "Book.h"

#import "BookCell.h"

#import "BookCell2.h"



@interface ViewController ()<UITableViewDelegate, UITableViewDataSource>



@property(nonatomic, strong)UITableView * tableView;

@property(nonatomic, strong)NSMutableArray<Book *> * dataModels;



@end



@implementation ViewController



- (void)viewDidLoad {

 [super viewDidLoad];

 // Do any additional setup after loading the view, typically from a nib.

 self.title = @"自定义cell";

 [self initForData];

 [self initForView];

}

- (IBAction)jumpSwip:(id)sender {



}



-(void)initForData{

 _dataModels = [NSMutableArray array];



 NSString * path = [[NSBundle mainBundle] pathForResource:@"bookData" ofType:@"plist"];

 NSArray * array = [NSArray arrayWithContentsOfFile:path];

 for (NSDictionary * dic in array) {

 Book * bookModel = [[Book alloc] initWithDictionary:dic];

 [_dataModels addObject:bookModel];

 }



}



-(void)initForView{

 _tableView = [[UITableView alloc] initWithFrame:[UIScreen mainScreen].bounds style:UITableViewStylePlain];



// 注册cell

 [self.tableView registerNib:[UINib nibWithNibName:@"BookCell2" bundle:nil] forCellReuseIdentifier:@"xibCell"];





 _tableView.delegate = self;

 _tableView.dataSource = self;

 [self.view addSubview:_tableView];

 self.tableView.separatorStyle = UITableViewCellSeparatorStyleNone;





}



#pragma mark

#pragma mark =============== delegate

-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{

 return _dataModels.count;

}



-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{

#if 1

 static NSString * identifer = @"cellID";

 BookCell * cell = [tableView dequeueReusableCellWithIdentifier:identifer];

 if (nil == cell) {

 cell = [[BookCell alloc] initWithStyle:UITableViewCellStyleValue1 reuseIdentifier:identifer];

 }



 cell.book = self.dataModels[indexPath.row];

//#else

// BookCell2 * cell = [tableView dequeueReusableCellWithIdentifier:@"xibCell"];

// cell.model = self.dataModels[indexPath.row];

#endif

 return cell;

}



#if 1



-(CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{

 return [self rowHeightByString:self.dataModels[indexPath.row].detail font:[UIFont systemFontOfSize:20]] + 80;

// return 100;

}



-(float)rowHeightByString:(NSString *)content font:(UIFont *)font{

 CGSize mySize = CGSizeMake(CGRectGetWidth(self.view.frame), CGFLOAT_MAX);

 CGSize size = [content boundingRectWithSize:mySize options:NSStringDrawingUsesLineFragmentOrigin attributes:@{NSFontAttributeName:[UIFont systemFontOfSize:20]} context:nil].size;

 return size.height;

}

#endif

- (void)didReceiveMemoryWarning {

 [super didReceiveMemoryWarning];

 // Dispose of any resources that can be recreated.

}



@end

```
