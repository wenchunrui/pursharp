---
title: vue初入坑 
tags: vue
grammar_cjkRuby: true
---

#### **简介**
1.jiavascript框架   
2.简化DOM操作  
3.响应式数据驱动

#### **使用初始**
导入开发版本的vue.js 
[vue.js官网](https://cn.vuejs.org/v2/guide/)

#### **el:挂载点**
1.vue实例的作用范围：vue会管理el选项命中的元素及内部的后代元素  
2.可以使用多种选择权，但是建议使用ID选择器    
3.可以设置于所有的双标签，但是不要设置于HTML和BODY  

``` javascript
var app = new Vue({
    el: "#id",
    data: {
        message: "测试"
    }
})
```

#### **data:数据对象**
1.vue中用到的数据定义在data中  
2.data中可以写复杂类型的数据  
3.渲染复杂类型数据时，遵守js的语法    

``` html
<html>
    <div id="app">
        {{ message }}
        <h2>{{ obj.name }} {{ obj.age }}</h2>
        <ul>
            <li>{{ list[0] }}</li>
            <li>{{ list[1] }}</li>
            <li>{{ list[2] }}</li>
        </ul>
    </div>
</html>
<script>
    var app = new Vue({
        el: "app",
        data: {
            message: "测试一下",
            obj: {
                name: "rui",
                age: "18"
            },
            list: ["red","blue","pink"]
        }
    })
</script>
```

#### **指令标签的学习**
**1.v-text:**
1.1.相当于textContent,设置标签内容  
1.2.默认写法会替换全部内容，使用差值表达式可以替换指定内容    
1.3.内部支持表达式 

``` html
<html>
    <div id="app">
        <h2 v-text="message"></h2>
        <h2 v-text="text+'!!'"></h2>
        <h2>{{ message }}</h2>
    </div>
</html>
<script>
    var app = new Vue({
        el: "app",
        data: {
            message: "测试一下",
            text: "测试第二下"
        }
    })
</script>
```
**2.v-html:**   
2.1.设置元素的innerHTML   
2.2.内容中有html结构会被解析为标签  
2.3.v-text指令无论内容是什么，只会解析为文本    
2.4.解析文本使用v-text，需要解析html结构使用v-html

``` html
<html>
    <div id="app">
        <p v-html="content"></p>
        <p v-text="content"></p>
    </div>
</html>
<script>
    var app = new Vue({
        el: "app",
        data: {
            content: "<a href='www.baidu.com'>百度一下</a>"
        }
    })
</script>
```
**3.v-on:**     
3.1.为元素绑定事件    
3.2.事件名不需要写on  
3.3.指令可以简写为@   
3.4.绑定的方法定义在methods属性中   
3.5.方法内部通过this关键字可以访问定义在data中得数据

``` html
<html>
    <div id="app">
        <input type="button" value="v-on指令" v-on:click="cs">
        <input typt="button" value="简写" @click="cs">
        <input type="button" value="双击事件" @dblclick="cs">
        <h2 @click="changeFn">{{ cs }}</h2>
    </div>
</html>
<script>
    var app = new Vue({
        el: "app",
        data: {
            cs: "测试数据"
        },
        methods: {
            cs:function(){
                alert("测试一下")
            },
            changeFn:(){
                this.cs+="!!!"
            }
        }
    })
</script>
```
**4.v-show:**   
4.1.根据元素的真假切换元素的现实状态  
4.2.原理是修改元素的display实现显示隐藏    
4.3.指令后面的内容最终都会解析为布尔值    
4.4.值为true元素显示，值为false元素隐藏   
4.5.数据改变之后，对应元素的显示状态会同步更新
 **5.v-if:**     
5.1.根据元素的真假切换元素的现实状态  
5.2.本质是通过操纵dom元素来切换显示状态    
5.3.表达式的值为true，元素存在dom树中，为false，从dom树中移除  
5.4.频繁的切换v-show，反之使用v-if，前者消耗性能比较少

 **6.v-bind:**   
6.1.为元素绑定属性    
6.2.完整写法是v-bind:属性名    
6.3.简写的话可以直接省略v-bind，只保留：属性名    
6.4.需要动态的增删class建议使用对象的方式

 **7.v-for:**    
7.1.根据数据生成列表  
7.2.数组经常和v-for综合使用   
7.3.语法是(item, index) in数据    
7.4.item和index可以结合其他指令一起使用   
7.5.数组长度的更新会同步到页面上,是响应式的

``` html
<body>
	<div id="app">
		<input type="button" name="" id="" value="add" @click="add" />
		<input type="button" name="" id="" value="remove" @click="remove" />
		<ul>
			<li v-for="(item,index) in arr">
				{{ index+1 }}城市:{{ item }}
			</li>
		</ul>
		<ul>
			<li v-for="(item, index) in obj" :title="item.name">{{item.age}}</li>
		</ul>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script type="text/javascript">
	var app = new Vue({
		el: '#app',
		data: {
			arr: ["北京", "上海", "广州", "深圳"],
			obj: [{
				name: "rui",
				age: 18
			}, {
				name: "min",
				age: 17
			}],
		},
		methods: {
			add: function() {
				this.obj.push({
					name: "bin",
					age: 19
				})
			},
			remove: function() {
				this.obj.shift();
			}
		}
	})
</script>
```
**8.v-on补充**  
8.1.事件绑定的方法写成函数调用的形式,可以传入自定义参数   
8.2.定义方法时需要定义形参来接收传入的实参    
8.3.事件的后面跟上.修饰符可以对事件进行限制   
8.4.**.enter**可以限制触发的按键回车  
8.5.事件修饰符有多种

``` html
<body>
	<div id="app">
		<input type="button" name="" id="" value="测试一下" @click="cs('测试两下',14)" />
		<input type="text" name="" id="" value="" @keyup.enter="sjdj"/>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script type="text/javascript">
	var app = new Vue({
		el: '#app',
		methods: {
			cs: function(a1, a2) {
				console.log("测试一下");
				console.log(a1);
				console.log(a2);
			},
			sjdj: function() {
				alert('!!')
			}
		}
	})
</script>
```
**10.v-model:**  
10.1.便捷的设置和获取表单元素的值  
10.2.绑定的数据会和表单元素值相关联    
10.3.绑定的值和修改的数据是双向绑定的,无论修改谁,另一个都会同步更新

``` html
<body>
	<div id="app">
		<input type="button" name="" id="" value="setM" @click="setM"/>
		<input type="text" v-model="message" @keyup.enter="getM">
		<h2>{{message}}</h2>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script type="text/javascript">
	var app = new Vue({
		el: '#app',
		data: {
			message: 'Hello Vue!'
		},
		methods: {
			getM: function(){
				alert(this.message)
			},
			setM: function(){
				this.message = "vue hello!"
			}
		}
	})
</script>
```

### **小demo**

``` html
<body>
	<div id="app">
		<header>
			<input type="text" name="" id="" value="" v-model="inputValue" @keyup.enter="add"/>
		</header>
		<section>
			<ul class="list">
				<li v-for="(item, index) in list">
					<span>{{ index+1 }}</span>
					<label>{{ item }}</label>
					<button @click="remove(index)" class="close">关闭</button>
				</li>
			</ul>
		</section>
		<footer v-show="list.length!=0">
			<p>{{ list.length }}条</p>
			<p @click="clear">clear</p>
		</footer>
	</div>
</body>
<script type="text/javascript">
	var app = new Vue({
		el: "#app",
		data: {
			inputValue: "拉克丝当减肥咯",
			list: ["你好", "不好"]
		},
		methods: {
			clear: function(){
				this.list = [];
			},
			add: function(){
				this.list.unshift(this.inputValue)
				this.inputValue = ""
			},
			remove: function(index){
				// console.log(index)
				this.list.splice(index, 1)
			}
		}
	})
</script>
```
### 网络应用

- axios     
1.axios必须先导入才可以使用  
2.使用get或post方法即可发送相对应的请求    
3.then方法中的回调函数会在请求成功或失败时触发    
4.通过回调函数的形参可以获取响应内容，或错误信息

**格式**：
``` javascript
axios.get(地址?key=value&key2=value).then(function(success){},function(err){})
axios.post(地址,{key=value&key2=value}).then(function(success){},function(err){})
```

##### **axios + vue**   
1.axios回调函数中的this已经改变，无法访问到data中的数据      
2.把this保存起来，回调函数中直接使用保存的this即可    
3.和本地应用最大的区别就是改变了数据来源

``` html
<body>
	<div id="app">
		<input type="button" name="" id="" value="获取内容" @click="getJoke"/>
		<p>{{ joke }}</p>
	</div>
</body>
<script type="text/javascript">
	var app = new Vue({
		el: "#app",
		data: {
			joke: "p容器"
		},
		methods: {
			getJoke: function(){
				var that = this;
				axios.get("https://autumnfish.cn/api/joke").then(function(res){
					console.log(res)
					that.joke = res.data
				},function(err){
					console.log(err)
				})
			}
		}
	})
</script>
```