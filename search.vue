

<template>

<div>

<br><br>
  <input
  type="text"
  id="search"
  placeholder="console data"
  @input="test2"
  class="mt-3"
    v-model="searchTerm2"
 
/>


 
  <li
    v-for="user in k"
    :key="user.name"
  >
    {{ user.name }} {{user.email}}
  </li>


</div>


</template>
<script>
import { computed, ref} from 'vue'

export default {
  setup() {
    const user = ref([]);
    const searchTerm=ref(null);
     const searchTerm2=ref(null);
    const k=ref('')
    
     
     let usersdata= (()=>{
       fetch(' https://jsonplaceholder.typicode.com/users').then((response) => response.json())
  .then((data) => {
  user.value.push(data);
  });
 });





/* -----------------------debouncing--------------------- */
const test2=debouncing(get,300);
function debouncing(fn,d)
  { 
    let timer;
    return(...args)=>{
    let context=this;
    clearTimeout(timer);
    timer=setTimeout(()=>{
      fn.apply(context,args);
      },d);
    };
  }
/* ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,, */


  function get(){

 if(searchTerm2.value=='')
 {
    k.value=[];
   return user.value
 }
 let n=user.value[0]?.filter(post =>{
       return(post.name.toLowerCase(),post.email.toLowerCase()).includes(searchTerm2.value?.toLowerCase());
});
 if(n.length>3){
      n=n.slice(0,10);
     }

k.value=n;
  }
usersdata();
const searchUser= computed(()=>{
 let t=user.value[0]?.filter(post => {
        
        return((post.name.toLowerCase(),post.email.toLowerCase()).includes(searchTerm.value?.toLowerCase()));
      })
      return t;
    });

return {searchTerm,user,searchUser,k,test2,searchTerm2}
  }
}
</script>
