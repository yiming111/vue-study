问题1:v-if和v-for哪个优先级高？如果两个同时出现，应该怎么优化得到更好的性能？

一、不应同时使用
v-for比v-if的优先级更高，这就说明在v-for的每次循环运行中每一次都会调用v-if的判断，所以不推荐v-if和v-for在同一个标签内同时使用。

二、解决方法

1、替换成计算属性

<ul>
	<li v-for="(item,id) in formList" :key="id"></li>
</ul>
 
//利用vue的计算属性，过滤掉不需要的数据，剩下需要的数据再利用v-for循环遍历取出渲染
computed: {
	formList: function () {
		return this.list.filter(function (item) {
			return item.state
		})
	}

2、把 v-if 改成 v-show。如果此 v-for上层已经有 v-for循环了，此处只是取了上层循环对象中的一个数组继续作循环，可以 v-if 改成 v-show，可以共存。
<div
  class="file_name"
  v-for="(fileMsg,index) in file.documents"
  :key="fileMsg.id"
  v-show="index < 2"
>
  <sys-file-layout :fileMsg="fileMsg"></sys-file-layout>
</div> 

3、将 v-if 置于外层元素 (或 <template>)上
<ul id="ul" v-if="todos.length">
<li v-for="todo in todos">
 {{ todo }}
</li>
<p v-else>
no todo left!
</p>
</ul>
  
  
<script> 
  
varvm=new Vue({
el:"#ul",
data:{
 todos: [ 1, 2, 3, 4, 5 ]
   }
})
  
</script>
