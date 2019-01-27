<template>
    <div>
        <firstChild></firstChild>
        <secondChild></secondChild>  
    </div>
</template>

<script>
import firstChild from './firstChild'
import secondChild from './secondChild'
export default {
    data(){
        return{

        }
    },
    components:{
        firstChild,
        secondChild
    }
}
</script>

<style>

</style>
