<template>
  <div>
    <v-head></v-head>
    <div class="idle-top-wap">
      <div class="idle-top">
        <div class="left">
          <h5>发布垃圾</h5>
          <span>让别人买你的垃圾吧</span>
        </div>
        <div class="t_title">
          <i class="iconfont icon-smile"></i>
          <span>收！</span>
        </div>
      </div>
    </div>
    <div class="container">
         <el-upload
         class="upload"
          action="http://10.12.80.203/api/base/upload"
          :show-file-list="true"
          list-type="picture-card"
          :accept="'image/*'"
          :on-success="handleSuccess"
           :limit="1"
        >
          <el-button type="primary" size="medium" >上传商品图片</el-button>
        </el-upload>
      <el-form label-width="80px" :model="formLabel">
        <el-form-item label="闲置标题">
          <el-input v-model="formLabel.title" placeholder="请输入你的标题"></el-input>
        </el-form-item>
        <el-form-item label="闲置价格">
          <el-input v-model="formLabel.price" placeholder="请输入你的价格"></el-input>
        </el-form-item>
         <el-form-item label="咸鱼链接">
          <el-input v-model="formLabel.link" placeholder="请输入你的链接"></el-input>
        </el-form-item>
        <el-form-item label="闲置描述">
          <el-input v-model="formLabel.description" placeholder="请输入你的商品描述"></el-input>
        </el-form-item>
        <el-form-item label="闲置内容">
          <el-input
            type="textarea"
            placeholder="请输入内容，最多两百字哦~"
            v-model="textarea"
            maxlength="200"
            show-word-limit
            resize="none"
            rows="5"
            @input="onInput()"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="sub">立即发布</el-button>
        </el-form-item>
      </el-form>
    </div>
    <v-footer></v-footer>
  </div>
</template>
<script>
import axios from "axios";
import vHead from "../components/head";
import vFooter from "../components/footer";
export default {
  data() {
    return {
      formLabel: {
        title: "",
        textarea: "",
        description:"",
        price:"",
        link:""

      },
      bannerUrl:{},
    };
  },
  components: {
    vFooter,
    vHead
  },
  created() {},
  methods: {
        handleSuccess: function(result) {
      this.bannerUrl = result.data[0];
      console.log(result.data[0]);
    },
    onInput(){
     this.$forceUpdate();
},
    sub() {
      //提交内容
      if (
        localStorage.getItem("login") == null ||
        localStorage.getItem("login") == "false"
      ) {
        this.$message({
          message: "请先登陆！",
          type: "warning",
          offset: "80"
        });
      } else {
          console.log(this.bannerUrl)
        axios
          .post("http://10.12.80.203/api/Idle/add_idle", {
              img:this.bannerUrl,
              title: this.formLabel.title,
              content:this.formLabel.textarea,
              price:this.formLabel.price,
              description: this.formLabel.description,
              user_id:localStorage.getItem("userid"),
              link:this.formLabel.link
          })
          .then(res => {
            if(res.data.code){
               this.$message({
                message: "发布成功！",
                type: "success",
                offset: "80"
              });
             
            }else{
               this.$message({
                message: "发布失败",
                type: "warning",
                offset: "80"
              });
            }
          })
          .catch(res => {
             this.$message({
                message: "发布失败",
                type: "warning",
                offset: "80"
              });
          });
      }
    }
  }
};
</script>
<style scoped lang="scss">
.upload{
    margin-top: 50px;
    margin-left: 40%;
}
.el-form {
  margin-top: 50px;
  width: 500px;
  margin-left: 25%;
  button {
    float: right;
  }
}
.idle-top-wap {
  margin-top: 60px;
  padding: 0;
  width: 100%;
  height: 80px;
  background: #ffffff;
  box-shadow: 0 15px 15px 0 rgba(15, 37, 64, 0.1);
}
.idle-top {
  padding-bottom: 15px;
  padding-top: 25px;
  max-width: 1200px;
  margin: 0 auto;
  height: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #ffffff;
  z-index: 10;
  position: relative;
}
.left {
  float: left;
  display: flex;
  min-height: 100%;
  position: relative;
  font-size: 12px;
  text-align: center;
  line-height: 24px;
  h5 {
    font-size: 24px;
    color: #424e66;
    text-align: center;
    line-height: 24px;
    display: inline-block;
    padding-right: 10px;
    border-right: 1px solid #e5e5e5;
  }
  span {
    font-size: 16px;
    color: #666666;
    line-height: 24px;
    padding-left: 10px;
  }
}
span {
  i {
    margin-right: 14px;
  }
}
.t_title {
  span {
    color: #666666;
    font-size: 16px;
    line-height: 24px;
  }
}
</style>