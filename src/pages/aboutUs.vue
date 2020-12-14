<template>
    <div>
        <v-head></v-head>
       <div class="container" v-html="data"></div>
       <v-backtop></v-backtop>
       <v-footer></v-footer>
    </div>
</template>
<script>
import axios from "axios";
import vHead from "../components/head";
import vFooter from "../components/footer";
import vBacktop from "../components/backtop";
export default {
    data(){
        return{
            data:{}
        }
    },
      components: {
    vFooter,
    vHead,
    vBacktop
  },created(){
       axios.get("http://10.12.80.203/api/Index/get_about",{

      }).then(res=>{
        console.log(res)
        this.data=res.data.about
      }).catch(err=>{

      })
  }
}
</script>
<style scoped lang="scss">
.container{
    min-height: 500px;
    padding-top: 100px;
}
</style>
