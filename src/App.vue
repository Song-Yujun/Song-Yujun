<template>
  <h2>reactive</h2>
 <div>
    {{ userInfo.name }}
 </div>
 <button @click="changeName">修改名称</button>
 <hr>
<h2>reactive异步数组</h2>
<ul v-for="item in userList" :key="item.name">
  <li>{{ item.name }}</li>
</ul>
<button @click="changeListData">修改数据</button>
<h2>readonly只读</h2>
<div>{{copyPerson.name}}</div>
<button @click="changePerson">修改只读名称</button>
</template>

<script setup lang='ts'>
import { reactive,readonly, triggerRef } from 'vue';

// reactive 基本用法
type User = {
  name:string
  age:number
}
const userInfo = reactive<User>({name:"张三",age:19});
const changeName = ()=>{
  userInfo.name = "李四";
}

// reactive 深层代理，处理异步返回的数组数据
let userList = reactive<Array<User>>([]);
setTimeout(() => {
  const res = [{name:"张三",age:19},{name:"李四",age:20},{name:"王五",age:22}];
  userList.push(...res);
}, 200);
const changeListData = ()=>{
  userList.push({name:"赵六",age:23});
}

// readonly 只读
let person = reactive({name:'王德彪'});
let copyPerson = readonly(person);
const changePerson=()=>{
  // copyPerson.name = "李四";
}

// shallowReactive 浅层代理，只代理第一层数据
</script>

<style scoped>

</style>