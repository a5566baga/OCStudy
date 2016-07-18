# UILabel学习
---
##XCode的页面布局
![布局](界面.png)
##一个应用的声明周期
![声明周期](应用程序生命周期.png)
##继承关系图
![继承](继承关系图.jpeg)
##iOS屏幕坐标系
>iPhone 4 , 4s, 3.5寸
屏幕分辨率  （640 * 960 ）  （0，0） （320，480） 宽320，高480

>iPone5, 5c, 5s, 4寸
屏幕分辨率 （640 * 1136）  （0，0） （320，568）宽 320 高568

>iPhone 6         4.7寸
屏幕分辨率 （750 *1334）   （0，0） （375，667）宽 375   高667

>iPhone 6 Plus  5.5寸
 屏幕分辨率（1242 *2208） （0，0） （414，736） 宽 414 高736
 ##UILable的使用
 ####创建一个label
 ```
 UILabel * label1 = [[UILabel
alloc]initWithFrame:CGRectMake(0, 0, 320, 30)];
 ```
 ####把Label添加到视图
 ```
 [self.view addSubview:label1];
 ```
 ####设置label内容
 ```
 label2.text =@"hi,我是 个标签. ";
 ```
 ####设置内容颜色
 ```
 label2.textColor = [UIColor whiteColor];
 ```
 ####设置标签背景颜色
 ```
 label.backgroundColor = [UIColor
orangeColor];
 ```
 ####设置字体大小
 ```
 label.font = [UIFont systemFontOfSize:40]; label.font = [UIFont boldSystemFontOfSize:20];//加粗
label.font = [UIFont italicSystemFontOfSize:20];//倾斜
 ```
 ####自适应宽度
 ```
 label.adjustsFontSizeToFitWidth = YES;
 ```
 ####