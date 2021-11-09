<template>
    <div>
        <section class="jumbotron">
            <h3 class="jumbotron-heading">Search Github Users</h3>
            <div>
                <input type="text" placeholder="enter the name you search" v-model="text"/>&nbsp;<button @click="SearchWorld">Search</button>
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios'
export default{
    name:"Search",
    data() {
        return {
            text:"",
        }
    },
    methods:{
        SearchWorld(){
            //等待加载
            this.$bus.$emit("UpdateLisData",{isFirst:false,isLoading:true,errMsg:'',users:[]})
            axios.get(`https://api.github.com/search/users?q=${this.text}`).then(
                response=>{
                    console.log("请求成功了",response.data.items);
                    //加载成功传值
                    this.$bus.$emit("UpdateLisData",{isLoading:false,errMsg:'',users:response.data.items})
                },
                error=>{
                    console.log("请求失败了",error.message);
                    //加载失败传值
                    this.$bus.$emit("UpdateLisData",{isLoading:false,errMsg:error.message,users:[]})
                }
            )
        }
    }
}
</script>

<style scoped>

</style>
