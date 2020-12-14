<template>
  <div class="row footer">
    <div class="container">
      <div class="row">
        <div class="footer-l col-lg-4">
          <div>
            <h3>数字空间</h3>
          </div>
          <div class="icon row">
            <i
              v-b-tooltip.bottom.hover="{ variant: 'light' }"
              title="phone number:156798799"
              class="iconfont icon-lianxi rounded-lg"
            ></i>
            <i
              v-b-tooltip.bottom.hover="{ variant: 'light' }"
              title="wechat:99999999"
              class="iconfont icon-weixin rounded-lg"
            ></i>
            <i
              v-b-tooltip.bottom.hover="{ variant: 'light' }"
              title="sina:99999999"
              class="iconfont icon-weibo-fill rounded-lg"
            ></i>
          </div>
          <div v-html="footer.foot"></div>
          <!-- <v-html>{{footer.foot}}</v-html> -->
        </div>
        <div class="footer-r col-lg-8">
          <div class="row">
            <div class="col col-lg-6 partner">
              <p class="text-sm-center partner-title">合作伙伴</p>
              <p class="text-sm-center">戴伟冠</p>
              <p class="text-sm-center">徐帅</p>
              <p class="text-sm-center">梁明志</p>
              <p class="text-sm-center">龙友娜</p>
              <p class="text-sm-center">吴晓研</p>
            </div>
            <div class="col col-lg-6 connect">
              <p class="text-sm-left connect-title"><el-link href="/aboutUs"
                            target="_blank" :underline="false">关于我们</el-link></p>
              <p class="text-sm-left"> <el-link href="/connectUs"
                            target="_blank" :underline="false">联系我们</el-link></p>
              <!-- <p class="text-sm-left">科技就是生产力</p> -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "v-footer",
  data(){
    return{
      footer:{}
    }
  },
    created() {
  
     axios.post("http://10.12.80.203/api/Index/get_index_data").then(res => {
   
      this.footer = res.data.config_list;
      
    });
  },
};
</script>
<style scoped lang="scss">.footer {
  width: 100%;
  margin-right: 0;
  margin-left: 0;
  margin-top: 8%;
  height: 300px;
  background-color: #282d36;
}
.icon {
  margin: 0;
  i {
    float: left;
    display: inline-block;
    text-align: center;
    font-size: 20px;
    color: #fff;
    background: #3d424a;
    width: 40px;
    height: 40px;
    float: left;
    line-height: 40px;
    margin-top: 5%;
    margin-right: 3%;
  }
}
.footer-l {
  margin-top: 5%;
  p {
    color: #999;
    display: block;
  }
}
h3 {
  color: #fff;
}
.partner,
.connect {
  margin-top: 8%;
  line-height: 14px;
  color: #999;
  font-size: 14px;
}
.partner-title,
.connect-title {
  color: #fff;
}
.my-tooltip-class {
  color: #fff;
}
.row {
  overflow: hidden;
}

</style>