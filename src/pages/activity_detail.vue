<template>
  <div>
    <v-head></v-head>
    <div class="idle-top-wap">
      <div class="idle-top">
        <div class="left">
          <h5>活动</h5>
          <span>欢聚·欢笑·兴趣</span>
        </div>
        <div class="t_title">
          <i class="iconfont icon-smile"></i>
          <span>我都想笑了</span>
        </div>
      </div>
    </div>
    <el-image style="width: 100%; height:300px" :src="activity.img"></el-image>
    <div class="container">
      <div class="row">
        <div class="col-2"></div>
        <div class="col-8">
          <div class="top">
            <div class="title">{{activity.name}}</div>
            <div class="ptime">发布时间：{{activity.start_time}}</div>
          </div>
          <div class="center">
            <span>活动时间</span>
            <div>开始时间：{{activity.start_time}}</div>
            <div>结束时间：{{activity.end_time}}</div>
            <div class="rs">招募人数</div>
            <div>{{activity.join_num}}人</div>
            <div class="dz">地址</div>
            <div>{{activity.address}}</div>
          </div>
          <div class="bottom">
            <div class="b-top">活动介绍</div>
            <div class="b-main">
              <div v-html="activity.content"></div>
            </div>
            <el-button v-if="isjion==false" type="primary" @click="canjia" class="canjia" round>参加活动</el-button>
            <!-- <el-button  type="primary" @click="canjia" class="canjia" round>已参加</el-button> -->
          </div>
          <div class="col-2"></div>
        </div>
      </div>
    </div>
    <v-backtop></v-backtop>
    <v-footer></v-footer>
  </div>
</template>

<script>
import axios from "axios";
import vHead from "../components/head";
import vAdd from "../components/add_top";
import vFooter from "../components/footer";
import vBacktop from "../components/backtop";
export default {
  data() {
    return {
      id: this.$route.query.id,
      activity: {},
      isjion: false
    };
  },
  created() {
    axios
      .post("http://10.12.80.203/api/Activity/activity_details", {
        user_id: localStorage.getItem("userid"),
        id: this.id
      })
      .then(res => {
        this.activity = res.data.article_details;
      })
      .catch(err => {});
  },
  components: {
    vHead,
    vAdd,
    vFooter,
    vBacktop
  },
  methods: {
    canjia() {
      console.log(this.activity.is_join);
      if (this.activity.is_join == 0) {
        this.$message({
          message: "你已经参加过本次活动啦！",
          type: "warning",
          offset: "80"
        });
      } else {
        axios
          .post("http://10.12.80.203/api/Activity/activity_details", {
            user_id: localStorage.getItem("userid"),
            id: this.id
          })
          .then(res => {
            this.activity = res.data.article_details;
            if (this.activity.is_join == 0) {
              this.$message({
                message: "你已经参加过本次活动啦！",
                type: "warning",
                offset: "80"
              });
            } else {
              axios
                .post("http://10.12.80.203/api/Activity/join_activity", {
                  user_id: localStorage.getItem("userid"),
                  id: this.id
                })
                .then(res => {
                  if (localStorage.getItem("login")) {
                    if (res.data.code) {
                      this.$message({
                        message: "参加成功！",
                        type: "success",
                        offset: "80"
                      });
                    } else {
                      this.$message({
                        message: "参加失败",
                        type: "warning",
                        offset: "80"
                      });
                    }
                  } else {
                    this.$message({
                      message: "请先登陆！",
                      type: "warning",
                      offset: "80"
                    });
                  }
                })
                .catch(err => {
                  this.$message({
                    message: "参加失败",
                    type: "warning",
                    offset: "80"
                  });
                });
            }
          });
      }
    }
  }
};
</script>
<style scoped lang="scss">
.canjia {
  margin-left: 40%;
}
.container {
  margin-top: 20px;
  .bottom {
    margin-top: 30px;
    margin-bottom: 50px;
    .b-main {
      margin-top: 20px;
    }
    .b-top {
      font-size: 14px;
      color: #333333;
    }
  }
  .center {
    padding-bottom: 20px;
    border-bottom: 1px solid #e5e5e5;
    .rs {
      font-size: 14px;
      color: #666666;
      margin-top: 30px;
    }
    .dz {
      font-size: 14px;
      color: #666666;
      margin-top: 30px;
    }
    padding-top: 20px;
    span {
      font-size: 14px;
      color: #666666;
    }
    div {
      margin-top: 5px;
      font-size: 18px;
      color: #333333;
    }
  }
  .top {
    border-bottom: 1px solid #e5e5e5;
    padding-top: 10px;
    padding-bottom: 30px;
  }
  .title {
    font-size: 40px;
    color: #0f2540;
    line-height: 56px;
  }
  .ptime {
    margin-top: 10px;
    font-size: 14px;
    color: #0f2540;
  }
}
.main {
  width: 100%;
  padding: 10px;
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