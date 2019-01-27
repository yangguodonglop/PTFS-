

<template>
    <div>
        <div class="textOne">
            <p>{{student.nowvalue}}</p>
           <!-- <el-input v-model="nowtext" ></el-input> 
           <el-button @click="getData()" type="primary">点击</el-button> -->
        </div>
    </div>
</template>

<script>
import bus from '../../assets/bus.js'
export default {
    data(){
        return{
           
              student:{
                nowvalue:"888888",
              }  
            
            
        }
    },
    beforeCreate:function(){
        alert(this.nowvalue)
        this.nowvalue= this.$route.query.userId
      
    },

    mounted:function(){
        
     this.$set(this.student,"nowvalue", 24)
     alert(JSON.stringify(this.student))
    //   bus.$on("clickValues",function(msg){
    //       //_this.gettext=msg;
    //     //   this.$set( this.arr,"nowvalue", msg)
    //     //   alert(JSON.stringify(this.arr))
    //   })
    },
    methods:{
    }
        // getData:function(){
    
        //     bus.$emit("clickValues",this.nowtext)
        // }
    }

</script>

<style>
.textOne{
 display: flex;
 justify-content: space-around;
}
</style>
