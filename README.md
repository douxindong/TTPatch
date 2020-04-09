# TTPatch
*热修复、热更新、JS代码动态下发、动态创建类*


[1. 使用文档](https://github.com/yangyangFeng/TTPatch/blob/master/%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3.md)

[2. 基础用法](https://github.com/yangyangFeng/TTPatch/wiki/%E5%9F%BA%E7%A1%80%E7%94%A8%E6%B3%95)




## 1. 功能列表 

|功能特性|备注限制|
|------|-------|
|**替换指定`ObjectC`方法实现**          | 实例/静态方法均可替换实现|
|**支持`block`**                      |`ObjectC`传入`JS`,  `JS`传入`ObjectC`均已支持|
|**支持添加属性**                     |为已存在的`class`添加属性|
|**支持基础数据类型**                   |非id类型,如`int`,`bool`均已支持|
|**支持下发纯`JS`页面**                    |纯`JS`代码映射原生代码,动态发布|
|**实现协议**                        | 2020年04月01日新增|
|**支持真机无线预览**                 | [详细说明](https://github.com/yangyangFeng/TTPatch/blob/master/%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3.md#%E7%AE%80%E5%8D%95%E4%BD%93%E9%AA%8C-ii)|



## 2. 安装


### CocoaPods `pod 0.3.0`

1. 在 Podfile 中添加  `pod 'TTPatch'`。
2. 执行 `pod install` 或 `pod update`。
3. 导入 "TTPatch.h"


**演示项目**:Example.xcodeproj 
#### 运行效果图

![效果图.gif](https://wos2.58cdn.com.cn/DeFazYxWvDti/frsupload/cc8621a9531f405a65118f2a4fe1bfc1_demo1.gif)


#### 在线下发补丁执行
![在线下发补丁执行.gif](https://wos2.58cdn.com.cn/DeFazYxWvDti/frsupload/f5a7fff60c8cf40308cca5d783981204_demo2.gif)


#### 重启后加载已下发补丁
![重启后加载已下发补丁.gif](https://wos2.58cdn.com.cn/DeFazYxWvDti/frsupload/cc1be957bfb4597aefd42aa3c579f30b_demo3.gif)

## 3. 使用文档

### 简单体验 I

首先要下载我们的demo工程,然后你只要修改`src`目录下的`.js`文件，然后运行 `npm run build`.这条命令会将我们刚刚修改的工作区代码(`src`)经过转义压缩输出到`outputs`目录下, `outputs`目录下的文件供app读取使用.

⚠️⚠️app不能直接读取`src`工作区的文件哦!!!!

### 简单体验 II
如果你已经熟练使用了`步骤 I`是不是觉得每次要经过下面三步,很麻烦. 那么你可以往下看
* `save`
* `run build`
* `run xocde`

目前`demo`已经支持模拟器/真机 在线实时预览修改内容了~~~~~

**下面为实时预览的准备工作**

1. 将`JS`目录下的`node.js`依赖下载成功.执行`npm install`即可.
2. 执行`npm run server` 开启本地服务
3. 将真机/模拟器调至同一`WIFI`下
4. 运行`demo`

> 如步骤1.失败请检查本地`npm,node`版本,下面给出我电脑版本供参考`npm -v  6.9.0`
/` node -v v10.16.0`


此时你的准备工作已经全部完成, 接下来用你最喜欢的`IDE`打开`src`目录下的任意`js`文件进行编辑, 在点击保存之后你会发现手机数据也跟着刷新了~~~~

### 实际使用 III

实际使用的话，就需要一些JS相关的支持，要确保本机已安装`npm`.如果不知道的同学可以百度安装。
如果已经安装好`npm`可以往下操作

1. `cd` /demo/JS  执行 `npm install`
2. `npm run server`



⚠️⚠️执行后，我们本地已经有可以执行`js`的环境了.
然后我们就可以在`/src`文件夹内修改`.js`源文件，修改后本地服务会自动执行打包更新并预览.


⚠️⚠️实际使用不要直接修改`outputs`目录, 因为每次`build`后 `outputs`目录会被全量替换

**关于build说明**
> 执行`npm run build` 将文件转成各自对应的js.

> 执行`npm run package` 将`src`目录下文件打包成一个文件.(demo中使用此种方式进行演示).

#### 使用模板 IV

初次使用可参照模板格式进行开发
```JavaScript
/**
 * 引入UI组件,不引入无法直接使用
 */ 
_import('UIView,UILabel,UIColor,UIFont,UIScreen,UIImageView,UIImage,UITapGestureRecognizer,UIButton,TTPlaygroundModel')

/**
 *  @params:1.要替换的Class名,`:`标识继承关系
 *  @params:2.声明实例方法
 *  @params:3.声明静态方法
 *  声明Class,如无需在Oc中动态创建,可不设置父类,直接在js中创建类
 *  声明Class,如Native不存在,则动态创建Class
 */
defineClass('TTPlaygroundController:UIViewController', {
    /**
	 * 添加属性,自动生成`setter`/`getter`方法,取值和赋值必须使用`setter`/`getter`方法.
	 */ 
	name: property(),
	/**
	 * 声明实例方法,如已存在则替换原有方法,如Native不存在,直接在js中添加方法实现
	 */ 
	viewDidLoad:function () {
	        /**
		 * Super 使用
		 */
		Super().viewDidLoad();
		/**
		 * self 使用
		 */ 
		self.loadJSCode();
	}
	/**
	 * 方法与方法之间 使用 , 分割
	 */
	,
	loadJSCode: function () {
  
	},
	/**
	 * 调用Obj-C传入的block
	 */
	callBlock_:function(callback){
		if(callback){
		   callback(10);
		}
	},
	/**
	 * Obj-C调用js传入block,并接受回调
	 */
	runBlock:function(){
		self.testCall2_(block("NSString*,NSString*"),function(arg){
			Utils.log_info('--------JS传入OC方法,接受到回调--------- 有参数,有返回值:string  '+arg);
			return '这是有返回值的哦';
		});
	}
}, {
	//静态方法
	testAction_:function (str) {
	}
})

```


> 您的喜欢就是我更新的动力

