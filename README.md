### 好客租房 App 开发

### 练习 dart 语言的插件

下载安装dart sdk(网址:http://www.cndartlang.com/920.html),下载vscode插件(code runner),dart 文件右键运行即可.

#### 开始项目创建

项目【菜单】— 【查看】—【命令面板】— 【Flutter：New Project】
测试页面代码:lib/main.dart

```js
import 'package:flutter/material.dart';
void main ()=>runApp(MyApp());
class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context){
    return MaterialApp(
      home:Scaffold(
        appBar: AppBar(
          title:const Text("Home")
        ),
      )
    );
  }
}
```

运行命令,在终端按 r 会自动刷新模拟器界面.

```js
flutter run
```

### 编写一个简单页面-实现

**效果：**

<img src="https://tva1.sinaimg.cn/large/006y8mN6ly1g7rz6g19lqj30dx0pkq5e.jpg" alt="image-20191009152217329" style="zoom: 50%;" />

**步骤：**

1. 添加 PageContent 组件(安装 Awesome Flutter Snippets
   插件有快捷键)
   1. 新建文件 /widgets/page_content.dart
   2. 添加 material 依赖;//importM
   3. 编写无状态组件//StatelessW
   4. 添加 name 参数 //起类名 PageContent ,声明 name:final String name;点击快速修复生成构造函数
   5. **使用 Scaffold** //返回一个 Scaffold
2. 添加 HomePage 页面
   1. 新建文件 /pages/home/index.dart
   2. 添加 material, page_content 依赖//import 'package:first_flutter_app/widgets/page_content.dart';
   3. 编写无状态组件
   4. 使用 PageContent
3. 添加 Application 应用根组件
   1. 新建文件 /application.dart
   2. 添加依赖和 HomePage 界面引入 //可以点击组件后用 ctrl+单击自动引入
   3. **使用 MaterialApp** //在 main 里导入 Application
4. 测试

### 路由配置

1. 安装(如果安装不了,可以参考我的另一篇博客方法https://blog.csdn.net/xiaodi520520/article/details/99672182)

```js
dependencies:
// 下面有空格,安装好后再通过.lock文件吧版本号写回去
fluro: any

```

2. 添加 /pages/login.dart
3. 参考 /pages/home/index.dart 完善登陆页。
4. 在 application 里把入口文件改成 loginPage

#### 配置步骤

步骤：

1.  创建 routes.dart 文件 并编写 Routes 类的基本结构
2.  定义路由名称

```js
static String home="/";
static String login="/login";
```

3.  定义路由处理函数

```js
//fluro官网有介绍
static Handler _homeHandler = Handler(handlerFunc: (BuildContext context, Map<String, dynamic> params) {
return HomePage();
static Handler _loginHandler = Handler(handlerFunc: (BuildContext context, Map<String, dynamic> params) {
return LoginPage();
```

4.  编写函数 configureRoutes 关联路由名称和处理函数

```js
 static  void configureRoutes(Router router) {
router.define(home, handler: _homeHandler);
router.define(login,handler:_loginHandler);
```

5.  在 Application 中引入和调用路由

```js
import 'package:goodhouse/routes.dart';
   Router router=Router();
 Routes.configureRoutes(router);

```

6. 测试路由
   在 page_content 文件中添加按钮查看效果.

```js
 FlatButton(child:Text(Routes.home),onPressed:(){
         Navigator.pushNamed(context,Routes.home);
       }),
       FlatButton(child:Text(Routes.login),onPressed:(){
         Navigator.pushNamed(context,Routes.login);
       }),

```

### 优化路由(参数传递)

带参数页面处理

1. 在 /pages 目录添加 room_detail/index.dart 文件
2. 实现 RoomDetailPage
3. 在 /routes.dart 添加 \_roomDetailPage
4. 在 /routes.dart 的 configureRoutes 中添加 RoomDetailPage;
5. 修改 PageContent 测试

### 登录页面

#### scafford

- appBar
- title— Text
- body
- 用户名— TextField
- 密码— TextField
- 登陆按钮— RaisedButton
- 注册链接— Row[Text,FlatButton]

#### 添加密码显示与隐藏

1. 将无状态组件改成有状态组件— 快速方法组件名右键重构
2. 添加可点击的图标— IconButton

`````js
TextField(
  ​       decoration: InputDecoration(
  ​         labelText:"密码",
  ​         hintText:"请输入密码",
  ​         suffixIcon:IconButton(icon:Icon(
  ​           showPassword?Icons.visibility_off:Icons.visibility
  ​         ),
  ​          onPressed: (){
  ​            setState(() {
  ​              showPassword=!showPassword;
  ​            });
  ​          }) //使用文本框 TextField 自带的属性【后缀图标 suffixIcon】
  ​       ),
  ​     ),
​````

3. 添加状态— showPassword

​```js
bool showPassword=false;
`````

4. 根据状态展示不同内容
5. 给图标添加点击事件
6. 测试

#### 细节优化

边距/异形屏幕问题？
使用 SafeArea(解决超出问题)

```js
minimum: EdgeInsets.all(30.0); //解决padding问题
```

垂直高度不足问题？
使用 ListView(不够可以滚动) 替代 Column

### 添加注册页面

步骤：

1. 添加文件 /pages/register.dart
2. 将 login.dart 文件拷贝到 register.dart
3. 修改类名称
4. 修改 title
5. 在路由中添加 register
   1. 添加 route name
   2. 添加 route handler
   3. 在 configureRoutes 中关联 name 和 router
6. 修改了组件类型，需要重启 app 后测试

##### 注册页面优化

步骤：

1. 删除密码显示逻辑
2. 添加确认密码
3. 修改按钮及下方链接到文案
4. 优化登陆注册跳转，使用 Navigator.pushReplacementNamed

```js
Navigator.pushReplacementNamed(context, "login"); //可以删除记录
```

### 首页开始

参考pubpacka官方代码:
(注意我的环境有两处要改)

```js
// fontSize: 30
   fontSize: 30.0
// selectedItemColor: Colors.amber[800],
   fixedColor:Colors.blue,
```

修改后的代码如下:

```js
import 'package:flutter/material.dart';
import 'package:goodhouse/widgets/page_content.dart';

// 1. 需要准备 4 个 tab 内容区（tabView）
List<Widget> tabViewList = [
  PageContent(name: '首页'),
  PageContent(name: '搜索'),
  PageContent(name: '咨询'),
  PageContent(name: '我的'),
];

// 2. 需要准备 4 个 BottomNavigationBarItem
List<BottomNavigationBarItem> barItemList = [
  BottomNavigationBarItem(title: Text('首页'), icon: Icon(Icons.home)),
  BottomNavigationBarItem(title: Text('搜索'), icon: Icon(Icons.search)),
  BottomNavigationBarItem(title: Text('咨询'), icon: Icon(Icons.info)),
  BottomNavigationBarItem(title: Text('我的'), icon: Icon(Icons.account_circle)),
];

// 3. 编写有状态组件
class HomePage extends StatefulWidget {
  HomePage({Key key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  int _selectedIndex = 0;
  void _onItemTapped(int index) {
    setState(() {
      _selectedIndex = index;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body:tabViewList[_selectedIndex],
      bottomNavigationBar: BottomNavigationBar(
        type: BottomNavigationBarType.fixed,
        items: barItemList,
        currentIndex: _selectedIndex,
        fixedColor: Colors.green,
        onTap: _onItemTapped,
      ),
    );
  }
}
```

### 首页第一屏页面

#### 底部 tab 实现

步骤：

1. 新建文件 /pages/home/tab_index/index.dart
2. 添加依赖，编写无状态组件
3. 简化实现顶部区域--appBar
4. body 部分包含多个组件且可以滚动—使用 ListView
5. 在 home/index.dart 中使用 TabIndex 组件

```js
List < Widget > tabViewList = [
  TabIndex(),
  // PageContent(name: '首页'),
  PageContent((name: "搜索")),
  PageContent((name: "咨询")),
  PageContent((name: "我的"))
];
```

#### 轮播图的实现

步骤：

1. 准备组件框架代码
   1. 新建文件 /widgets/common_swipper.dart
   2. 添加依赖 material 和 flutter_swiper
   3. 准备图片数据
   4. 编写无状态组件
   5. 添加 images 参数 并在构造函数中赋值
2. 编写 swiper 核心代码
   1. 参照官网使用 swipper
   2. 修改 itemBuilder 和 itemCount
   3. Swiper 父组件指定高度
   ```js
   //父组件获取屏幕高度的固定方法
   var width = MediaQuery.of(context).size.width;
   ```
3. 测试

   1. 在 tabIndex 中使用 CommonSwiper

#### 首页导航

1.数据准备

```js
import 'package:flutter/material.dart';

//准备数据:title,image
class IndexNavigatorItem {
  final String title;
  final String imageUrl;
  final Function (BuildContext contenxt) onTap;
  IndexNavigatorItem(this.title,this.imageUrl,this.onTap);
}

List<IndexNavigatorItem> indexNavigatorItemList=[
  IndexNavigatorItem('整租','static/images/home_index_navigator_total.png',(BuildContext context){
    Navigator.of(context).pushNamed('login');
  }),
  IndexNavigatorItem('合租','static/images/home_index_navigator_share.png',(BuildContext context){
    Navigator.of(context).pushNamed('login');
  }),
  IndexNavigatorItem('地图找房','static/images/home_index_navigator_map.png',(BuildContext context){
    Navigator.of(context).pushNamed('login');
  }),
  IndexNavigatorItem('去出租','static/images/home_index_navigator_rent.png',(BuildContext context){
    Navigator.of(context).pushNamed('login');
  }),
];
```

2. 添加依赖 material 和 index_navigator_item
3. 编写无状态组件
4. 完成页面结构

```js
import 'package:flutter/material.dart';
import 'package:goodhouse/pages/home/tab_index/index_navigator_item.dart';
class IndexNavigator extends StatelessWidget {
  const IndexNavigator({Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      padding:EdgeInsets.only(top:6.0,bottom:6.0),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceAround,
        children: indexNavigatorItemList
        .map((item)=>InkWell(//InkWell可以实现水波纹效果
          onTap: (){
            item.onTap(context);
          },
          child: Column(
            children:<Widget>[
              Image.asset(item.imageUrl,
              width:47.5,),
              Text(item.title,style:TextStyle(
                fontSize: 14.0,
                fontWeight: FontWeight.w500
              ))

            ]
          ),
        )).toList()//转化成List
      ),
    );
  }
}

```

### 使用图片缓存进行封装

步骤：

1. 准备
   1. 安装 flutter_advanced_networkimage: ^0.5.0 依赖
   2. 添加文件 /widgets/common_image.dart
   3. 引入两个依赖
   4. 编写 正则 根据图片地址判断是网络图片还是本地图片
   ```js
   final networkUrlRef=RegExp('^http');//网络图片
   final localworkUrlRef=RegExp('^static');//本地图片
   ```

2) 编写框架代码
   1. 编写无状态组件
   2. 完善组件参数 src width height fit
3) 完成核心逻辑

   1. 如果是网络图片，使用 flutter_advanced_networkimage
   2. 如果是本地图片，使用 Image.asset
   3. 返回 Container

```js
import 'dart:html';
import 'package:flutter/material.dart';
import 'package:flutter_advanced_networkimage/provider.dart';

final networkUrlRef=RegExp('^http');//网络图片
final localworkUrlRef=RegExp('^static');//本地图片

class CommonImage extends StatelessWidget {

 final String src;
 final double width;
 final double height;
 final BoxFit fit;//图片的适应模式类型

const CommonImage({this.src,Key key,  this.width, this.height, this.fit}) : super(key: key);//把this.src放在最前面



 @override
 Widget build(BuildContext context) {
  if(networkUrlRef.hasMatch(src)){//正则判断是否网络图片
    return Image(
      width: width,
      height: height,
      fit: fit,
      image: AdvancedNetworkImage(
        src,
        useDiskCache: true,//磁盘缓存
        cacheRule: CacheRule(maxAge:Duration(days:7)),//保存时间
        timeoutDuration: Duration(seconds: 20)//超时时间
      ),
     );
  }
  if(localworkUrlRef.hasMatch(src)){
    return Image.asset(
      src,
      width: width,
      height: height,
      fit: fit,
    );
  }
  assert(false,"图片地址不合法");//抛出异常
  return Container();


 }
}

```

4. 使用 CommonImage

```js
CommonImage((src: item.imageUrl), (width: 47.5));
```

### 首页-tabIndex-推荐-准备

1.数据准备

```js
class IndexRecommendItem{
   final String title;
   final String subTitle;
   final String imageUrl;
   final String navigateUrl;
   const IndexRecommendItem(this.title,this.subTitle,this.imageUrl,this.navigateUrl);
}

const List<IndexRecommendItem> indexRecommendData=[
  const IndexRecommendItem(
    '家住回龙观','归属的感觉', 'static/images/home_index_recommend_1.png', 'login'),

  const IndexRecommendItem(
    '宜居四五环', '大都市生活','static/images/home_index_recommend_2.png', 'login'),

  const IndexRecommendItem(
    '喧嚣三里屯', '繁华的背后','static/images/home_index_recommend_3.png', 'login'),
  const IndexRecommendItem(
     '比邻十号线','地铁心连心', 'static/images/home_index_recommend_4.png', 'login'),
];

```

步骤：

1. 准备

   1. 使用上一节准备好的数据
   2. 新建文件 pages/home/tab_index/index_recommond.dart

2. 编写核心代码
   1. 添加依赖，无状态组件，dataList 参数，indexRecommendData 改成常量
   2. 添加背景色及边距
   3. 添加 wrap
3. 测试

```js
import 'package:flutter/material.dart';
import 'package:goodhouse/pages/home/tab_index/index_recommond_data.dart';
class IndexRecommond extends StatelessWidget {
  final List<IndexRecommendItem> dataList;

  const IndexRecommond({Key key,this.dataList=indexRecommendData}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(10.0),
      decoration: BoxDecoration(color: Color(0x08000000)),
      child: Column(
        children:<Widget>[
          Row(
            mainAxisAlignment:MainAxisAlignment.spaceBetween,
            children:<Widget>[
              Text('房屋推荐',style:TextStyle(
                color:Colors.black,
                fontWeight:FontWeight.w600,

              )),
              Text('更多',style:TextStyle(
                color:Colors.black54
              ))
            ]
          ),
          Padding(padding: EdgeInsets.all(5),),
          Wrap(
            spacing: 10.0,
            runSpacing: 10.0,
            children:dataList.map((e) => Container(
                width: (MediaQuery.of(context).size.width -30.0)/2,
                height: 100.0,
                decoration: BoxDecoration(color:Colors.red),
              )).toList()
          ),
        ]
      ),
    );
  }
}


```

优化提取 item

```js
import 'package:flutter/material.dart';
import 'package:goodhouse/pages/home/tab_index/index_recommond_data.dart';
import 'package:goodhouse/widgets/common_image.dart';

var textStyle=TextStyle(fontSize:14.0,fontWeight:FontWeight.w500);

class IndexRecommendItemWidget extends StatelessWidget {
  final IndexRecommendItem data;

  const IndexRecommendItemWidget(this.data, {Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return GestureDetector(//手势组件
      onTap: (){
        Navigator.of(context).pushNamed(data.navigateUrl);
      },
      child: Container(
        decoration: BoxDecoration(
          color:Colors.white,
        ),
        width:(MediaQuery.of(context).size.width-30.0)/2,
        padding: EdgeInsets.all(10.0),
        child:Row(
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          children:<Widget>[
            Column(
              children: <Widget>[
                Text(data.title,style:textStyle),
                Text(data.subTitle,style:textStyle),
              ],
            ),
            CommonImage(src:data.imageUrl,width:55.0)
          ]
        )
      ),
    );
  }
}

```

### 资讯部分

数据准备 home/info/data.dart

```js
// 资讯数据准备,注意下面的格式

class InfoItem {
  final String title;
  final String imageUrl;
  final String source;
  final String time;
  final String navigateUrl;
  const InfoItem(
      {this.title, this.imageUrl, this.source, this.time, this.navigateUrl});
}

const List<InfoItem> infoData = [
  const InfoItem(
      title: '置业选择 | 安贞西里 三室一厅 河间的古雅别院',
      imageUrl:
          'https://wx2.sinaimg.cn/mw1024/005SQLxwly1g6f89l4obbj305v04fjsw.jpg',
      source: "新华网",
      time: "两天前",
      navigateUrl: 'login'),
];

```

内容编码 info/index.dart

```js
import 'package:flutter/material.dart';
import 'package:goodhouse/pages/home/info/data.dart';
import 'package:goodhouse/pages/home/info/item_widget.dart';

class Info extends StatelessWidget {
  final bool showTitle;//考虑到资讯和首页有些显示,所以采用复用
  final List<InfoItem> dataList;

  const Info({Key key,this.showTitle=false, this.dataList=infoData}) : super(key: key);//infoData是data.dart文件内容


  @override
  Widget build(BuildContext context) {
    return Container(
      child: Column(
        children:<Widget>[
          if(showTitle)Container(
            alignment:Alignment.centerLeft,
            padding:EdgeInsets.all(10.0),
            child:Text('最新资讯',style:TextStyle(color:Colors.black,fontWeight:FontWeight.w600))
          ),
          Column(
            children:dataList.map((myitem) =>

            //  Container(
            //   height:100.0,
            //   margin:EdgeInsets.only(bottom:10.0),
            //   decoration:BoxDecoration(color:Colors.red),
            // )
            ItemWidget(myitem)
            ).toList(),
          )
        ]
      ),
    );
  }
}

```

子选项内容 info/item_widget.dat

```js
import 'package:flutter/material.dart';
import 'package:goodhouse/pages/home/info/data.dart';
import 'package:goodhouse/widgets/common_image.dart';

class ItemWidget extends StatelessWidget {
  final InfoItem data;
  const ItemWidget(this.data,{Key key}) : super(key: key);//记住this.data一定要在前面,并且和后面对象独立

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 100.0,
      padding: EdgeInsets.only(left: 10.0, right: 10.0, bottom: 10.0),
      child: Row(
        children: <Widget>[
          CommonImage(
            src: data.imageUrl,
            width: 120.0,
            height: 90.0,
          ),
          Padding(
            padding: EdgeInsets.only(left: 10.0),
          ),
          Expanded(
              child: Column(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: <Widget>[
                Text(data.title,
                    style: TextStyle(
                        fontWeight: FontWeight.w600, color: Colors.black)),
                Row(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: <Widget>[
                    Text(data.source, style: TextStyle(color: Colors.black54)),
                    Text(data.time, style: TextStyle(color: Colors.black54))
                  ],
                )
              ])) //Expanded自动填充组件
        ],
      ),
    );
  }
}
```

### 首页资讯 tab_info 页面

创建 tab_info/index.dart 文件,复用 info 文件内容渲染即可
在 home/index.dart 中将第三个资讯 tabViewList 改为 TabInfo().

### 首页搜索页面

数据准备:tab_search/dataList.dart
**步骤：**

1. 创建文件 /pages/home/tab_search/dataList.dart 使用上一节准备的数据
2. 创建文件 /pages/home/tab_search/index.dart
3. 引入依赖，创建有状态组件
4. 编写主体结构(使用列表组件和搜索组件分离)

```js
import 'package:flutter/material.dart';

import 'dataList.dart';

class TabSearch extends StatefulWidget {
  const TabSearch({Key key}) : super(key: key);

  @override
  _TabSearchState createState() => _TabSearchState();
}

class _TabSearchState extends State<TabSearch> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('tabSearch'),),
      body: Column(children: <Widget>[
        Container(
          height: 40.0,
          child: Text('filterBar'),),
          Expanded(child: ListView(
            children: dataList.map((item)=>Container(
              height: 200.0,
              margin: EdgeInsets.only(bottom: 10.0),
              decoration: BoxDecoration(color: Colors.grey),
            )).toList(),
          ),)
      ],),

    );
  }
}
```

然后封装 item 和 tags:
common_tag.dart(中间用工厂构造函数)/room_list_item_widget.dart
工厂函数用法：

```js
import 'package:flutter/material.dart';
class CommonTag extends StatelessWidget {
  final String title;//这是外部传递过来的
  final Color color;//自己的
  final Color backgroundColor;
  const CommonTag.origin(this.title,{Key key,this.color=Colors.black,this.backgroundColor}) : super(key: key);//注意外部和自己的写法

  // 工厂函数
  factory CommonTag(String title){
    switch (title) {
      case '近地铁':
      return CommonTag.origin(
        title,//title子不变，其它值都改变
        color:Colors.red,
        backgroundColor: Colors.red[50],
      );
      case '集中供暖':
      return CommonTag.origin(
        title,//title子不变，其它值都改变
        color:Colors.blue,
        backgroundColor: Colors.blue[50],
      );
      case '新上':
      return CommonTag.origin(
        title,//title子不变，其它值都改变
        color:Colors.green,
        backgroundColor: Colors.green[50],
      );
      case '随时看房':
      return CommonTag.origin(
        title,//title子不变，其它值都改变
        color:Colors.orange,
        backgroundColor: Colors.orange[50],
      );

      default:
      return CommonTag.origin(title);
    }
  }


  @override
  Widget build(BuildContext context) {
    return Container(
      margin: EdgeInsets.only(right:4.0),
      padding: EdgeInsets.only(left:4.0,right:4.0,top:4.0,bottom:2.0),
      decoration: BoxDecoration(
        color:backgroundColor,//工厂里自定义的背景
        borderRadius: BorderRadius.circular(8.0)
      ),
      child: Text(
        title,//工厂里自定义的文字
        style: TextStyle(
          fontSize:10.0,
          color:color//工厂里自定义的字体颜色
        ),
      ),
    );
  }
}
```

### 搜索组件封装开发

步骤：

1. 创建文件 /widgets/search_bar/index.dart
2. 引入 material 依赖， 创建有状态组件，添加参数
3. 编写界面代码
4. 测试

```js
import 'package:flutter/material.dart';
import 'package:goodhouse/widgets/common_image.dart';

class SearchBar extends StatefulWidget {
  final bool showLocation; //是否显示位置
  final Function goBackCallback; //回退
  final String inputValue; //搜索框值
  final String defaultInputValue; //默认显示值
  final Function onCancel; //取消按钮
  final bool showMap; //是否显示地图按钮
  final Function onSearch; //点击搜索框触发
  final ValueChanged<String> onSearchSubmit; //点击按键回车触发

  const SearchBar(
      {Key key,
      this.showLocation,
      this.goBackCallback,
      this.inputValue = "",
      this.defaultInputValue = "请输入搜索词",
      this.onCancel,
      this.showMap,
      this.onSearch,
      this.onSearchSubmit})
      : super(key: key);

  @override
  _SearchBarState createState() => _SearchBarState();
}

class _SearchBarState extends State<SearchBar> {
  String _searchWord = '';
  TextEditingController _controller; //输入框的控制器
  FocusNode _focus; //焦点声明
  _onClean() {
    _controller.clear(); //清除输入框控制器
    setState(() {
      _searchWord = '';
    });
  }

  // 初始化控制器
  @override
  void initState() {
    _focus = FocusNode(); //初始化焦点
    _controller = TextEditingController(text: widget.inputValue);
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Row(
        children: <Widget>[
          if (widget.showLocation != null)
            //location判断
            Padding(
              padding: EdgeInsets.only(right: 10.0),
              child: GestureDetector(
                //手势组件
                onTap: () {},
                child: Row(
                  children: <Widget>[
                    Icon(Icons.room, color: Colors.green, size: 16.0),
                    Text('北京',
                        style: TextStyle(color: Colors.black, fontSize: 14.0))
                  ],
                ),
              ),
            ),
          if (widget.goBackCallback != null)
            Padding(
                padding: EdgeInsets.only(right: 10.0),
                child: GestureDetector(
                    onTap: () {},
                    child: Row(children: <Widget>[
                      Icon(Icons.chevron_left, color: Colors.black87)
                    ]))),
          Expanded(
              //自适应组件
            child: Container(
            height: 34.0,
            decoration: BoxDecoration(
                borderRadius: BorderRadius.circular(17.0),
                color: Colors.grey[200]),
            margin: EdgeInsets.only(right: 10.0),
            child: TextField(
              // 优化start
              focusNode: _focus,
              onTap: (){
                if(widget.onSearchSubmit==null) {
                  _focus.unfocus();//解决回退失去焦点问题
                }
              widget.onSearch(); //使用自己定义的变量方法

              },
              onSubmitted: widget.onSearchSubmit,
              textInputAction: TextInputAction.search, //按键变为搜索
              controller: _controller, //自己定义的控制器
              onChanged: (String value) {
                //值改变问题
                setState(() {
                  _searchWord = value;
                });
              },
              // 优化end
              decoration: InputDecoration(
                  hintText: '请输入搜索词',
                  border: InputBorder.none,
                  contentPadding: EdgeInsets.only(top: -2.0, left: -10.0),
                  suffixIcon: GestureDetector(
                    //触摸控件
                    onTap: () {
                      _onClean();
                    },
                    child: Icon(
                      //后置图标
                      Icons.clear,
                      size: 18.0,
                      color: _searchWord == ''
                          ? Colors.grey[200]
                          : Colors.grey, //去图标的技巧:当空时设置为没颜色
                    ),
                  ),
                  icon: Padding(
                    padding: EdgeInsets.only(top: 4.0, left: 8.0),
                    child: Icon(
                      Icons.search,
                      size: 18.0,
                      color: Colors.grey,
                    ),
                  )),
            ),
          )),
          if (widget.onCancel != null)
            Padding(
                padding: EdgeInsets.only(right: 10.0),
                child: GestureDetector(
                    onTap: () {},
                    child: Row(children: <Widget>[
                      Text('取消',
                          style: TextStyle(color: Colors.black, fontSize: 14.0))
                    ]))),
          if (widget.showMap != null)
            CommonImage(src: 'static/icons/widget_search_bar_map.png')
        ],
      ),
    );
  }
}



```

#### 主页搜索页使用封装好的搜索组件

````js
  appBar:AppBar(
        title:SearchBar(showLocation: true,showMap: true,onSearch: (){
          // Navigator.of(context).pushNamed('search');//跳转到搜索页面
          print("跳转到搜索页面测试");
        },),
        backgroundColor: Colors.white,
      ),
```
````

### 我的页面
创建tab_profile/index.dart
​```js
import 'package:flutter/material.dart';
import 'package:goodhouse/pages/home/tab_profile/header.dart';
class TabProfile extends StatelessWidget {
  const TabProfile({Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        elevation:0,//去掉边框
        title:Text('我的'),
        actions:<Widget>[//右侧组件
          IconButton(
            onPressed: (){
              // Navigator.of(context).pushNamed(routeName);
              print("准备跳转到设置页");
            },
            icon:Icon(Icons.settings),
          )
        ]
      ),
      
      body:ListView(
        children: <Widget>[
          Header(),
          Text('内容区域')
        ],
      )
    );
  }
}

```
#### 准备头部header.dart
```js
import 'package:flutter/material.dart';

var loginFontStyle = TextStyle(fontSize: 20.0, color: Colors.white);

class Header extends StatelessWidget {
  const Header({Key key}) : super(key: key);
Widget _noLoginPageHeader(BuildContext context){
   return Container(
      decoration: BoxDecoration(color: Colors.green),
      height: 95.0,
      padding: EdgeInsets.only(top: 10.0, left: 20.0, bottom: 20.0),
      child: Row(children: <Widget>[
        Container(
          height: 65.0,
          width: 65.0,
          margin: EdgeInsets.only(right: 15.0),
          child: CircleAvatar(
            //圆形头像
            backgroundImage: NetworkImage(
                "https://tva1.sinaimg.cn/large/006y8mN6ly1g6tbgbqv2nj30i20i2wen.jpg"),
          ),
        ),
        Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[
            Padding(padding: EdgeInsets.only(top: 6.0)),
            Row(
              children: <Widget>[
                GestureDetector(
                  //手势组件
                  onTap: () {
                    Navigator.of(context).pushNamed("login");
                  },
                  child: Text('登录', style: loginFontStyle),
                ),
                Text("/", style: loginFontStyle),
                GestureDetector(
                  //手势组件
                  onTap: () {
                    print("点击了注册");
                    Navigator.of(context).pushNamed("rigister");
                  },
                  child: Text('注册', style: loginFontStyle),
                ),
              ],
            ),
            Text('登录后可以体验更多',style:TextStyle(color:Colors.white))
          ],
        )
      ]),
    );
}
Widget _loginPageHeader(BuildContext context){
  String userName="张三";
  String userImage="https://tva1.sinaimg.cn/large/006y8mN6ly1g6tbnovh8jj30hr0hrq3l.jpg";
   return Container(
      decoration: BoxDecoration(color: Colors.green),
      height: 95.0,
      padding: EdgeInsets.only(top: 10.0, left: 20.0, bottom: 20.0),
      child: Row(children: <Widget>[
        Container(
          height: 65.0,
          width: 65.0,
          margin: EdgeInsets.only(right: 15.0),
          child: CircleAvatar(
            //圆形头像
            backgroundImage: NetworkImage(userImage),
          ),
        ),
        Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[
            Padding(padding: EdgeInsets.only(top: 6.0)),
            Row(
              children: <Widget>[
               
                Text(userName, style: loginFontStyle),
               
              ],
            ),
            Text('查看编辑个人资料',style:TextStyle(color:Colors.white))
          ],
        )
      ]),
    );
}

  @override
  Widget build(BuildContext context) {
    var isLogin=false;
   return isLogin ?_noLoginPageHeader(context):_loginPageHeader(context);
  }
}


```
### 功能按钮
1.准备数据文件  pages/home/tab_profile/function_button_data.dart
```js
import 'package:flutter/material.dart';

class FunctionButtonItem{
  final String imageUrl;
  final String title;
  final Function onTapHandle;
  FunctionButtonItem(this.imageUrl,this.title,this.onTapHandle);
}

final List<FunctionButtonItem> list=[
  FunctionButtonItem('static/images/home_profile_record.png', "看房记录", (){}),
  FunctionButtonItem('static/images/home_profile_order.png', '我的订单', null),
  FunctionButtonItem('static/images/home_profile_favor.png', '我的收藏', null),
  FunctionButtonItem('static/images/home_profile_id.png', '身份认证', null),
  FunctionButtonItem('static/images/home_profile_message.png', '联系我们', null),
  FunctionButtonItem('static/images/home_profile_contract.png', '电子合同', null),
  FunctionButtonItem('static/images/home_profile_wallet.png', '钱包', null),
  FunctionButtonItem('static/images/home_profile_house.png', "房屋管理", (context){
    bool isLogin=true;//假设先设置未登录
    if(isLogin){
      Navigator.pushNamed(context, 'login');
    }
  })
  
];
```
创建function_button.dart组件
```js
import 'package:flutter/material.dart';

import 'function_button_data.dart';
class FunctionButton extends StatelessWidget {
  const FunctionButton({Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Wrap(
        spacing: 1.0,
        runSpacing: 1.0,
        children:list.map((item)=>Container(
          height:20.0,
          width:MediaQuery.of(context).size.width*0.33,
          decoration: BoxDecoration(color:Colors.red),
        )).toList(),
      )
    );
  }
}
```
item选项组件封装
```js
import 'package:flutter/material.dart';

import '../../../widgets/common_image.dart';
import 'function_button_data.dart';

class FunctionButtonWidget extends StatelessWidget {
  final FunctionButtonItem data;
  const FunctionButtonWidget(this.data,{Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap:(){
        if(data.onTapHandle!=null){
          data.onTapHandle(context);
        }
      },
      child:Container(
        margin: EdgeInsets.only(top:30.0),
        width: MediaQuery.of(context).size.width*0.33,
        child: Column(
          children:<Widget>[
            CommonImage(src:data.imageUrl),
            Text(data.title)
          ]
        ),
      )
    );
  }
}
```
广告组件Advertisement.dart
```js
import 'package:flutter/material.dart';

import '../../../widgets/common_image.dart';
class Advertisement extends StatelessWidget {
  const Advertisement({Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.only(top:30.0,bottom:20.0,left:10.0,right:10.0),
      child: CommonImage(src:'https://tva1.sinaimg.cn/large/006y8mN6ly1g6te62n8f4j30j603vgou.jpg')
    );
  }
}
```
### 右上角设置页

**效果：**

<div align=left>
  <img src="https://tva1.sinaimg.cn/large/006y8mN6ly1g872a22ec3j30cm0mygnr.jpg" alt="image-20191022163305038" style="zoom:50%;" />
  <img src="https://tva1.sinaimg.cn/large/006y8mN6ly1g872aihmqjj30cm0mygnv.jpg" alt="image-20191022163331340" style="zoom:50%;" />
</div?



**分析：**

需要 toast 弹窗，使用 [fluttertoast](https://github.com/PonnamKarthik/FlutterToast)

用法：

  ```dart
// 1. 安装依赖
fluttertoast: ^4.0.0

// 2. 引入依赖
import 'package:fluttertoast/fluttertoast.dart';

// 3. 使用
Fluttertoast.showToast(
        msg: "This is Center Short Toast",
        toastLength: Toast.LENGTH_SHORT,
        gravity: ToastGravity.CENTER,
        timeInSecForIos: 1,
        backgroundColor: Colors.red,
        textColor: Colors.white,
        fontSize: 16.0
    );
  ```



**步骤：**

1. 准备

   1. 按照依赖 fluttertoast: ^3.1.3
   2. utils/common_toast.dart 新建文件，封装 CommonToast
   3. 新建文件 pages/setting.dart，引入依赖，添加无状态组件

2. 核心编码

   1. 完成页面主体结构
   2. 在路由系统注册当前页面
   3. 添加`退出登陆`按钮
   4. 实现退出登陆逻辑
   
   ### 房屋管理页面开始
   参考flutter官网的appBar:https://flutterchina.club/catalog/samples/tabbed-app-bar/
   ```js
   import 'package:flutter/material.dart';
import 'package:goodhouse/pages/home/tab_search/dataList.dart';
import 'package:goodhouse/widgets/room_list_item_widget.dart';

class RoomManagePage extends StatelessWidget {
  const RoomManagePage({Key key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
        length: 2,//tab个数
        initialIndex: 0,//初始索引
        child: Scaffold(
          appBar: AppBar(
            title:  Text('房屋管理'),
            bottom: TabBar(
              tabs: <Widget>[
                Tab(text:'空置'),
                Tab(text:'已租'),
              ]
            ),
          ),
          body: TabBarView(
            children: <Widget>[
              ListView(
                children:dataList.map((item)=>
                RoomListItemWidget(item)
                ).toList()
              ),
              ListView(
                children:dataList.map((item)=>
                RoomListItemWidget(item)
                ).toList()
              )
            ]
          ),
        ),
      );
    
  }
}
```
同时增加底部悬浮按钮
准备悬浮按钮common_floating_button.dart组件
```js
import 'package:flutter/material.dart';
class CommonFloatingButton extends StatelessWidget {
  final String title;
  final Function onTap;

  const CommonFloatingButton(this.title, this.onTap,{Key key}) : super(key: key);
  

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap:(){
        if(onTap!=null) onTap();
      },
      child:Container(
        margin: EdgeInsets.all(20.0),
        height: 40.0,
        decoration: BoxDecoration(
          borderRadius:BorderRadius.circular(10.0),
          color:Colors.green
        ),
        child: Center(
          child:Text(title,style:TextStyle(
            color:Colors.white,
            fontSize:16.0,
            fontWeight:FontWeight.w600
          ))
        ),
      )
    );
  }
}
```
在Scaffold组件中加入悬浮按钮
```js
floatingActionButtonLocation: FloatingActionButtonLocation.centerFloat,//悬浮位置
          floatingActionButton: CommonFloatingButton('发布房源', (){
            Navigator.of(context).pushNamed("login");
          }),
```
### 发布房源页面开发
里面对各个组件进行了封装
大体代码如下
```js
import 'package:flutter/material.dart';
import 'package:goodhouse/widgets/common_floating_button.dart';
import 'package:goodhouse/widgets/common_form_item.dart';
import 'package:goodhouse/widgets/common_image_pick.dart';
import 'package:goodhouse/widgets/common_radio_form_item.dart';
import 'package:goodhouse/widgets/common_select_form_item.dart';
import 'package:goodhouse/widgets/common_title.dart';
import 'package:goodhouse/widgets/room_Appliance.dart';

class RoomAddPage extends StatefulWidget {
  RoomAddPage({Key key}) : super(key: key);

  @override
  _RoomAddPageState createState() => _RoomAddPageState();
}

class _RoomAddPageState extends State<RoomAddPage> {
  int rentType=0;
  int rentTypeTwo=0;
  int roomType = 0;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar:AppBar(
        title:Text('发布房源'),
      ),
      floatingActionButton: CommonFloatingButton('提交', (){
        print("点击了提交");
      }),
      floatingActionButtonLocation: FloatingActionButtonLocation.centerFloat,
      body:ListView(
        children:<Widget>[
          CommonTitle('房源信息'),
          CommonFormItemWidget(//自定义小区
            label: '小区',
            contentBuilder:(context){
              return Container(
                height: 40.0,
                width: 280.0,
                child:GestureDetector(
                  behavior: HitTestBehavior.translucent,//解决点击空白地方无效问题
                  child: Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: <Widget>[
                      Text('请选择小区',style:TextStyle(
                        fontSize:16.0
                      )),
                      Icon(Icons.keyboard_arrow_right)
                    ],
                  ),
                  onTap: (){
                    print("跳转选择小区页");
                  },
                )
              );
            }
          ),
          CommonFormItemWidget(
            label: '租金',
            hitText:'请输入租金',
            suffixText: '元/月',
            controller: TextEditingController(),
          ),
          CommonFormItemWidget(
            label: '大小',
            hitText:'请输入房屋大小',
            suffixText: '平方米',
            controller: TextEditingController(),
          ),
          CommonRadioFormIten(
            label: '租赁方式',
            options: ['合租', '整租'],
            value: rentType,
            onChange: (index){
              print(index);
              setState(() {
                rentType=index;
              });
            },
          ),
          CommonRadioFormIten(
              label: '装修',
              options: ['精装', '简装'],
              value: rentTypeTwo,
              onChange: (index) {
                setState(() {
                  rentTypeTwo = index;
                });
              }),
          CommonSelectFormItemWedget(
            label: '户型',
            value:roomType,
            onChange: (val){
              setState(() {
                roomType=val;
              });
            },
            options: ['一室','二室','三室','四室'],
          ),
          CommonTitle('房屋图像'),
          //  CommonImagePicker(
          //   onChange: (List files) {},//删除File,不然会报错
          //   // onChange: (List<File> files) {},
          // ),
       
          CommonTitle('房屋标题'),
          CommonTitle('房屋配置'),
          RoomAppliance(
            onChange: (data) {},
          ),
          CommonTitle('房屋描述'),
        ]
      ),
    );
  }
}

```

#### 房屋详情页

```js
import 'package:flutter/material.dart';
import 'package:goodhouse/pages/room_detail/data.dart';
import 'package:goodhouse/widgets/common_title.dart';
import 'package:share/share.dart';

var bottomButtonTextStyle = TextStyle(fontSize: 20.0, color: Colors.white);

class RoomDetailPage extends StatefulWidget {
  final String roomId;
  const RoomDetailPage({Key key, this.roomId}) : super(key: key);

  @override
  _RoomDetailPageState createState() => _RoomDetailPageState();
}

class _RoomDetailPageState extends State<RoomDetailPage> {
  RoomDetailData data;
  bool isLike = false;
  @override
  void initState() {
    // TODO: implement initState
    data = defaultData;
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    if (null == data) return Container();
    return Scaffold(
        appBar: AppBar(
          // title: Text('roomId:${widget.roomId}'),
          title: Text(data.title),
          actions: <Widget>[
            IconButton(
                icon: Icon(Icons.share),
                onPressed: () {
                  Share.share('https://baidu.com');
                }),
          ],
        ),
        body: Stack(
          children: <Widget>[
            ListView(children: <Widget>[
              CommonTitle('房屋配置'),
              CommonTitle('房屋概况'),
              CommonTitle('猜你喜欢'),
            ]),
            Positioned(
              width: MediaQuery.of(context).size.width,
              height: 100.0,
              bottom: 0,
              child: Container(
                color: Colors.grey[200],
                padding: EdgeInsets.only(top: 10.0, left: 20.0, bottom: 20.0),
                child: Row(
                  children: <Widget>[
                    GestureDetector(
                      onTap: () {
                        setState(() {
                          isLike = !isLike;
                        });
                      },
                      child: Container(
                          height: 50.0,
                          width: 40.0,
                          margin: EdgeInsets.only(right: 10.0),
                          child: Column(
                            mainAxisAlignment: MainAxisAlignment.spaceBetween,
                            children: <Widget>[
                              Icon(isLike ? Icons.star : Icons.star_border,
                                  size: 24.0,
                                  color: isLike ? Colors.green : Colors.black),
                              Text(isLike ? '已收藏' : '收藏',
                                  style: TextStyle(fontSize: 12.0))
                            ],
                          )),
                    ),
                    Expanded(
                        child: GestureDetector(
                            onTap: () {},
                            child: Container(
                              height: 50.0,
                              margin: EdgeInsets.only(right: 5.0),
                              decoration: BoxDecoration(
                                  color: Colors.cyan,
                                  borderRadius: BorderRadius.circular(6.0)),
                              child: Center(
                                child:
                                    Text('联系房东', style: bottomButtonTextStyle),
                              ),
                            ))),
                    Expanded(
                        child: GestureDetector(
                            onTap: () {},
                            child: Container(
                              height: 50.0,
                              margin: EdgeInsets.only(right: 5.0),
                              decoration: BoxDecoration(
                                  color: Colors.green,
                                  borderRadius: BorderRadius.circular(6.0)),
                              child: Center(
                                child:
                                    Text('预约看房', style: bottomButtonTextStyle),
                              ),
                            ))),
                  ],
                ),
              ),
            )
          ],
        ));
  }
}

```

### 搜索页展示区域filter_bar(细节在源码里)
```js
import 'package:flutter/material.dart';
import 'package:goodhouse/pages/home/tab_search/filter_bar/data.dart';
import 'package:goodhouse/pages/home/tab_search/filter_bar/item.dart';
class FilterBar extends StatefulWidget {
  final ValueChanged<FilterBarResult> onChange;
  const FilterBar({Key key, this.onChange}) : super(key: key);

  @override
  _FilterBarState createState() => _FilterBarState();
}

class _FilterBarState extends State<FilterBar> {

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 41.0,
      decoration: BoxDecoration(
        border:Border(bottom:BorderSide(color:Colors.black12,width: 1.0))
      ),
       child: Row(
         mainAxisAlignment: MainAxisAlignment.spaceAround,
         children: <Widget>[
           Item(
             title: '区域',
             isActive:true,
             onTap: (context){},
           ),
           Item(
             title: '方式',
             isActive:false,
             onTap: (context){},
           ),
           Item(
             title: '租金',
             isActive:false,
             onTap: (context){},
           ),
           Item(
             title: '筛选',
             isActive:false,
             onTap: (context){},
           ),
         ],
       ),
    );
  }
}

```
### 接口联调

#### 封装dio请求
文件utils/dio_http.dart
核心代码
```js
import 'dart:io';

import 'package:dio/dio.dart';
import 'package:flutter/material.dart';
import '../config.dart';

class DioHttp {
  Dio _client;
  BuildContext context;

  static DioHttp of(BuildContext context) {
    return DioHttp.internal(context);
  }

  DioHttp.internal(BuildContext context) {
    if (_client != null || context != this.context) {
      this.context = context;
      var options = BaseOptions(
          baseUrl: Config.BaseUrl,
          connectTimeout: 1000 * 10,
          receiveTimeout: 1000 * 3,
          extra: {'context': context});
      var client = Dio(options);
      this._client = client;
    }
  }

  Future<Response<Map<String, dynamic>>> get(String path,
      [Map<String, dynamic> params, String token]) async {
    var options = Options(headers: {'Authorization': token});
    return await _client.get(path, queryParameters: params, options: options);
  }

  Future<Response<Map<String, dynamic>>> post(String path,
      [Map<String, dynamic> params, String token]) async {
    var options = Options(headers: {'Authorization': token});
    return await _client.post(path, data: params, options: options);
  }

  Future<Response<Map<String, dynamic>>> postFormData(String path,
      [Map<String, dynamic> params, String token]) async {
    var options = Options(
        contentType: ContentType.parse('multipart/form-data'),
        headers: {'Authorization': token});
    return await _client.post(path, data: params, options: options);
  }
}

```

### 接口联调(注册页)
核心代码:
```js
import 'dart:convert';
import 'package:flutter/material.dart';
import 'package:goodhouse/utils/common_toast.dart';
import 'package:goodhouse/utils/dio_http.dart';
import 'package:goodhouse/utils/string_is_null_or_empty.dart';

// import 'package:goodhouse/widgets/page_content.dart';
class RigisterPage extends StatefulWidget {
  const RigisterPage({Key key}) : super(key: key);

  @override
  RigisterPageState createState() {
    return new RigisterPageState();
  }
}

class RigisterPageState extends State<RigisterPage> {
  var usernameController = TextEditingController();
  var passwordController = TextEditingController();
  var repeatPasswordController = TextEditingController();

  // 注册方法
  _registerHandler() async {
    var username = usernameController.text;
    var password = passwordController.text;
    var repeatPassword = repeatPasswordController.text;
    if (password != repeatPassword) {
      CommontToast.showToast('两次输入密码不一致!');
      return;
    }
    if(stringIsNullOrEmpty(username)||stringIsNullOrEmpty(password)) {
      CommontToast.showToast("用户名或者密码不能为空");
      return;
    }
    const url="/register";
    var params ={'username':username,'password':password};

    var res=await DioHttp.of(context).post(url,params);
    // var resString=json.decode(res.toString());
    // print(resString);//这个转成了字符串
    // print(res.data['data']['code']==0);
    if(res.data['data']['code']==0){
      CommontToast.showToast("注册成功,请登录");
      Navigator.of(context).pushReplacementNamed('login');
      
    }

  }

  // 定义一个bool值
  bool showPassword = false;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(title: Text("注册")),
        body: SafeArea(
          minimum: EdgeInsets.all(30.0),
          child: ListView(children: <Widget>[
            TextField(
              controller: usernameController,
              //文本输入框组件
              decoration: InputDecoration(labelText: '用户名', hintText: '请输入用户名'),
            ),
            Padding(
              padding: EdgeInsets.all(10.0),
            ),
            TextField(
              controller: passwordController,
              decoration: InputDecoration(
                labelText: "密码",
                hintText: "请输入密码",
                // 注册页面不需要
                // suffixIcon: IconButton(
                //     icon: Icon(showPassword
                //         ? Icons.visibility_off
                //         : Icons.visibility),
                //     onPressed: () {
                //       setState(() {
                //         showPassword = !showPassword;
                //       });
                //     }) //使用文本框 TextField 自带的属性【后缀图标 suffixIcon】
              ),
            ),
            Padding(
              padding: EdgeInsets.all(10.0),
            ),
            TextField(
              controller: repeatPasswordController,
              decoration: InputDecoration(
                labelText: "确认密码",
                hintText: "请再次输入密码",
                // 注册页面不需要
                // suffixIcon: IconButton(
                //     icon: Icon(showPassword
                //         ? Icons.visibility_off
                //         : Icons.visibility),
                //     onPressed: () {
                //       setState(() {
                //         showPassword = !showPassword;
                //       });
                //     }) //使用文本框 TextField 自带的属性【后缀图标 suffixIcon】
              ),
            ),
            Padding(
              padding: EdgeInsets.all(10.0),
            ),
            RaisedButton(
              color: Colors.green,
              child: Text(
                "注册",
                style: TextStyle(color: Colors.white),
              ),
              onPressed: () {
                // print("注册");
                _registerHandler();
              },
            ),
            Padding(
              padding: EdgeInsets.all(10.0),
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: <Widget>[
                Text("已有账号,"),
                FlatButton(
                    child: Text(
                      "去登录",
                      style: TextStyle(color: Colors.green),
                    ),
                    onPressed: () {
                      Navigator.pushReplacementNamed(context, 'login');
                      // Navigator.pushNamed(context, 'login');
                    })
              ],
            )
          ]),
        ));
  }
}
```
因真实接口失效了,故只用了一个真实线上模拟接口.
### 增加启动页面
配置启动页面路由和设置第一页为启动页.
启动页面核心代码
```js
import 'dart:async';

import 'package:flutter/material.dart';

class LoadingPage extends StatefulWidget {
  LoadingPage({Key key}) : super(key: key);

  @override
  _LoadingPageState createState() => _LoadingPageState();
}

class _LoadingPageState extends State<LoadingPage> {
  @override
  void initState() {
    Timer(Duration(seconds: 3), () {
      Navigator.of(context).pushReplacementNamed('/');
    });
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(
          image: DecorationImage(
              fit: BoxFit.fill,
              image: AssetImage('static/images/loading.jpg'))),
    );
  }
}
```

### 安卓打包
步骤：

1. 修改应用名称
   /项目目录/android/app/src/main/AndroidManifest.xml    文件 application 标签 android:label 属性
2. 修改图标及背景图(背景图解决空白问题,我是直接替换res文件夹的)
   文件地址：/resource/chapter6/01静态资源/package/android/res
   目标地址：/项目目录/android/app/src/main/res
3. 修改构建配置(构建时不需要检验)
   在 android/app/build.gradle 文件中 android.lintOption 添加属性 checkReleaseBuilds false
4. 构建
   flutter build apk 

打包成功后apk文件的位置:build\app\outputs\apk\release\app-release.apk (18.4MB)
安装成功后,打开有闪退问题,在 android/app/build.gradle 文件,最后解决办法:
```js
    buildTypes {
        release {
            signingConfig signingConfigs.debug
            //关键代码:关闭混淆,解决闪退问题
            minifyEnabled false
            shrinkResources false
            // 关键代码:解决闪退问题
        }
    }
```
我安装后测试能正常运行,本次开发告一段落,但学习不会停止,请期待下一个开发.
