---
title: Day04
date: 2020-08-04
tags:
 - java
categories: 
 - Java
---

::: tip
你好
:::
<!-- more -->

![敲里吗](http://oss.zhijing657.cn/img/typora/0fa52ed6367b3f3b7129d4f033065988807d66c0.jpg)
# 一、ztree简介

## 1.什么是zTree

zTree 是一个依靠 jQuery 实现的多功能 “树插件”。优异的性能、灵活的配置、多种功能的组合是 zTree 最大优点。



## 2.zTree优点

采用了 延迟加载 技术，上万节点轻松加载，即使在 IE6 下也能基本做到秒杀

兼容 IE、FireFox、Chrome、Opera、Safari 等浏览器

支持 JSON 数据

支持静态 和 Ajax 异步加载节点数据

支持任意更换皮肤 / 自定义图标（依靠css）

支持极其灵活的 checkbox 或 radio 选择功能

提供多种事件响应回调

灵活的编辑（增/删/改/查）功能，可随意拖拽节点，还可以多节点拖拽哟

在一个页面内可同时生成多个 Tree 实例

简单的参数配置实现 灵活多变的功能



# 二、ztree使用

## 1.入门实例

### 1.1下载ztree资源https://gitee.com/zTree/zTree_v3

### 1.2解压资源并导入到项目

![1571084251670](assets/1571084251670.png)

将js、metro.css和metro.css对应的img文件夹复制到项目，注意保持css和img结构对应关系



### 1.3 示例

~~~
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="js/vue.js"></script>
		<!-- ztree提供三种风格css，需要与img文件夹对应上 -->
		<link href="static/ztree/metro.css"   rel="stylesheet"/>
<!-- 依赖jquery  必须导入 -->
		<script src="js/jquery-2.1.1.min.js"></script>
<!-- all.js = core + excheck + exedit ( 不包括 exhide ); -->
		<script src="static/ztree/jquery.ztree.all-3.5.js"></script>	
		
	</head>
	<body>
	
		<div id="treeDiv" style="height: 100px;">
           <input  style="width: 100px;" @focus="showTree(true)"  v-model="name" />
			   
	<!--ul用于生成菜单树 注意默认class为ztree   -->	   
           <ul id="treeDemo"  class="ztree" v-show="isShow" >
					
			</ul>
           
        </div>

	</body>
	<script>
		new Vue({
			el:'#treeDiv',
			data: {
				
				isShow:false,
					name:'',	
                    //zTree 的参数配置
					setting:{	

						data: {
							key: {
								title:"t"
							},
							simpleData: {
								enable: true//简单数据模式，传入数组自动转成树结构显示，必须有id、pId对应关系
							}
						}

					},
					
					//// zTree 的数据  
					zNodes:[
			{ id:1, pId:0, name:"节点搜索演示 1", t:"id=1", open:true},
			{ id:11, pId:1, name:"关键字可以是名字", t:"id=11"},
			{ id:12, pId:1, name:"关键字可以是level", t:"id=12"},
			{ id:13, pId:1, name:"关键字可以是id", t:"id=13"},
			{ id:14, pId:1, name:"关键字可以是各种属性", t:"id=14"},
			{ id:2, pId:0, name:"节点搜索演示 2", t:"id=2", open:true},
			{ id:21, pId:2, name:"可以只搜索一个节点", t:"id=21"},
			{ id:22, pId:2, name:"可以搜索节点集合", t:"id=22"},
			{ id:23, pId:2, name:"搜我吧", t:"id=23"},
			{ id:3, pId:0, name:"节点搜索演示 3", t:"id=3", open:true },
			{ id:31, pId:3, name:"我的 id 是: 31", t:"id=31"},
			{ id:32, pId:31, name:"我的 id 是: 32", t:"id=32"},
			{ id:33, pId:32, name:"我的 id 是: 33", t:"id=33"}
		],
					treeObj:''
				
			},
			methods:{
				initTree:function(){
				//根据选择器选中生成树的元素节点，传入设置和节点数据 生成ztree
					this.treeObj = $.fn.zTree.init($('#treeDemo'),this.setting,this.zNodes);
					console.log(this.treeObj);
				},showTree:function(flag){
					this.isShow=true;
				},

			
			
			},
			mounted:function(){//在节点创建后初始化ztree
				this.initTree();
		})
	</script>
	
</html>

~~~



![1571085000193](assets/1571085000193.png)



## 2.callback节点事件回调

ztree的setting属性，callback用于处理节点上的事件回调



案例：点击元素打印出选中元素id及元素节点数据

~~~
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="js/vue.js"></script>
		<!-- ztree提供三种风格css，需要与img文件夹对应上 -->
		<link href="static/ztree/metro.css"   rel="stylesheet"/>
<!-- 依赖jquery  必须导入 -->
		<script src="js/jquery-2.1.1.min.js"></script>
<!-- all.js = core + excheck + exedit ( 不包括 exhide ); -->
		<script src="static/ztree/jquery.ztree.all-3.5.js"></script>	
		
	</head>
	<body>
	
		<div id="treeDiv" style="height: 100px;">
           <input  style="width: 100px;" @focus="showTree(true)"  v-model="name" />
			   
	<!--ul用于生成菜单树 注意默认class为ztree   -->	   
           <ul id="treeDemo"  class="ztree" v-show="isShow" >
					
			</ul>
           
        </div>

	</body>
	<script>
		new Vue({
			el:'#treeDiv',
			//vue的data中，如果通过对象格式或者是箭头函数来声明绑定属性，在绑定属性中都不能使用this
			//解决方案：声明函数方式，通过return返回绑定属性
			data: function(){
				return {
				isShow:false,
					name:'',			
					setting:{	
						data: {
							key: {
								title:"t"
							},
							simpleData: {
								enable: true
							}
						},
						//beforeClick在点击事件执行前触发
						//onClick在点击事件执行时触发
						callback: {
							beforeClick: this.beforeClick,
							onClick: this.onClick
						}
					},
					zNodes:[
			{ id:1, pId:0, name:"节点搜索演示 1", t:"id=1", open:true},
			{ id:11, pId:1, name:"关键字可以是名字", t:"id=11"},
			{ id:12, pId:1, name:"关键字可以是level", t:"id=12"},
			{ id:13, pId:1, name:"关键字可以是id", t:"id=13"},
			{ id:14, pId:1, name:"关键字可以是各种属性", t:"id=14"},
			{ id:2, pId:0, name:"节点搜索演示 2", t:"id=2", open:true},
			{ id:21, pId:2, name:"可以只搜索一个节点", t:"id=21"},
			{ id:22, pId:2, name:"可以搜索节点集合", t:"id=22"},
			{ id:23, pId:2, name:"搜我吧", t:"id=23"},
			{ id:3, pId:0, name:"节点搜索演示 3", t:"id=3", open:true },
			{ id:31, pId:3, name:"我的 id 是: 31", t:"id=31"},
			{ id:32, pId:31, name:"我的 id 是: 32", t:"id=32"},
			{ id:33, pId:32, name:"我的 id 是: 33", t:"id=33"}
		],
					treeObj:''
				}
			},
			methods:{
				initTree:function(){
					this.treeObj = $.fn.zTree.init($('#treeDemo'),this.setting,this.zNodes);
					console.log(this.treeObj);
				},
				showTree:function(flag){
					this.isShow=true;
				},
				onClick:function(event,treeId,treeNode){
					console.log(treeId);
					console.log(treeNode);
				},
				beforeClick:function(treeId, treeNode, clickFlag) {
					console.log(treeId)
				}
			
				
				
			},
			mounted:function(){
				this.initTree();
		})
	</script>
	
</html>

~~~

![1571085593037](assets/1571085593037.png)



## 3.综合案例

点击输入框，显示菜单树，并且输入内容按回车后，实现模糊搜索查出节点名字，用红色字体显示这些节点

~~~
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="js/vue.js"></script>
		<!-- ztree提供三种风格css，需要与img文件夹对应上 -->
		<link href="static/ztree/metro.css"   rel="stylesheet"/>
<!-- 依赖jquery  必须导入 -->
		<script src="js/jquery-2.1.1.min.js"></script>
<!-- all.js = core + excheck + exedit ( 不包括 exhide ); -->
		<script src="static/ztree/jquery.ztree.all-3.5.js"></script>	
		
	</head>
	<body>
	
		<div id="treeDiv" style="height: 100px;">
           <input  style="width: 100px;" @focus="showTree(true)" @keydown.enter="search" v-model="name" />
			   
	<!--ul用于生成菜单树 注意默认class为ztree   -->	   
           <ul id="treeDemo"  class="ztree" v-show="isShow" >
					
			</ul>
           
        </div>

	</body>
	<script>
		new Vue({
			el:'#treeDiv',
			data: function(){
				return {
				isShow:false,
					name:'',			
					setting:{	
						data: {
							key: {
								title:"t"
							},
							simpleData: {
								enable: true
							}
						},
						callback: {
							beforeClick: this.beforeClick,
							onClick: this.onClick
						},
						view: {
							fontCss: this.setFontCss
						}

					},
					zNodes:[
			{ id:1, pId:0, name:"节点搜索演示 1", t:"id=1", open:true},
			{ id:11, pId:1, name:"关键字可以是名字", t:"id=11"},
			{ id:12, pId:1, name:"关键字可以是level", t:"id=12"},
			{ id:13, pId:1, name:"关键字可以是id", t:"id=13"},
			{ id:14, pId:1, name:"关键字可以是各种属性", t:"id=14"},
			{ id:2, pId:0, name:"节点搜索演示 2", t:"id=2", open:true},
			{ id:21, pId:2, name:"可以只搜索一个节点", t:"id=21"},
			{ id:22, pId:2, name:"可以搜索节点集合", t:"id=22"},
			{ id:23, pId:2, name:"搜我吧", t:"id=23"},
			{ id:3, pId:0, name:"节点搜索演示 3", t:"id=3", open:true },
			{ id:31, pId:3, name:"我的 id 是: 31", t:"id=31"},
			{ id:32, pId:31, name:"我的 id 是: 32", t:"id=32"},
			{ id:33, pId:32, name:"我的 id 是: 33", t:"id=33"}
		],
					treeObj:''
				}
			},
			methods:{
				initTree:function(){
					this.treeObj = $.fn.zTree.init($('#treeDemo'),this.setting,this.zNodes);
					console.log(this.treeObj);
				},
				showTree:function(flag){
					this.isShow=true;
				},
				onClick:function(event,treeId,treeNode){
					console.log(treeId);
					console.log(treeNode);
				},
				beforeClick:function(treeId, treeNode, clickFlag) {
					console.log(treeId)
				},
				search:function(){
					let nodes = this.treeObj.getNodesByParamFuzzy("name",this.name,null);
					// console.log(this.treeObj.getNodes());
					var treeNodes = this.treeObj.transformToArray(this.treeObj.getNodes());
					console.log(treeNodes);
					for (let i = 0; i < treeNodes.length; i++) {
						treeNodes[i].highlight = false;
						this.treeObj.updateNode(treeNodes[i]);
					}
					console.log(nodes);
					for( var i=0, l=nodes.length; i<l; i++) {
						nodes[i].highlight = true;
						this.treeObj.updateNode(nodes[i]);
					}
				},
				setFontCss:function(treeId, treeNode){
					return treeNode.highlight ? {color:"#A60000", "font-weight":"bold"} : {color:"#333", "font-weight":"normal"};
				}

			},
			mounted:function(){
				this.initTree();
				
				
			}
		})
	</script>
	
</html>

~~~





    