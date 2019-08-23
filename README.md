

# Huilder X开发-猫耳APP（H5+/MUI/VUE）

## 前言

最早的App开发只有原生这个概念，Html页面只是用来做一些简单的静态资源展示，但是随着H5的兴盛，大家发现很多功能、逻辑都可用web来实现，然后原生作为容器显示，而且H5展示的页面更炫酷、功能更丰富，在IOS、Andriod中都有很好的支持，这样开发效率更高、成本更低，同时用户体验也不错。

近年来国内也出现了一些可以让前端人员编写移动端App的IDE，Hbuilder X是DCloud推出的一款免费开发工具，最大的亮点是可以开发App，利用html5+技术，结合mui+nativejs可以在云端打包，主要用到的技术就是HTML5、JS、CSS，一套代码，即可生成Android和IOS对应的两种App。



项目已上传github，欢迎大家下载交流。

前端项目地址：https://github.com/Hanxueqing/Maoer-App-HBuilder/network/alerts

在线项目手册：



## 项目技术栈

UI框架：MUI（官方推荐的模拟原生App的UI框架）

JS框架：VUE

API：H5+、Native.js（原生40万API随意调用）

编辑器：HBuilder，在5+ App项目下编写的HTML、js等文件，会被打包到原生的安装包（Android是apk包、iOS是ipa包）。



## 项目运行

```
# 克隆到本地
git clone git@github.com:Hanxueqing/Maoer-App-HBuilder.git

# 放到HBuilder环境下运行

# 使用数据线连接手机

# IOS系统在AppStore下载HBuilder插件

# 在HBuilder中输入ctrl+r开启真机演示
```



## 项目开发

### 环境搭建

#### 下载安装HBuilder X

在官网地址选择合适的版本下载安装：

http://www.dcloud.io/hbuilderx.html

![](http://ww3.sinaimg.cn/large/006y8mN6ly1g67kcqgob3j31gd0u0tkq.jpg)



#### 新建项目

打开HBuilder，在菜单栏中选择文件——新建——项目，选择5+App，创建一个mui项目，填写文件名称、保存位置，点击创建，会给你生成一个包含mui的js、css、字体资源的项目模版。

![](https://upload-images.jianshu.io/upload_images/2484592-1184280a0539256f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 文件结构

新建完成后，会在左侧的项目管理器中出现如下目录结构，跟我们平时做前端开发的项目类似。mainifest.json文件中存储的是app相关的配置。

![](https://upload-images.jianshu.io/upload_images/2484592-f9325ff24bba78f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 真机调试

使用数据线连接手机和电脑，在Android设备会自动安装并启动HBuilder调试基座，IOS系统的同学请下载一个名字叫HBuilder的调试插件，点击窗口上方的播放键小图标或者使用快捷键command+r在手机上运行。

真机运行有3个特点：  

1. 真实。虽然PC端HBuilder右侧的内置浏览器也可以看大致的页面，但真实的布局效果以及手机上的特殊能力调用，还是必须在真机测试。  
2. 边改边看。在HBuilder更改页面并保存后，可立即同步在真机上看到保存后的显示效果。比开发原生应用还方便。  
3. 检查错误和log。手机运行HTML等文件时如果发生错误以及打印的console.log，都可以在真机运行时从手机端反馈回到HBuilder的控制台，在控制台直接查看。
   注意只有移动App项目才可以真机联调。  



如果你真机失败，注意看控制台的提示，或点HBuilder菜单-运行里的故障排查指南。
**注意：真机联调App时，提供的是一个测试环境，并不真实发生打包，调试基座App的名字、图标、启动封面图片、是否可旋转这些只有打包才能更改的属性不会因为开发者修改manifest文件而变化。只有修改manifest且点击菜单发行-打包后，上述4个设置才能更改。**

运行后，HBuilder中修改页面代码，保存后会自动同步到手机中，如果手机当前展示着被修改的页面，则会刷新页面。尝试在js中在plus ready之后编写console.log，或者改写错误的js，可以直接在HBuilder的控制台看到结果。如果真机运行遇到各种故障，请点击运行菜单里的真机运行常见故障指南。  



### 底部Tab选项卡

#### 页面初始化

mui框架将很多功能配置都集中在mui.init方法中，要使用某项功能，只需要在mui.init方法中完成对应参数配置即可，目前支持在mui.init方法中配置的功能包括：创建子页面、关闭页面、手势事件配置、预加载、下拉刷新、上拉加载、设置系统状态栏背景颜色。

```
			 //mui初始化
			mui.init();
```



编写三个tab选项：首页、好玩、设置，在href中填写展示页面的id。

```
		<nav class="mui-bar mui-bar-tab">
			<!-- href写id -->
			<a id="defaultTab" class="mui-tab-item mui-active" href="home.html"> 
				<span class="mui-icon mui-icon-home"></span>
				<span class="mui-tab-label">首页</span>
			</a>
			</a>
			<a class="mui-tab-item" href="play.html">
				<span class="mui-icon mui-icon-paperplane"></span>
				<span class="mui-tab-label">好玩</span>
			</a>
			<a class="mui-tab-item" href="mine.html">
				<span class="mui-icon mui-icon-gear"></span>
				<span class="mui-tab-label">设置</span>
			</a>
		</nav>
```



#### 配置子页面

先通过`var self = plus.webview.currentWebview();`创建一个主窗口self，然后内部通过循环拿到三个子窗口，通过H5+方法 Webview——create创建新的Webview窗口，判断i是否大于0来判断当前窗口是否是第2、3窗口，如果是则隐藏，如果不是则说明为第一个子窗口，就追加到self主窗口中，并且通过subpage_style样式规定它在主窗口的展示位置。



**H5 + create方法**

`WebviewObject plus.webview.create( url, id, styles, extras );`

http://www.html5plus.org/doc/zh_cn/webview.html#plus.webview.create



```js
		<script type="text/javascript" charset="utf-8">
			 //mui初始化
			mui.init();
			// var subpages = ['tab-webview-subpage-about.html', 'tab-webview-subpage-chat.html', 'tab-webview-subpage-contact.html', 'tab-webview-subpage-setting.html'];
			//配置子页面
			var subpages = [
				{url:"./pages/home/",id:"home.html"},
				{url:"./pages/play/",id:"play.html"},
				{url:"./pages/mine/",id:"mine.html"},
			]
			var subpage_style = {//规定子窗口在主窗口中的位置
				top: '0px',
				bottom: '51px'
			};
			
			var aniShow = {}; //创建一个空对象
			
			 //创建子页面，首个选项卡页面显示，其它均隐藏；
			mui.plusReady(function() {//放到plusReady中才可以调用h5+的plus方法
				var self = plus.webview.currentWebview();//主窗口的对象
				for (var i = 0; i < 3; i++) {//循环三次
					var temp = {};
					// WebviewObject plus.webview.create( url, id, styles, extras );
					var sub = plus.webview.create(subpages[i].url+subpages[i].id,subpages[i].id,subpage_style);
					if (i > 0) { //第二个与第三个窗口隐藏
						sub.hide();//调用hide方法
					}else{
						temp[subpages[i].id] = "true"; //{home.html:"true"}
						mui.extend(aniShow,temp); //对象扩展 aniShow = {home.html:"true"}
					}
					self.append(sub);//子窗口添加到主窗口
				}
			});
```



在app开发中，对于HTML5+应用的页面有一个很重要的“plusready”事件，此事件会在页面加载后自动触发，表示所有HTML5+ API可以使用，在此事件触发之前不能调用HTML5+ API，若要使用HTML5+扩展api，必须等plusready事件发生后才能正常使用，所以应该在此事件回调函数中调用页面初始化需要调用的HTML5+ API，而不应该在onload或DOMContentLoaded事件中调用。mui将该事件封装成了`mui.plusReady()`方法，涉及到HTML5+的api，建议都写在mui.plusReady方法中。如下为打印当前页面URL的示例：

```
mui.plusReady(function(){
     console.log("当前页面URL："+plus.webview.currentWebview().getURL());
});
```

**如果手机版本是ios10+系统，即使不写plusready，内部也可以拿到plus对象，如果是安卓系统，系统报错plus is not defined说明找不到plus对象，需要将方法写在plusready中。**



通过`subpages[0].id`获取当前激活选项，通过事件委托，给所有的a标签动态绑定事件，让后续动态添加的元素也有之前的事件。

```
			 //当前激活选项
			var activeTab = subpages[0].id; //"home.html"
			 //选项卡点击事件
			 //事件委托 让后续动态添加的元素也有之前的事件
			mui('.mui-bar-tab').on('tap', 'a', function(e) { //给所有的a标签动态绑定事件
				var targetTab = this.getAttribute('href'); //获得href属性 "home.html"
				if (targetTab == activeTab) { //如果href属性和id相同
					return;
				}
```



#### 通过Mui.os判断平台

http://dev.dcloud.net.cn/mui/util/#os

我们经常会有通过`navigator.userAgent`判断当前运行环境的需求，mui对此进行了封装，通过调用mui.os.XXX即可。



如果是ios操作系统直接打开对应页面，如果是非ios系统并且第一次进入该页面，则以fade-in动画的形式打开。

```
			//显示目标选项卡
			//若为iOS平台或非首次显示，则直接显示
			
			//判断平台
			//如果是ios操作系统直接打开对应页面 如果是非ios系统并且第一次进入该页面 则以动画的形式打开
			if(mui.os.ios||aniShow[targetTab]){
				plus.webview.show(targetTab);
			}else{//如果是其他平台则以动画的形式打开
				//否则，使用fade-in动画，且保存变量
				var temp = {};
				temp[targetTab] = "true"; //temp = [“play.html”:"true"]
				mui.extend(aniShow,temp);//aniShow = ["home.html":"true","play.html":"true"]
				plus.webview.show(targetTab,"fade-in",300);
			}
```

**请注意，mui只封装了部分HTML5Plus Api，学会mui框架不代表可以不学习HTML5Plus规范。mui不会做的很重，只是很有限的通过封装简化了常见开发过程。  **



打开对应页面之后需要将之前激活的页面隐藏，然后将activeTab更改为当前的targetTab。

```
			//隐藏当前;
			plus.webview.hide(activeTab);
			//更改当前活跃的选项卡
			activeTab = targetTab;
```



最后通过自定义事件，模拟点击"首页选项卡"，实现当前点击的选项卡高亮显示。

			 //自定义事件，模拟点击“首页选项卡”
			document.addEventListener('gohome', function() {
				var defaultTab = document.getElementById("defaultTab");
				//模拟首页点击
				mui.trigger(defaultTab, 'tap');
				//切换选项卡高亮
				var current = document.querySelector(".mui-bar-tab>.mui-tab-item.mui-active");
				if (defaultTab !== current) {
					current.classList.remove('mui-active');
					defaultTab.classList.add('mui-active');
				}
			});



### Banner轮播图

#### 引入swiper

上bootcdn将swiper的样式和js文件复制到本地

https://www.bootcdn.cn/Swiper/



此项目我们要使用vue框架进行开发，所以将vue.js也复制到本地。

封装rem.js文件，实现移动端响应式布局。

```
document.documentElement.style.fontSize = 
	document.documentElement.clientWidth / 3.75 +"px"

window.onresize = function(){
	document.documentElement.style.fontSize = 
	document.documentElement.clientWidth / 3.75 +"px"
}

```



在homt.html中将文件依次引入：

```
		<link href="css/mui.css" rel="stylesheet" />
		<link rel="stylesheet" href="../../css/swiper.css">
		<script src = "../../js/rem.js"></script>
		<script src="../../js/mui.js"></script>
		<script src = "../../js/vue.js"></script>
		<script src = "../../js/swiper.js"></script>
```



编写banner结构

```
	<body>
		<div id = "app">
			<home-banner></home-banner>
		</div>
		
		<template id = "home-banner">
			<div class="home-banner swiper-container">
				<div class = "swiper-wrapper">
					<div class = "swiper-slide"></div>
				</div>
				<div class="swiper-pagination"></div>
			</div>
		</template>
			
			//注册home-banenr组件
			Vue.component("home-banner",{
				template:"#home-banner"
			})
			new Vue({
				el:"#app"
			})
		</script>
	</body>
```



#### 利用ajax方法请求数据

mui框架基于htm5plus的XMLHttpRequest，封装了常用的Ajax函数，支持GET、POST请求方式，支持返回json、xml、html、text、script数据类型；
本着极简的设计原则，mui提供了mui.ajax方法，并在mui.ajax方法基础上，进一步简化出最常用的mui.get()、mui.getJSON()、mui.post()三个方法。

http://dev.dcloud.net.cn/mui/ajax/



```
created(){
					mui.ajax('https://www.missevan.com/mobileWeb/newHomepage3',{
						dataType:'json',//服务器返回json格式数据             
						success:function(data){
							console.log(JSON.stringify(data))
						}
					});
				}
```





#### 获取数据

在data中声明一个空数组

```
data(){ //组件里面数据必须是函数的形式 为了让每一个实例可以获取一份被返回对象的独立的拷贝
					return{
						banners:[]
					}
				}
```



将获取到的数据赋值给banner，这里注意下this指向问题，不能写成普通函数，要写成箭头函数。

```
success:(data) => {
							// console.log(JSON.stringify(data))
							this.banners = data.info.banner
						}
```



在页面中利用v-for循环渲染数据

```
<template id = "home-banner">
			<div class="home-banner swiper-container">
				<div class = "swiper-wrapper">
					<div class = "swiper-slide"
						v-for= "(banner,index) in banners"
						:key = "index"
					>
						<img width = "100%" :src = "banner.pic" />
					</div>
				</div>
				<div class="swiper-pagination">
					
				</div>
			</div>
		</template>
```



现在banner还无法滑动，需要实例化，但是会出现轮播图划不动的现象。这是因为我们需要等到因数据改变，生成虚拟dom，对比完成之后生成真实dom再去进行实例化，所以我们要将实例化操作写在this.$nextTick中。

```
//数据改变,生成新的虚拟dom,与上一次虚拟dom结构做对比,对比完成之后,生成好了新的真实dom,然后在这个函数的回调函数内部就可以访问到因数据变化而渲染出来的真实dom结构了,所以就可以进行实例化相关的操作.
							this.$nextTick(()=>{
								new Swiper (".home-banner",{
									loop:true,
									pagination:{
										el:".swiper-pagination"
									}
								})
							})
```



效果演示：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g69d4twxsfg30ku112x6p.gif)



### 沉浸式导航栏

http://ask.dcloud.net.cn/article/421

此模式下应用占用全屏区域，而系统状态栏会拦截用户操作事件，此时需要预留出系统状态栏高度。
获取系统状态栏高度及沉浸式状态判断参考：

如何动态判断沉浸式状态栏模式：

http://ask.dcloud.net.cn/article/422



HBuilder创建的应用默认不使用沉浸式状态栏样式，需要进行如下配置开启：
 打开应用的manifest.json文件，切换到代码视图，在plus -> statusbar 下添加immersed节点并设置值为true。  

```
"plus" : {
        "statusbar" : {
            "immersed" : true
        }
 }
```

注意：  

1. 真机运行不生效，需提交App云端打包后才生效；  
2. 此功能仅在Android4.4及以上系统有效。  



#### navigator状态栏样式

设置系统状态栏样式

`void plus.navigator.setStatusBarStyle(style);`

http://www.html5plus.org/doc/zh_cn/navigator.html#plus.navigator.setStatusBarStyle

说明：设置应用在前台运行时系统状态栏的样式，默认值可通过manifest.json文件的plus->statusbar->style配置。 	

注意：此操作是应用全局配置，调用的Webview窗口关闭后仍然生效。 	

参数：

- style:  ( String) 必选* 系统状态栏样式
- 可取值："dark"：深色前景色样式（即状态栏前景文字为黑色），此时background建议设置为浅颜色；        "light"：浅色前景色样式（即状态栏前景文字为白色），此时background建设设置为深颜色；



在全局index.html中设置样式

```
		mui.plusReady(function() {
				//设置导航条的颜色
				plus.navigator.setStatusBarStyle("light")
		})
```



### my-sound内容区

#### 组件嵌套

分别编写my-sound-box、my-sound、my-sound-item组件，互相嵌套。

```
		<template id = "my-sound-box">
			<div class="my-sound-box">
				<my-sound>
				</my-sound>
			</div>
		</template>
		
		<template id = "my-sound">
			<div class="my-sound">
				<div class="panel-head">
					<p>日抓</p>
					<p>更多</p>
				</div>
				<div class="panel-body">
					<my-sound-item></my-sound-item>
				</div>
			</div>
		</template>
		
		<template id = "my-sound-item">
			<div class="my-sound-item">
				<div class="img-box">
					<img src="" alt="">
				</div>
				<div class="title"></div>
				<div class="detail">
					<span class="play-count"></span>
					<span class = "comments"></span>
				</div>
			</div>
		</template>
```



当宽度小于330px时利用媒体查询标签添加横向滚动条 

```
@media only screen and (max-width: 330px) {
				.my-sound .panel-body{
					justify-content: flex-start;
					overflow-x: auto;
				}
			}
```



#### 数据请求

在最外层组件my-sound-box中请求数据，将获取到的music赋值给sounds。

```
//注册my-sound-box组件
			Vue.component("my-sound-box",{
				template:"#my-sound-box",
				data(){
					return {
						sounds:[]
					}
				},
				created(){
					mui.ajax('https://www.missevan.com/sound/newhomepagedata',{
						dataType:'json',
						success:(data)=>{
							this.sounds = data.music
						}
					})
				}
			})
```



在my-sound-box模版中循环遍历sounds，并且将拿到的值传递给子组件my-sound：

```
<template id = "my-sound-box">
			<div class="my-sound-box">
				<my-sound
					v-for = "sound in sounds"
					:key = "sound.id"
					:sound = "sound">
				</my-sound>
			</div>
		</template>
```



my-sound通过props接收my-sound-box传递来的sound：

```
//注册my-sound组件
			Vue.component("my-sound",{
				template:"#my-sound",
				props:["sound"]
				
			})
```



再在my-sound模板中循环遍历sound.objects_point，并且将item传递给子组件my-sound-item：

```
<template id = "my-sound">
			<div class="my-sound">
				<div class="panel-head">
					<p>{{sound.title}}</p>
					<p>更多</p>
				</div>
				<div class="panel-body">
					<my-sound-item
						v-for = "item in sound.objects_point"
						:key = "item.id"
						:item = "item"
					></my-sound-item>
				</div>
			</div>
		</template>
```



my-sound-item通过props接收父组件my-sound传递过来的参数item，在自己的模板中打印对应的数据：

```
<template id = "my-sound-item">
			<div class="my-sound-item">
				<div class="img-box">
					<img :src="item.cover_image" alt="">
				</div>
				<div class="title">
					{{item.soundstr}}}
				</div>
				<div class="detail">
					<span class="play-count">{{item.view_count}}}</span>
					<span class = "comments">{{item.comment_count}}</span>
				</div>
			</div>
		</template>
```



**发现问题：图片加载不出来**

原因：图片地址不完整，需要手动拼接字符串

我们获取到的地址：201906/12/fdc535722aa97844750cbb3843c6ec22152202.jpg
实际图片地址：http://static.missevan.com/coversmini/201906/12/fdc535722aa97844750cbb3843c6ec22152202.jpg

解决办法：

为了方便维护，在my-sound-item中添加一个计算属性computed，直接返回拼接好的字符串：

```
Vue.component("my-sound-item",{
				template:"#my-sound-item",
				props:["item"],
				computed:{
					getImgUrl(){
						let baseDir = "http://static.missevan.com/coversmini/"
						return baseDir + this.item.cover_image
					}
				}
				
			})
```



然后前端页面直接调用计算属性即可，不需要打括号。

```
<img :src="getImgUrl" alt="">
```



#### filters过滤器

对请求到的数据进行优化，当数值大于10000时显示保留一位小数后加"万"的形式。

```
filters:{
					filterVal(val){
						if(val>10000){
							val = val/10000;
							val = val.toFixed(1);
							val = val+"万"
						}
						return val;
					}
				}
```



调用数据的时候在后面添加filters：

```
<div class="detail">
					<span class="play-count">{{item.view_count | filterVal}}</span>
					<span class = "comments">{{item.comment_count | filterVal}}</span>
</div>
```



#### 显示系统的等待对话框

showWaiting：显示系统等待对话框

http://www.html5plus.org/doc/zh_cn/nativeui.html#plus.nativeUI.showWaiting



在请求数据之前添加showWaiting等待框：

```
created(){
					plus.nativeUI.showWaiting("等待中...");
					mui.ajax('https://www.missevan.com/mobileWeb/newHomepage3',{
						dataType:'json',//服务器返回json格式数据             
						success:(data) => {
							// console.log(JSON.stringify(data))
							this.banners = data.info.banner
							//数据改变,生成新的虚拟dom,与上一次虚拟dom结构做对比,对比完成之后,生成好了新的真实dom,然后在这个函数的回调函数内部就可以访问到因数据变化而渲染出来的真实dom结构了,所以就可以进行实例化相关的操作.
							this.$nextTick(()=>{
								new Swiper (".home-banner",{
									loop:true,
									pagination:{
										el:".swiper-pagination"
									}
								})
							})
						}
					});
				}
```



设置一个标志isOk，默认是0:

```
let isOk = 0;
```



在数据请求到的时候，每请求一次执行执行isOk++，当isOk ===2时，执行关闭等待框的方法：

```
success:(data) => {
							// console.log(JSON.stringify(data))
							this.banners = data.info.banner
							//数据改变,生成新的虚拟dom,与上一次虚拟dom结构做对比,对比完成之后,生成好了新的真实dom,然后在这个函数的回调函数内部就可以访问到因数据变化而渲染出来的真实dom结构了,所以就可以进行实例化相关的操作.
							this.$nextTick(()=>{
								new Swiper (".home-banner",{
									loop:true,
									pagination:{
										el:".swiper-pagination"
									}
								})
							})
							isOk++
							if(isOk ===2 ){
								plus.nativeUI.closeWaiting("等待中...")
							}
						}
```



效果演示：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g69d5go2o8g30ku112npe.gif)



### 好玩页面

#### 创建页面

在play文件夹下，新建含mui的html页面，新建Vue实例挂载到div上。



#### 设置头部

使用mui自带header组件生成头部，添加`common-header`class名

```
<div id="app">
			<header class="mui-bar mui-bar-nav common-header">
				<a class  = "mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
				<h1 class="mui-title">好玩</h1>
			</header>
		</div>
```



#### 获取系统状态栏高度

此时头部被系统状态栏挡住，需要调整头部距离页面顶部的高度。



getStatusbarHeight：获取系统状态栏高度

http://www.html5plus.org/doc/zh_cn/navigator.html#plus.navigator.getStatusbarHeight



在mui.min.js中编写设置状态栏的方法

```
function setStatusBar(){
	let commonHeader = document.querySelector(".common-header");
	let status_bar = plus.navigator.getStatusbarHeight();
	commonHeader.style.paddingTop = statusbar + "px"
	commonHeader.style.height = 44 + status_bar + "px"
}
```



在前端页面created生命周期中调用此方法

```
new Vue({
				el:"#app",
				created(){
					setStatusBar()
				}
			})
```



#### musicbox

编写musicbox组件和music组件

```
<template id = "music-box">
			<div class="music-box">
				<music></music>
			</div>	
		</template>
```

在music-box中请求数据，编写getMusics请求数据的方法，并且将数据赋值给musics。在created中调用getMusics方法

```
Vue.component("music-box",{
				template:"#music-box",
				data(){
					return{
						musics:[]
					}
				},
				methods:{
					getMusics(){
						mui.ajax('https://www.missevan.com/explore/tagalbum',{
							data:{
								order:0
							},
							dataType:'json',//服务器返回json格式数据             
							success:(data) => {
								this.musics = data.albums
							}
						});
					}
				},
				created(){
					this.getMusics()
				}
			})
			
			Vue.component("music",{
				template:"#music",
				props:["music"]
			})
```



在music组件中接收父组件传递过来的music，渲染数据到页面上。

```
		<template id = "music-box">
			<div class="music-box">
				<music
					v-for = "music in musics"
					:key = "music.id"
					:music = "music"
				></music>
			</div>	
		</template>
		<template id="music">
			<div class="music">
				<div class="img-box">
					<img :src="music.front_cover" alt="">
				</div>
				<p class = "title">{{music.title}}</p>
			</div>
		</template>
```



**v-for指令循环渲染必须要设置key值**

1. 跟diff算法有关，为了提高效率

   如果在两个元素之间插入新元素，如果没有key的话需要把原位置的元素卸载了，把新元素插进来，然后依次卸载，会打乱后续元素的排列规则，如果有key值，只需要插入到对应位置即可，不会改变其他元素的走向。

2. key也为了减免一些出错问题

   例如在数组中，本来第一个是选中的，这时候我们再去添加新的元素，如果没有key的话那么新添加进来的元素就会被选中，加上key就是为了避免出现这样的问题。



#### 上拉加载功能

**单webview模式**

http://dev.dcloud.net.cn/mui/pulldown/#doubleWVpull

引入上拉刷新容器，放入我们的数据music-box。

```
<!--下拉刷新容器-->
			<div id="refreshContainer" class="mui-content mui-scroll-wrapper">
			  <div class="mui-scroll">
				<!--数据列表-->
				<music-box></music-box>
			  </div>
			</div>
```



初始化方法类似下拉刷新，通过mui.init方法中pullRefresh参数配置上拉加载各项参数

```
				created(){
					this.getMusics()
					mui.init({
					  pullRefresh : {
						container:"#refreshContainer",//待刷新区域标识，querySelector能定位的css选择器均可，比如：id、.class等
						up : {
						  height:50,//可选.默认50.触发上拉加载拖动距离
						  //默认启动的话就不需要执行this.getMusics()了
						  //auto:true,//可选,默认false.自动上拉加载一次
						  contentrefresh : "正在加载...",//可选，正在加载状态时，上拉加载控件上显示的标题内容
						  contentnomore:'没有更多数据了',//可选，请求完毕若没有更多数据时显示的提醒内容；
						  //不需要打括号了,打括号就立马执行了
						  callback :this.getMusics //必选，刷新函数，根据具体业务来编写，比如通过ajax从服务器获取新数据；
						}
					  }
					});
				}
```



当数据请求成功的时候执行，结束上拉刷新操作，并且执行this.p++，请求第二页数据，在参数中判断当前页码是否大于总页码，将请求到的数据使用concat方法拼接到原数组中，否则新请求到的数据会将原数据覆盖。将this.p 与data.pagination.maxpage（最大页数）做对比，当前页数大于最大页数的时候停止请求。

```
success:(data) => {
								this.musics = this.musics.concat(data.albums)
								this.p++;
								//若还有更多数据,则传入False,否则传入distribute
								mui('#refreshContainer').pullRefresh().endPullupToRefresh(this.p > data.pagination.maxpage);
							}
```



**双webview模式**

http://dev.dcloud.net.cn/mui/pulldown/

通过两个窗口来实现，在play.html中加载子页面play-content.html

主页面内容比较简单，就只有一个头部，在url中添加下拉刷新内容子页面地址。

```
new Vue({
				el:"#app",
				created(){
					// setStatusBar()
					mui.init({
						subpages:[{
						  url:"./play-content.html",//下拉刷新内容页面地址
						  id:"play-content.html",//内容页面标志
						  styles:{
							top:44 + plus.navigator.getStatusbarHeight(),//内容页面顶部位置,需根据实际页面布局计算，若使用标准mui导航，顶部默认为48px；
							bottom:0//其它参数定义
						  }
						}]
					});
				}
			})
```



创建子页面，下拉刷新的操作写在子页面中：

```
	<body>
		<div id="app">
			<!--下拉刷新容器-->
			<div id="refreshContainer" class="mui-content mui-scroll-wrapper">
			  <div class="mui-scroll">
				<!--数据列表-->
				<music-box></music-box>
			  </div>
			</div>	
			<!-- 置顶 -->
			<div @tap="backtop" class = "back-top-box" v-if="isShow">
				<div class = "back-top">
					<i class = "mui-icon mui-icon-arrowup"></i>
				</div>
			</div>
		</div>
		<template id = "music-box">
			<div class="music-box">
				<music
					v-for = "music in musics"
					:key = "music.id"
					:music = "music"
				></music>
			</div>	
		</template>
		<template id="music">
			<div class="music" @tap = "toAlbum(music.id)">
				<div class="img-box">
					<img :src="music.front_cover" alt="">
				</div>
				<p class = "title">{{music.title}}</p>
			</div>
		</template>
		<script src="../../js/mui.min.js"></script>
		<script src = "../../js/vue.js"></script>
		<script type="text/javascript">
			Vue.component("music-box",{
				template:"#music-box",
				data(){
					return{
						musics:[],
						order:0,
						p:1,
						tid:0
					}
				},
				mounted(){
					window.addEventListener("getTid",e=>{
						// console.log(e.detail.tid)
						this.changeType(e.detail.tid)
					})
				},
				methods:{
					changeType(tid){
						this.musics = [];
						this.p = 1;
						this.tid = tid
						this.getMusics();
						//重置上拉加载
						mui('#refreshContainer').pullRefresh().refresh(true);
						//需要实现滚动到顶部
						mui("#refreshContainer").pullRefresh().scrollTo(0,0);
					},
					getMusics(){
						// let {order,p} = this;
						let data;
						if(this.tid === 0){ //说明是全部分类
							data = {order:this.order,p:this.p}
						}else{
							data = {order:this.order,p:this.p,tid:this.tid}
						}
						mui.ajax('https://www.missevan.com/explore/tagalbum',{
							data,
							dataType:'json',//服务器返回json格式数据             
							success:(data) => {
								this.musics = this.musics.concat(data.albums)
								this.p++;
								//若还有更多数据,则传入False,否则传入distribute
								mui('#refreshContainer').pullRefresh().endPullupToRefresh(this.p > data.pagination.maxpage);
							}
						});
					}
				},
				created(){
					//this.getMusics()
					mui.init({
					  pullRefresh : {
						container:"#refreshContainer",//待刷新区域标识，querySelector能定位的css选择器均可，比如：id、.class等
						up : {
						  height:50,//可选.默认50.触发上拉加载拖动距离
						  //默认启动的话就不需要执行this.getMusics()了
						  auto:true,//可选,默认false.自动上拉加载一次
						  contentrefresh : "正在加载...",//可选，正在加载状态时，上拉加载控件上显示的标题内容
						  contentnomore:'没有更多数据了',//可选，请求完毕若没有更多数据时显示的提醒内容；
						  //不需要打括号了,打括号就立马执行了
						  callback :this.getMusics //必选，刷新函数，根据具体业务来编写，比如通过ajax从服务器获取新数据；
						}
					  }
					});
				}
			})
			
			Vue.component("music",{
				template:"#music",
				props:["music"],
				methods:{
					toAlbum(albumId){
						//打开album.html这个窗口
						mui.openWindow({
							url:"../album/album.html",
							id:"album.html",
							extras:{
								albumId
							},
							styles:{
						    	//设置一个渐变式导航栏
						    	"titleNView":{
									backgroundColor: '#234245',//导航栏背景色
				                    titleText: '猫耳FM',//导航栏标题
								    titleColor: '#fff',//文字颜色
								    type:'transparent',//透明渐变样式
								    autoBackButton: true,//自动绘制返回箭头
								    splitLine:{//底部分割线
								        color:'#cccccc'
								    }
								}
						    }
						})
					}
				}
			})
			
			new Vue({
				el:"#app",
				data:{
					isShow:false
				},
				methods:{
					backtop(){
						//需实现滚动到顶部
						mui('#refreshContainer').pullRefresh().scrollTo(0,0,100)
					}
				},
				mounted(){ //可以拿到真实dom
					//监听滚动事件
					document.querySelector('#refreshContainer' ).addEventListener('scroll', (e)=>{ 
					  var scroll = mui('#refreshContainer').pullRefresh(); //获取具体容器的滚动条
					  // console.log(scroll.y); 
					  if(scroll.y <= -300 && !this.isShow){
						  this.isShow = true;
					  }else if(scroll.y > -300 && this.isShow){
						  this.isShow = false;
					  }
					}) 

				}
			})
		</script>
	</body>
```

效果演示：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g69d61l419g30ku112hdw.gif)



#### 返回顶部

编写返回顶部按钮，添加点击事件。

```
<!-- 返回顶部 -->
			<div @tap="backtop" class = "back-top-box">
				<div class = "back-top">
					<i class = "mui-icon mui-icon-arrowup"></i>
				</div>
			</div>
```

编写返回顶部方法

```
methods:{
					backtop(){
						//需实现滚动到顶部
						mui('#refreshContainer').pullRefresh().scrollTo(0,0,100)
					}
				}
```



mui提供的返回顶部的方法

![](http://ww4.sinaimg.cn/large/006tNc79ly1g4jc7xwjbgj319u0e0ada.jpg)



一开始不让返回顶部按钮显示，在data中定义一个数据`isShow:false`，通过v-if指令来控制按钮的现实与隐藏，当滚动到一定高度的时候再显示出来。

```
			<div @tap="backtop" class = "back-top-box" v-if="isShow">
				<div class = "back-top">
					<i class = "mui-icon mui-icon-arrowup"></i>
				</div>
			</div>
```



在mounted钩子函数中监听滚动事件（注意不能写在created生命周期中，因为created中获取不到真实dom）

https://www.cnblogs.com/xzzzys/p/7597299.html

```
mounted(){ //可以拿到真实dom
					//监听滚动事件
						document.querySelector('#refreshContainer' ).addEventListener('scroll', function (e ) { 
						  var scroll = mui('#refreshContainer').scroll(); 
						  console.log(scroll.y); 
						}) 
				}
```



由于我们使用的是双web view模式，所以会出现两个滚动条，需要改成：

```
mounted(){ //可以拿到真实dom
					//监听滚动事件
					document.querySelector('#refreshContainer' ).addEventListener('scroll', function (e ) { 
					  var scroll = mui('#refreshContainer').pullRefresh(); //获取具体容器的滚动条
					  console.log(scroll.y); 
					}) 
				}
```



由于向下滚动是负值，所以需要判断，数值小于等于-300的时候给isShow赋值为true让返回顶部按钮显示，反之则为false，不显示。**同时需要将普通函数function改为箭头函数，否则this指向有问题。**

```
mounted(){ //可以拿到真实dom
					//监听滚动事件
					document.querySelector('#refreshContainer' ).addEventListener('scroll', (e)=>{ 
					  var scroll = mui('#refreshContainer').pullRefresh(); //获取具体容器的滚动条
					  // console.log(scroll.y); 
					  if(scroll.y <= -300){
						  this.isShow = true;
					  }else{
						  this.isShow = false;
					  }
					}) 
				}
```

为了不让isShow频繁的赋值，给if添加判断条件：

```
if(scroll.y <= -300 && !this.isShow){
						  this.isShow = true;
					  }else if(scroll.y > -300 && this.isShow){
						  this.isShow = false;
					  }
```

效果演示：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g69d9spgfgg30ku1127wj.gif)



### mine页面

#### 搭建页面

搭建mine我的页面，添加登录按钮，给登录按钮添加点击事件，点击跳转到login登录页面。

```
		<div id="app">
			<div class="user-info">
				<div class="login-info">
					<div class="img-box">
						<img :src="getUserimg" alt="">
					</div>
					<p v-if = "!userInfo" class = "login"><button @tap = "login ">登录</button></p>
					<p v-else class = "username">{{userInfo.nickname}}</p>
				</div>
				
				<div class = "exit" @tap= "exit"><i class = "mui-icon mui-icon-more"></i></div>
			</div>
		</div>
```



#### 登录功能

mui提供登录模版，右键——新建项目选择带登录和设置的MUI项目模板。

![](http://ww1.sinaimg.cn/large/006tNc79ly1g4i3572fyij312n0u0tcr.jpg)



我们需要在mine页面打开login新页面，将login.html页面添加到我们pages中login目录下，修改文件引入路径。



打开新页面方法：

http://dev.dcloud.net.cn/mui/window/#openwindow



在methods中编写打开login页面的方法：

```
new Vue({
				el:"#app",
				methods:{
					login(){
						mui.openWindow({
							url:"../login/login.html",
							id:"login.html",
							styles:{
							  top:0,//新页面顶部位置
							  bottom:0,//新页面底部位置
							},
							show:{
							  autoShow:true,//页面loaded事件发生后自动显示，默认为true
							  aniShow:"slide-in-top",//页面显示动画，默认为”slide-in-right“；
							  duration:2000//页面动画持续时间，Android平台默认100毫秒，iOS平台默认200毫秒；
							}
						})
					}
				}
			})
```



在h5+中查看AnimationTypeShow的方法：一组用于定义页面或控件显示动画效果

http://www.dcloud.io/docs/api/zh_cn/webview.html#plus.webview.AnimationTypeShow



#### 第三方登录

OAuth模块管理客户端的用户登录授权验证功能，允许应用访问第三方平台的资源。

**getServices:获取登录授权认证服务列表**

http://www.html5plus.org/doc/zh_cn/oauth.html



Hbuilder目前支持的第三方登录列表有QQ、微信、新浪微博，所以for循环之后就会把Hbuilder内部支持的第三方登录列表跟我们所期待的（var authBtns = ['qihoo', 'weixin', 'sinaweibo', 'qq'];）进行一个匹配，如果匹配上之后，它就会在页面上渲染图标，并且给图标自动绑定一个authId，这个authId和你提供的service.id匹配上了才追加上去，另外它还对weixin的id进行了强制性判断，如果service.id名称叫weixin并且未安装，就添加disabled禁用。

```
				$.plusReady(function() {
					plus.screen.lockOrientation("portrait-primary");
					//第三方登录  定义了需要支持的第三方登录名称
					var authBtns = ['qihoo', 'weixin', 'sinaweibo', 'qq']; //配置业务支持的第三方登录
					var auths = {};
					var oauthArea = doc.querySelector('.oauth-area');
					plus.oauth.getServices(function(services) {
						//终端支持的登录授权认证服务列表
						//所以hbuilder第三方服务认证列表目前支持的Services:weixin sinaweibo qq
						for (var i in services) {
							var service = services[i];
							auths[service.id] = service;//{weixin:weixinService,qq:qqService}
							if (~authBtns.indexOf(service.id)) { //==if (authBtns.indexOf(service.id) > -1)
								var isInstalled = app.isInstalled(service.id);
								var btn = document.createElement('div');
								//如果微信未安装，则为不启用状态
								btn.setAttribute('class', 'oauth-btn' + (!isInstalled && service.id === 'weixin' ? (' disabled') : ''));
								btn.authId = service.id;
								btn.style.backgroundImage = 'url("../../images/' + service.id + '.png")'
								oauthArea.appendChild(btn);
							}
						}
```

通过事件委托给按钮添加点击事件，通过getUserInfo方法获取用户信息，并存储在localStorage中。

```
//事件委托
						$(oauthArea).on('tap', '.oauth-btn', function() {
							if (this.classList.contains('disabled')) {
								plus.nativeUI.toast('您尚未安装微信客户端');
								return;
							}
							var auth = auths[this.authId];
							var waiting = plus.nativeUI.showWaiting();
							auth.login(function() {
								waiting.close();
								plus.nativeUI.toast("登录认证成功");
								auth.getUserInfo(function() {
									plus.nativeUI.toast("获取用户信息成功");
									var name = auth.userInfo.nickname || auth.userInfo.name;
									//nickname  headimgurl
									localStorage.userInfo = JSON.stringify(auth.userInfo)
								}, function(e) {
									plus.nativeUI.toast("获取用户信息失败：" + e.message);
								});
							}, function(e) {
								waiting.close();
								plus.nativeUI.toast("登录认证失败：" + e.message);
							});
						});
```

在mine页面中声明一个data，userInfo用来存放用户数据，在methods中添加从localStorage中获取用户信息的方法。

```
getUserInfo(){
						this.userInfo = JSON.parse(localStorage.userInfo ? localStorage.userInfo : "null")
					}
```

给p标签添加v-if/v-else指令，通过userInfo来控制登录按钮的显示与隐藏。

```
<p v-if = "!userInfo" class = "login"><button @tap = "login ">登录</button></p>
<p v-else class = "username">{{userInfo.nickname}}</p>
```

这时候系统会报错，说是没有办法给图片赋值为空，所以在页面中获取图片属性的时候，需要等到数据请求到时再获取，这时候添加一个计算属性getUserimg，判断一下是否有图片信息，如果没有图片信息则使用未登录默认图片。

```
computed:{
					getUserimg(){
						return this.userInfo?this.userInfo.headimgurl:"../../images/"
					}
				}
```

在页面中调用计算属性getUserimg，获取图片数据。

```
<img :src="getUserimg" alt="">
```

*** 注意：需要重新启动程序才可以看到头像和用户名，因为从mine页面跳入login窗口的时候，mine窗口没有被销毁，所以没有走created生命周期函数。**



**AuthService:登录授权认证服务对象**

http://www.html5plus.org/doc/zh_cn/oauth.html



#### 退出功能

编写exit方法，使用h5+的actionSheet方法

**actionSheet：弹出系统选择按钮框**

http://www.html5plus.org/doc/zh_cn/nativeui.html#plus.nativeUI.actionSheet

**ActionSheetCallback：系统选择按钮框的回调函数**

http://www.html5plus.org/doc/zh_cn/nativeui.html#plus.nativeUI.actionSheet



在回调函数中我们可以拿到用户点击的项目下标，数据类型为number，根据返回的数值进行switch判断，当点击注销登录的时候，清除localStorage中的用户信息，并且重新执行getUserInfo，当点击切换账号时直接跳转到登录页面（注意箭头函数的this指向问题）。

```
		exit(){
						plus.nativeUI.actionSheet(
							{
								title:"Plus is ready!",
								cancel:"取消",
								buttons:[
									{
										style:"destructive",
										title:"注销登录"
									},
									{
										title:"切换登录"
									}
								]},
								(e)=>{
									console.log("User pressed: "+e.index);
									switch(e.index){
										case 1:
											localStorage.removeItem("userInfo",null)//清除用户信息
											this.getUserInfo()//重新执行，页面重新渲染
											break;
										case 2: 
											this.login() //点击切换账号直接跳到登录页面
											break;
									}
								}
						);
					}
```



效果演示：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g69dfnqn6og30ku11210p.gif)



#### 解决登录后拿不到用户信息的问题

我们点击第三方登录，登录成功之后返回mine页面应该显示用户信息，但是没有显示，原因是这个组件没有进行销毁，没有销毁我们看不到结果，因为created只会执行一次，现在我们就要使用mui中提供的窗口之间的通信。

添加自定义事件

http://dev.dcloud.net.cn/mui/event/#customevent



我们在mine页面中的mounted钩子函数中添加一个监听自定义事件，等待这个事件被触发，初始化的时候会执行这个函数，定义一个方法："login：end"，回调函数中获取用户信息。

```
mounted(){
				//定义自定义事件
					window.addEventListener("login:end",e=>{
						this.getUserInfo()
						console.log(e.detail.a)
					})
				}
```



在login页面，登录成功之后需要调用mine页面中的login：end方法，通过mui.fire可以触发目标窗口的自定义事件。我们需要给它传递三个参数

![](http://ww3.sinaimg.cn/large/006tNc79ly1g4jbuzoy3bj31by0mmwh8.jpg)



由于我们之前在总的index.html中定义了id，所以这里可以通过getWebviewById的方法获得相应的webView

![](http://ww4.sinaimg.cn/large/006tNc79ly1g4jbvjoyalj310g08owgq.jpg)



```
//触发mine.html里面login:end方法
									let mine = plus.webview.getWebviewById("mine.html")
									mui.fire(mine,"login:end",{a:100})
```



效果演示：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g69dcx41dog30ku112toj.gif)



### 音单分类

#### 创建音单分类页面

在play文件夹中新建一个play-type页面，我们希望在play页面点击右上角三个点打开play-type页面，给a标签添加点击事件。

```
<a @tap = "changeType" class  = "mui-icon-more mui-icon mui-icon-left-nav mui-pull-right"></a>
```



在methods中编写changeType方法，调用mui中的打开新页面方法

http://dev.dcloud.net.cn/mui/window/#openwindow

```
				methods:{
					changeType(){
						//打开新窗口
						mui.openWindow({
							url:"./play-type.html",
							id:"play-type.html",
							styles:{
							  bottom:0,//新页面底部位置
							  height:260
							},
							show:{
							  autoShow:true,//页面loaded事件发生后自动显示，默认为true
							  aniShow:"slide-in-bottom",//页面显示动画，默认为”slide-in-right“；
							  duration:200//页面动画持续时间，Android平台默认100毫秒，iOS平台默认200毫秒；
							}
						})
```



#### 添加遮罩层

找到H5+中的Webview方法中的WebviewObject：Webview窗口对象，用于操作加载HTML页面的窗口

http://www.html5plus.org/doc/zh_cn/webview.html#plus.webview.WebviewObject



setStyle方法：

http://www.html5plus.org/doc/zh_cn/webview.html#plus.webview.WebviewObject.setStyle

查看传递的参数

![](http://ww2.sinaimg.cn/large/006tNc79ly1g4jdafnfopj315l0u0dmk.jpg)

通过currentWebview获得当前窗体，给当前窗体通过setStyle设置遮罩层。

```
//设置遮罩层
					let self = plus.webview.currentWebview()
					self.setStyle({mask:'rgba(0,0,0,0.5)'})
```



添加关闭遮罩层事件，当点击遮罩层的时候让遮罩层消失，并且让play-type页面关闭

```
mounted(){
					//绑定自定义事件
					let self = plus.webview.currentWebview()
					self.addEventListener('maskClick', function(){ //点击遮罩层
						self.setStyle({mask:'none'}); //让遮罩层消失
						plus.webview.getWebviewById("play-type.html").close();//让play-type窗口关闭
					},false);
				}
```



#### 请求数据

在data中声明musicType

```
data:{
					musicType:null
				}
```



在created钩子函数中使用ajax请求数据

```
created(){
					mui.ajax({
						url:"https://www.missevan.com/malbum/recommand",
						dataType:"json",
						success:(data)=>{
							console.log(JSON.stringify(data))
							//{"success":true,"info":{"情感":[[170,"热血"],[28,"治愈"],[4421,"抖腿"]],"场景":[[26310,"玩游戏"],[26311,"运动听"],[25,"作业向"]],"主题":[[370,"OP"],[376,"ED"],[273,"翻唱"],[5,"古风"],[850,"同人音乐"],[13349,"游戏原声"],[4,"广播剧"]]}}
							this.musicType = data.info;
						}
					})
				}
```



在页面中通过v-for循环渲染数据

```
<div class="play-type-box">
				<div 
					class="play-type"
					v-for = "(value,key,index) in musicType"
					:key = "index"
				>
					<span>{{key}}</span>
					<button
						v-for = "(item,i) in value"
						:key = "i"
					>{{item[1]}}</button>
				</div>
			</div>
```



#### 在头部插入一个数据

在musicType中添加一个"全部音单"数据

```
this.musicType["全部"] = [[0,"全部音单"]]
```



如果我们想让全部音单在前面显示就需要将这条语句写在前面，但是之后赋值会将它覆盖，所以我们需要将musicType赋值为空数组，然后用ES6中的展开符将它展开，然后再展开data.info。

```
this.musicType["全部"] = [[0,"全部音单"]]
this.musicType = {...this.musicType,...data.info}
```



或者通过ES5中的Object.assign方法将数据合并

```
//或者通过ES5中的assign方法
this.musicType = Object.assign({},this.musicType,data.info)
```



#### close关闭功能

添加一个点击事件，执行关闭功能

```
<div class="close" @tap="close">
					<i class = "mui-icon mui-icon-closeempty"></i>
				</div>
```



我们需要调用play.html中的方法来关闭play-type页面

在play中将关闭窗体的方法，单独封装为

```
closeType(self){
						self.setStyle({mask:'none'});//让遮罩层消失
						plus.webview.getWebviewById("play-type.html").hide();//让play-type窗口关闭
					}
```



在mounted钩子函数中调用，并且将"close：type"方法传递给play-type.html

```
mounted(){
					//绑定自定义事件
					let self = plus.webview.currentWebview()
					self.addEventListener('maskClick', (e)=>{//点击遮罩层
						this.closeType(self)
					},false);
					//绑定自定义事件
					window.addEventListener("close:type",e=>{
						this.closeType(self)
					})
				}
```



在play-type页面中的close方法中通过mui.fire来调用该方法

```
methods:{
					close(){
						//需要关闭遮罩层与play-type.html
						let play = plus.webview.getWebviewById("play.html")
						mui.fire(play,"close:type")
					}
				}
```



#### 默认选中全部音单

默认让它选中全部音单，在data中声明一条数据activeId，默认是0。

```
data:{
					musicType:{},
					activeId:0
				}
```



在button按钮上动态添加class，判断当前Id是否等于activeId，如果相等，则添加class。

```
<button
						v-for = "(item,i) in value"
						:key = "i"
						:class = "{'mui-btn-danger' : item[0] === activeId}"
					>{{item[1]}}</button>
```



给按钮添加点击事件，将当前id变成activeId，实现点击相应按钮出现选中状态。

```
<button
						v-for = "(item,i) in value"
						:key = "i"
						:class = "{'mui-btn-danger' : item[0] === activeId}"
						@click = "activeId = item[0]"
					>{{item[1]}}</button>
```



但是当我们关闭列表页面再打开的时候，选中状态又变回了全部音单，这是因为我们关闭列表页的时候这个组件被销毁了，activeId又变回了0，所以我们在closeType方法中不能使用close方法，需要使用hide隐藏方法。



hide：隐藏Webview窗口

http://www.html5plus.org/doc/zh_cn/webview.html#plus.webview.hide



```
closeType(self){
						self.setStyle({mask:'none'}); //让遮罩层消失
						// plus.webview.getWebviewById("play-type.html").close();//让play-type窗口关闭
						
						plus.webview.getWebviewById("play-type.html").hide();//让play-type窗口隐藏
					}
```



#### 点击切换相应音单

当我们点击按钮的时候activeId发生变化，这时候我们需要后面的play-content请求相应数据，所以ajax中的data需要发生变化，要获取url相应的id，在play-content的mounted中添加一个事件监听。

```
mounted(){
					window.addEventListener("getTid",e=>{
						console.log(e.detail.tid)
					})
				}
```



我们需要编写一个changeType方法，在mounted钩子函数中调用，并且将play-type传递过来的tid作为参数传递过去。

```
mounted(){
					window.addEventListener("getTid",e=>{
						// console.log(e.detail.tid)
						this.changeType(e.detail.tid)
					})
				}
```



在play-type添加一个watch监听，将activeId最新的值val传递给play-content，当我们的数据一旦变化它就可以获取到对应的tid。

```
watch:{
					activeId(val){
						let playContent = plus.webview.getWebviewById("play-content.html")
						mui.fire(playContent,"getTid",{tid:val})
					}
				}
```



编写changeType方法，在音单类型改变的时候需要将musics清空，p页码变为1，tid变为传递过来的tid，并且重新执行请求数据操作，调用getMusics方法。

```
changeType(tid){
						this.musics = [];
						this.p = 1;
						this.tid = tid
						this.getMusics();
					}
```



在getMusics中对data数据进行判断，当tid为0的时候显示全部音单，传递order和p字段过去，当tid不为0的时候显示相对应的数据，并且将tid传递过去。

```
let data;
						if(this.tid === 0){ //说明是全部分类
							data = {order:this.order,p:this.p}
						}else{
							data = {order:this.order,p:this.p,tid:this.tid}
						}
```



将data传递给mui.ajax：

```
						mui.ajax('https://www.missevan.com/explore/tagalbum',{
							data,
							dataType:'json',//服务器返回json格式数据             
							success:(data) => {
								this.musics = this.musics.concat(data.albums)
								this.p++;
								//若还有更多数据,则传入False,否则传入distribute
								mui('#refreshContainer').pullRefresh().endPullupToRefresh(this.p > data.pagination.maxpage);
							}
						});
```



#### 重启上拉加载

**出现问题：从别的页面跳转到全部音单页面上拉加载失效**

原因：如果别的页面只有一页数据，切换到全部音单的时候，也认为数据请求完毕了，就把上拉加载功能禁用了

解决办法：重置上拉加载

![](http://ww4.sinaimg.cn/large/006tNc79ly1g4jh1n0rwgj31cq0aywhf.jpg)

```
changeType(tid){
						this.musics = [];
						this.p = 1;
						this.tid = tid
						this.getMusics();
						//重置上拉加载
						mui('#refreshContainer').pullRefresh().refresh(true);
					}
```



**出现问题：从全部音单跳转到别的页面时数据从上方加载进来**

原因：全部音单的页码为第三页，跳转到别的页面的第一页

解决办法：每次切换的时候让它滚动到最上面，从头部开始加载数据

```
changeType(tid){
						this.musics = [];
						this.p = 1;
						this.tid = tid
						this.getMusics();
						//重置上拉加载
						mui('#refreshContainer').pullRefresh().refresh(true);
						//需要实现滚动到顶部
						mui("#refreshContainer").pullRefresh().scrollTo(0,0);
					}
```



#### 体验优化

每次切换完毕应该让列表页关闭

在play-type中的watch监听里调用下封装好的close方法，关闭遮罩与play-type窗口。

```
watch:{
					activeId(val){
						let playContent = plus.webview.getWebviewById("play-content.html")
						mui.fire(playContent,"getTid",{tid:val})
						//关闭遮罩与play-type窗口
						this.close()
					}
				}
```



在play-content中的上拉加载操作中有一个auto属性，如果注释掉请求数据的操作this.getMusics，将auto属性设置为true，会默认执行一次上拉加载，效果与请求数据操作相同。

```
created(){
					//this.getMusics()
					mui.init({
					  pullRefresh : {
						container:"#refreshContainer",//待刷新区域标识，querySelector能定位的css选择器均可，比如：id、.class等
						up : {
						  height:50,//可选.默认50.触发上拉加载拖动距离
						  //默认启动的话就不需要执行this.getMusics()了
						  auto:true,//可选,默认false.自动上拉加载一次
						  contentrefresh : "正在加载...",//可选，正在加载状态时，上拉加载控件上显示的标题内容
						  contentnomore:'没有更多数据了',//可选，请求完毕若没有更多数据时显示的提醒内容；
						  //不需要打括号了,打括号就立马执行了
						  callback :this.getMusics //必选，刷新函数，根据具体业务来编写，比如通过ajax从服务器获取新数据；
						}
					  }
					});
				}
```



效果演示：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g69ddoj69pg30ku1127wi.gif)



### album详情页

#### 点击跳转

在play-content页面中给每一个music专辑绑定一个点击事件toAlbum，在调用它的时候传递music.id。

```
<div class="music" @tap = "toAlbum(music.id)">
```



编写toAlbum方法，通过mui.openWindow添加打开新页面方法，将albumId传递给album，通过styles设置一个渐变式导航。

```
Vue.component("music",{
				template:"#music",
				props:["music"],
				methods:{
					toAlbum(albumId){
						//打开album.html这个窗口
						mui.openWindow({
							url:"../album/album.html",
							id:"album.html",
							extras:{
								albumId
							},
							styles:{
						    	//设置一个渐变式导航栏
						    	"titleNView":{
									backgroundColor: '#234245',//导航栏背景色
				                    titleText: '猫耳FM',//导航栏标题
								    titleColor: '#fff',//文字颜色
								    type:'transparent',//透明渐变样式
								    autoBackButton: true,//自动绘制返回箭头
								    splitLine:{//底部分割线
								        color:'#cccccc'
								    }
								}
						    }
						})
					}
				}
			})
```



在album页面中let self = plus.webview.currentWebview()，通过self.albumId就可以拿到传递过来的参数，参数命名的时候不要直接命名id，不然他会优先打印当前窗口文件的id（album.html），而不是你传递过来的参数。在mui.ajax中获取数据。

```
created(){
					let self = plus.webview.currentWebview()
					// console.log(self.albumId)
					mui.ajax({
						url:"https://www.missevan.com/sound/soundalllist",
						data:{
							albumid:self.albumId
						},
						dataType:"json",
						success:data => {
							this.album = data.info.album;
							this.owner = data.info.owner;
						}
					})
				}
```



在前端页面将数据渲染输出：

```
<div class="album-box">
				<div class="album-bg">
					<img :src="album.front_cover" alt="">
				</div>
				<div class="img-box">
					<img :src="album.front_cover" alt="">
				</div>
				<div class="album-info">
					<p class = "title">{{album.title}}</p>
					<p class="auther">
						<img class = "headimg" :src="owner.boardiconurl2" alt="">
						<span class = "nickname">{{album.username}}</span>
					</p>
				</div>
			</div>
```



#### 请求list数据

在data中声明sounds和sound数据：

```
data:{
					album:{},
					owner:{},
					sounds:[],
					sound:[]
				}
```



请求数据的时候给sounds赋值，并且通过.splice方法切分出十条数据复制给sound。

```
success:data => {
							this.album = data.info.album;
							this.owner = data.info.owner;
							this.sounds = data.info.sounds;//[0,115] ==> [10,115]
							this.sound = this.sounds.splice(0,10) //[0,9]
						}
```



在页面上渲染数据

```
<div class="album-list">
				<div class="album-list-item"
					 v-for = "item in sound"
					 :key = "item.id"
					 @tap = "toDetail(item.id)"
				>
					<div class="img-box">
						<img :src="item.front_cover" alt="">
					</div>
					<div class="album-detail">
						<p class = "title">{{item.soundstr}}</p>
						<p class="album-desc">
							<span class="play">{{item.view_count_formatted}}</span>
							<span class="time">{{item.duration}}</span>
						</p>
					</div>
				</div>
			</div>
```



#### 向下滚动获取数据

![](http://ww3.sinaimg.cn/large/006tNc79ly1g4klqunwnej30wa0u0gq2.jpg)



在data中声明cHeight和pHeight

```
data:{
					album:{},
					owner:{},
					sounds:[],
					sound:[],
					cHeight:"",
					pHeight:""
				}
```



在mounted钩子函数中绑定滚动事件，在当前窗口高度`document.documentElement.clientHeight`+滚动高度`document.body.scrollTop || document.documentElement.scrollTop()`+距离底部高度>整个文档的高度`document.documentElement.offsetHeight`的时候在之前的基础上追加十条数据。

```
mounted(){
					window.addEventListener("scroll",e=>{
						let sTop = document.body.scrollTop || document.documentElement.scrollTop()//获取滚动高度
						this.cHeight = document.documentElement.clientHeight;//获取当前可视区域的高度
						this.pHeight = document.documentElement.offsetHeight;//获取整个文档的高度
						if(this.cHeight+sTop+50 >= this.pHeight){//50：距离底部的高度
							// console.log("滚动到底部了")
							//在之前的基础上追加十条数据
							this.sound = this.sound.concat(this.sounds.splice(0,10))
						}
					})
				}
```



**发现问题：当数据加载完毕的时候向下滚动会继续加载**

解决办法：将滚动监听单独封装一个方法listenScroll

```
methods:{
					listenScroll(e){
						let sTop = document.body.scrollTop || document.documentElement.scrollTop;
						this.cHeight = document.documentElement.clientHeight;
						this.pHeight = document.documentElement.offsetHeight;
						if(this.cHeight+sTop+50 >= this.pHeight){
							console.log("滚动到底部了！")
							this.sound = this.sound.concat(this.sounds.splice(0,10))
						}
					}
				}
```



在页面初始化的时候添加事件监听。

```
mounted(){
					window.addEventListener("scroll",this.listenScroll)
				}
```



添加一个watch监听，监听sounds的变化，当sounds数组长度为0的时候，移除事件。

```
watch:{
					sounds(val){
						if(val.length === 0){
							window.removeEventListener("scroll",this.listenScroll)
						}
					}
				}
```



效果演示：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g69de8e0okg30ku112e82.gif)



#### 详情页

在album中添加点击事件toDetail，将detailId传递给detail.html。

```
				<div class="album-list-item"
					 v-for = "item in sound"
					 :key = "item.id"
					 @tap = "toDetail(item.id)"
				>
```



编写toDetail方法，打开对应的页面。

```
toDetail(detailId){
						mui.openWindow({
							url:"../detail/detail.html",
							extras:{
								detailId
							}
						})
					}
```



在详情页detail.html中通过self.detailId打开相应的目标窗口。

```
new Vue({
				el:"#app",
				created(){
					let self = plus.webview.currentWebview();
					mui.openWindow({
						url:"https://m.missevan.com/sound/"+self.detailId,
						id:"detail.html"
					})
				}
			})
```



现在我们想将头部的猫耳FM删掉

![](http://ww2.sinaimg.cn/large/006tNc79ly1g4kmz4b05vj30iw0ikjuj.jpg)



我们在控制台中输入两条语句，去掉当前窗口的头部导航栏

![](http://ww1.sinaimg.cn/large/006tNc79ly1g4kmz0ffvpj30r6058t9g.jpg)

![](http://ww2.sinaimg.cn/large/006tNc79ly1g4kmz2dvuvj30jg0kmjuv.jpg)



我们声明一个参数，返回一个窗体对象

```
let albumDetail = mui.openWindow({
						url:"https://m.missevan.com/sound/"+self.detailId,
						id:"detail.html"
					})
```



调用对象的onloaded方法，当窗体载入的时候执行这两条js语句

**evalJS**

调用evalJS在Webview窗口中执行JS脚本

http://www.html5plus.org/doc/zh_cn/webview.html#plus.webview.WebviewObject.evalJS

```
albumDetail.onloaded = function(){
						albumDetail.evalJS('document.getElementsByTagName("header")[0].innerHTML = "";document.getElementsByTagName("container")[0].style.padding = 0')
					}
```



在style中设置一个渐变式导航栏

```
styles:{
							//设置一个渐变式导航栏
							"titleNView":{
								backgroundColor: '#234245',//导航栏背景色
								titleColor: '#fff',//文字颜色
								type:'transparent',//透明渐变样式
								autoBackButton: true,//自动绘制返回箭头
								splitLine:{//底部分割线
									color:'#cccccc'
								}
							}
						}
```



#### 让头部显示对应标题

在success中通过self.setStyle方法设置成titleNView的样式，让头部显示对应的标题。

```
self.setStyle({
								"titleNView":{
									titleText:this.album.title
								}
							})
```



#### 优化时间显示方式

编写filter过滤器，将毫秒数转成"分钟：秒数"的形式。

```
filters:{
					timer(val){
						val = val/1000;
						let second = Math.ceil(val%60)
						let min = parseInt(val/60)
						second = second < 10 ? "0" + second:second
						return min + ":" + second
					}
				}
```

在时间展示的span标签中应用

```
<span class="time">{{item.duration | timer}}</span>
```



效果演示：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g69deyptczg30ku112b29.gif)



### 打包发布

#### 配置文件

上线发布之前我们需要在manifest.json中针对App进行设置，设置内容分别为：

- 基础配置：主要是基本的应用信息，包含；应用名称、版本号、入口文件、是否需要根据重力感应横竖屏。
- 图标配置：安装应用后，显示在主页的入口图标，可以根据你上传的大图标自动压缩生成各种小图标。
- 启动图配置：应用启动后展示给用户的图片。
- SDK配置：这里是你引用的第三方插件的配置，例如：支付、推送、分享等。
- 模块权限配置：你需要使用的原生模块，去掉不需要的模块，可以减少原生安装包的体积。
- App常用其它设置
- 源码视图

![](https://upload-images.jianshu.io/upload_images/2484592-eb7cabec6cb83f48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 云端打包

HBuilder提供的打包有云打包和本地打包两种。
HBuilder提供的云打包对正常开发者是免费的。但过多浪费服务器资源会额外收费。用本地打包无任何限制。
云打包的特点是DCloud官方配置好了原生的打包环境，可以把HTML等文件编译为原生安装包。  

在manifest.json中配置完毕就可以进行云端打包了，你只需要提交代码，不用部署xcode和Android sdk就可以打包应用。打包完毕下载安装，打包速度取决于你的网速。

文件名右键选择【发行】——原生APP云打包

![](http://ww3.sinaimg.cn/large/006tNc79ly1g4gw36b4osj30zs0mutlw.jpg)



选择IOS平台或者Andriod平台，如果只是自己测试可以使用DCloud的公用证书，但是不能发布上线。已经打好的安装包，允许开发者在指定天内下载指定次数。超时或超次后服务器端会清除文件。  

![](http://ww4.sinaimg.cn/large/006tNc79ly1g4gw48656gj30tu172gsw.jpg)



打包成功后会生成下载地址，点击下载后就可以进行安装使用了。

![](http://ww4.sinaimg.cn/large/006tNc79ly1g4gw3zmj5hj31t40ca7bz.jpg)