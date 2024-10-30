<template>
  <h2>ref</h2>
 <div>
    {{ message }}
 </div>
 <hr>
 <div>
  {{ hello }}
 </div>
 <button @click="changeMsg">修改ref</button>
 <hr>
 <h2>shallowRef</h2>
<div>{{ user.name }}</div>
<button @click="changeUserName">修改用户名称</button>
<hr>
<h2>triggerRef</h2>
<div>强制更新页面DOM</div>
<hr>
<h2>customRef</h2>
<div>{{ userName }}</div>
<button @click="changeName">修改自定义名称</button>
</template>

<script setup lang="ts">
import { ref,Ref,isRef,shallowRef, triggerRef, customRef } from 'vue';
type Obj ={
  name:string
  age:number
}

// ref 响应式数据
let message:Ref<string | number> = ref('你好');

let hello:string | number = "hello";

// shallowRef 浅层ref 只代理第一层数据
const user:Ref<Obj> = shallowRef({
  name:"张三",
  age:18
})

// isRef判断是否是ref类型
const changeMsg = () => {
  message.value = 111;
  hello = 123;
  console.log(isRef(message)); // true
  console.log(isRef(hello)); // false
}


const changeUserName = ()=>{
  user.value.name = "李四";
  // 强制执行更新
  triggerRef(user);
}


// 自定义customRef
function myRef<T>(value:T){
  let timer:any;
  return customRef((track,trigger)=>{
    return {
      get(){
        track();
        return value;
      },
      set(newValue){
        clearTimeout(timer)
        timer =  setTimeout(() => {
          console.log("触发了set");
          value = newValue;
          trigger();
        }, 1000);
      }
    }
  })

}
const userName = myRef<string>("王五");
const changeName = ()=>{
  userName.value = "赵六";
}

</script>

<style scoped>

</style>