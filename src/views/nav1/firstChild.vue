

<template>
    <div>
        <div class="textOne">
           <el-input v-model="nowtext" ></el-input> 
           <el-button @click="handleClick" type="primary">点击</el-button>
        </div>
    </div>
</template>

<script>
import bus from '../../assets/bus.js'
export default {
    data(){
        return{
            nowtext:"",
        //       params:{
        //          name:'nameValue',
        //         code:10011
        // }
        }
    },
    mounted:function(){

    },
    methods:{
        handleClick () {
     // bus.$emit('getData', "ygd")
       this.$router.push({ path: '/secondChild', query: { userId: 123 }});
    },
//     beforeDestroy () {
        
//     console.log('A before destroy')
//    bus.$emit('getData', "ygd")
//   }
}
}
</script>

<style>
.textOne{
 display: flex;
 justify-content: space-around;
}
</style>
