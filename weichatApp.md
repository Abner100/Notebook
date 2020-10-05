# 微信小程序

## navigator标签：

用于在小程序内部进行跳转,举例:

```html
<navigator url="" >跳转到xx<navigator>
```



## scroll-view标签

滚动视图，上下滚动或者左右滚动，

属性 scroll-x :左右滑动  scroll-y :上下滑动

举例

```html
<scroll-view scroll-x scroll-left="150px">
<view class="scrout">
    <view class="scrBox">111</view>
    <view class="scrBox">222</view>
    <view class="scrBox">333</view>
    <view class="scrBox">444</view>
    <view class="scrBox">555</view>
</view>
</scroll-view>
```

```css
.scrOut{ border:1px solid green; display: flex; flex-warp: nowrap;}
.scrBox{ width: 100px; height 100px; backgroup: glod; margin-right:2px; flex:0 0 100px}
```

## swiper标签：

用于制作轮播图

```html
<swiper class="banner" indicator-dots indicator-color= "rgba(255,255,255,0.5)" indicator-active-color="#fff" autoplay interval="3000" circular >
    <swiper-item>
        <image src="" mode= >
            11111
        </image>
    </swiper-item>
    <swiper-item>
        <image src="">
            11111
        </image>
    </swiper-item>
    <swiper-item>
        <image src="">
            2222
        </image>
    </swiper-item>
    <swiper-item>
        <image src="">
            3333
        </image>
    </swiper-item>
</swiper>
```

```css
.banner{height :300px ;border 1px solid red; }
.banner image{ width:100% height: 295rpx;}
```

## 常用的表单组件

button

```html
<button class ="btn1" type="primary">	登陆
</button>
```

checkbox 选择框

checkbox-group 选择框组

editor

form

```html
<form bindsubmit="onSumbit">
    <button form-type="sumbit">
        提交
    </button>
</form>
```

input

```html
<input class="" placeholder="xxxx"></input>
```

label

picker

picker-view

picker-view-column

radio

radio-group

radio-group

silder

switch

```html
<switch></switch>
```

textarea

```html
<textarea></textarea>
```

## 组件，其他类型的组件

icon 图标

progress 进度条 

```html
<progress percent="60" show-info font-size="15" border-radius="10" activeColor="pick" backgroundColor="#ccc" active bindactiveend="wangcheng"></progress>
```

## 框架

app.json全局设置

```html
"pages":{
	"pages/index/index",   //页面
},
"windows"{
	"backgroundTextStyle" : "drak",    //背景颜色
	"navigationBarBackgroundColor" :"#ccc",   //标题栏颜色
"navigationBarTitleText" :"小程序练习",       //小程序标题
"navigationBarTextStyle":"white",
		  //标题颜色
"enablePullDownRefresh":true
		  //是否开启刷新
},
"tabBar":{
	"color" :"#c30",  
	//导航栏颜色
	"selectedColor" :"#0ff",
	//选中后的颜色
	"backgroundColor" :"#ccc",
	//背景颜色
	"borderStyle" :"white",
	//边框颜色
	"list" :[
{
"pagePath":"pages/index/index",
//对应的地址
"text":"首页",
//名称
"iconPath" :" images/home.png",
//图标
"selectedIconPath": "images/home-hover.png"
//点击后的图标
		}，
{
"pagePath":"pages/logs/logs",
"text":"日志"
"iconPath" :" images/log.png",
"selectedIconPath": "images/log-hover.png"
		}，
]
//导航栏的内容
}
```

## 单页面配置 (每个页面的json文件)

  

```html
{
	"usingComponents":{}, 
	//自定义组件
	"navigationBarTitleTetx":"案例展示",
	//页面标题	"navigationBarBackgroundColor":"#000",
	//导航上栏的颜色
"enablePullDownRefresh":"true"，
	//下拉刷新
"backgroundColorTap":"#000"
	//顶部背景颜色
}
```

## WXML数据绑定

微信中每个页面的js文件中的pages大类下都有一个data数据，这个data数据即为当前页面的数据

```html
<view>数据传递展示{{title}}</view>
<view> {{content}}</view>
<view> {{listArr[0]}}</view>
<view>{{obj.name}} - {{obj.age}} -{{obj.sex}}</view>


在对应的js文件中
data:{
title:"数据绑定"，
content:"欢迎学习小程序",
listArr:["平面设计"，"UI设计","web前端"]，
obj:{name:"王进",age:18,sex"男"}
}，

输出即为“数据传递展示 数据绑定”
```

## WXML_if条件

```html
<view wx:if="{{day==1}}">周一</view>
<view wx:if="{{day==2}}">周二</view>
<view wx:if="{{day==3}}">周三</view>
<view wx:if="{{day==4}}">周四</view>
<view wx:if="{{day==5}}">周五</view>
<view wx:if="{{day==6}}">周六</view>
<view wx:if="{{day==7}}">周日</view>
//普通的if-else

<block wx:if="{{flase}}">
    <navigator url="xxxx">
        跳转1
    </navigator>
</block>
//整个块都消失
<block wx:else>
    <navigator url="xxxx">
        跳转2
    </navigator>
</block>
```



## WXML_for循环

```html
data:{
listArl:["平面设计","室内设计","UI设计","影视后期","web前端"]，
listObj:[
{title:"形式",time:"2020-01-02"},
{title:"形式",time:"2020-01-02"}，
{title:"形式",time:"2020-01-02"}，
{title:"形式",time:"2020-01-02"}
]
},

<view wx:for="{{listArr}}">{{index}} ,{{item}}</view>
//index表示索引值
//item 数组的每一项

<view wx:for="{{listArr}}"  wx:for-index="i" wx:for-item="content">
{{i}} .{{content}}
</view>
//for-index  自定义名字替换下键
//for-item   自定义数组的键值

<view wx:for={{listObj}}>
    <text>{{item.title}}</text>-
    <text>{{item.time}}</text>
</view>

  
```

## 框架接口

### page

onLoad 监听页面加载(一次)

onReady 监听页面初次渲染完成(一次)

onShow 监听页面显示(离开页面从其他页面进入)（多次）

onHide 监听页面隐藏（多次）

onPullDownRefresh 监听页面下拉刷新（多次）

onReadchBottom 下拉触底更新(多次)

onShareAppMessage 用户右上角分享

## 事件函数setData

所有的事件都需要添加bind，如bindtap，bindtouchmove

touchstart 

touchmove

touchcancel

touchend

tap

```html
<view bindtap="myTap">测试</view>


myTap:function(res){
console.log(res);
}
```

在函数中修改data数据的方法

```html
<view bindtap="myTap" data-name="测试"></view>
//data-name用于修改闯入的类中的数据

myTap:function(res){
var name = res.currentTarget.dataset.name
this.setData({
name:name，
})
}
```

## API基本使用

getSystemInfoSunc()    //返回设备信息

```html
onLoad:function(options){
console.log(wx.getSystemInfoSync()
}
```

showLoading与hideLoading  //加载与加载结束

shwoToast   //展示文本内容

```html
wx.showToadst({
title:"提交成功",
})
```

## API路由用法

  路由控制跳转

```html
clickBtn:funcyion(){
	wx.navigaTo({
		url:"pages/demo2/demo2"
		success:res=>{
		console.log(res)
		//返回函数
}
	}),     
}  //定义跳转函数，不能跳转tarbab页面

wx.switchTab()  //可以跳转到tarbab
wx.relaunch()  //可以带参数的跳转


<view bindtap="navBack">返回</view>
navBack:function(){
wx.navigateBack()
},
//返回上一页
```

  

## API模拟数据请求

 测试时设置为不校验合法域名 

```html
onLoad:function(options){
wx.request({
	url:"xxxxx",   //服务器的url
	data:{},       //传递给服务器的数
success:res=>{     //回调函数
console.log(res)
this.setData({
resData:res.data   //设置data中的数据
})
}
})
}
```

## 自定义组件

自定义组件的创建方法：
1.在根目录下创建components文件夹。（第一次的时候创建，以后就不需要再创建了）

2.在components下面创新组件名，如xsj，继续在xsj文件夹下单击右键，新建component。

3.在创建好的组件的wxml中写出你想显示的内容即可，在wXSS中编辑样式

4.比如首页想要调用xsj组件，那么就需要在index.json中进行配置。

5.在index.json中的调用方法，属性名是组件名，值就是组件的路径

```json
{"usingComponents": {
    "xjs":"components/xsj/xsj"
}}
```

6.在index.html中就可以使用了,如：

```html
<xsj></xsj>
或者
</xsj>
```

