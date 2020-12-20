<template>
  <div>
    <v-head></v-head>
    <div class="idle-top-wap">
      <div class="idle-top">
        <div class="left">
          <h5>闲置</h5>
          <span>交易你的垃圾</span>
        </div>
        <div class="t_title">
          <i class="iconfont icon-smile"></i>
          <span><el-link href="/post_xianzhi" target="_blank" :underline="false"><el-button type="primary" class="canjia"  round>发布垃圾</el-button></el-link></span>
        </div>
      </div>
    </div>

    <div class="container">
      <div class="row">
        <div class="col main">
          <!-- 活动卡片 -->
          <el-col :span="6" v-for="(item,index) in xianzhi" :key="index">
            <el-card shadow="hover" @click.native="todetail(item.id)">
              <el-image :src="item.img"></el-image>
              <div class="title">{{item.title}}</div>
              <div class="time">{{item.update_time}}</div>
            </el-card>
          </el-col>
        </div>
      </div>
      <b-button
        pill
        variant="outline-secondary"
        class="load-more"
        v-if="page<page_count"
        @click="loadMore"
      >加载更多...</b-button>
      <b-button pill variant="outline-secondary" class="load-more" v-else>没有更多了</b-button>
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
      xianzhi: {},
       page: 1,
      page_count:333
    };
  },
  created() {
    axios
      .post("http://10.12.80.203/api/idle/get_idle_list", {})
      .then(res => {
        this.xianzhi = res.data.idle_list;
         this.page_count=res.data.total_page
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
       todetail(id) {
      //跳转闲置页面
       this.$router.push({
        path: "xianzhi_detail",
        query: { id: id }
      });
    },
    //下一页
    loadMore() {
      this.page += 1;
      axios
        .post("http://10.12.80.203/api/idle/get_idle_list", {
          page: this.page
        })
        .then(res => {
          this.xianzhi = this.xianzhi.concat(res.data.idle_list);
          console.log(res.data);
        })
        .catch(e => {});
    }
  }
};
</script>
<style scoped lang="scss">
.load-more {
  margin-top: 50px;
  margin-left: 45%;
  bottom: -50px;
}
.more {
  margin-top: 50px;
  margin-left: 50%;
  margin-bottom: 50px;
}
.main {
  margin-top: 50px;
  .el-card {
    cursor: pointer;
    border-radius: 10px;
    padding: 0px;
    margin: 15px;
    .el-image {
      border-radius: 5px;
      width: 100%;
      height: 185px;
    }
    .title {
      font-family: PingFangSC-Medium;
      font-size: 16px;
      color: #333333;
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 1;
    }
    .time {
      font-size: 12px;
      color: #999999;
      margin-top: 10px;
      margin-bottom: 10px;
    }
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