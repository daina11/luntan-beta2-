<template>
  <div class="content">
    <div class="container">
      <div class="row">
        <div class="col col-lg-6">
          <b-card
            @click="toDetail(Lcard.id)"
            :title="Lcard.title"
            :img-src="Lcard.background_img"
            img-alt="Image"
            img-top
            class="l-card"
            img-height="500px"
            img-width="390px"
            :class="active == Lcard.id ? 'addclass' : '' "
            @mouseover="dowm(Lcard.id)"
            @mouseleave="clearActive()"
          >
            <b-card-text>{{Lcard.description}}</b-card-text>
            <b-card-text class="small text-muted">{{Lcard.create_time}}</b-card-text>
            <b-badge pill class="badge">{{Lcard.column_name}}</b-badge>
          </b-card>
        </div>

        <div class="col col-lg-6">
          <!-- 第一行 -->
          <div class="row small-card">
            <b-card-group deck>
              <b-card
                img-alt="Image"
                img-top
                tag="article"
                style="max-width: 20rem;"
                class="mb-2"
                v-for="(item,index) in firstCol.slice(1 , 3)"
                :key="index"
                :img-src="item.background_img"
                img-height="163px"
                img-width="265px"
                :class="active == item.id ? 'addclass' : '' "
                @mouseover="dowm(item.id)"
                @mouseleave="clearActive()"
                @click="toDetail(item.id)"
              >
                <b-card-text>{{item.title}}</b-card-text>
                <b-card-text class="small text-muted">{{item.create_time}}</b-card-text>
                <b-badge pill class="badge">{{item.column_name}}</b-badge>
              </b-card>
            </b-card-group>
          </div>
          <!-- 第二行 -->
          <div class="row small-card">
            <b-card-group deck>
              <b-card
                img-alt="Image"
                img-top
                tag="article"
                style="max-width: 20rem;"
                class="mb-2"
                v-for="(item,index) in firstCol.slice(3 , 5)"
                :key="index"
                :img-src="item.background_img"
                img-height="163px"
                img-width="265px"
                :class="active == item.id ? 'addclass' : '' "
                @mouseover="dowm(item.id)"
                @mouseleave="clearActive()"
                @click="toDetail(item.id)"
              >
                <b-card-text>{{item.title}}</b-card-text>
                <b-card-text class="small text-muted">{{item.create_time}}</b-card-text>
                <b-badge pill class="badge">{{item.column_name}}</b-badge>
              </b-card>
            </b-card-group>
          </div>
        </div>
      </div>

      <!-- 底部两篇 -->
      <div class="row bSide">
        <div class="col col-lg-6" v-for="(item,index) in  firstCol.slice(5 , 7)">
          <b-card-group deck>
            <b-card
              :title="item.title"
              img-alt="Image"
              img-top
              :key="index"
              :img-src="item.background_img"
              img-height="395px"
              img-width="390px"
              class="mb-3"
              :class="active == item.id ? 'addclass' : '' "
              @mouseover="dowm(item.id)"
              @mouseleave="clearActive()"
              @click="toDetail(item.id)"
            >
              <b-card-text>{{item.description}}</b-card-text>
              <b-card-text class="small text-muted">{{item.create_time}}</b-card-text>
              <b-badge pill class="badge">{{item.column_name}}</b-badge>
            </b-card>
          </b-card-group>
        </div>
      </div>

      <!-- 新增闲置 -->
      <div class="new-b">
        <div class="row">
          <div class="col main">
            <!-- 活动卡片 -->
            <el-col :span="6" v-for="(item,index) in newdata" :key="index">
              <el-card shadow="hover" @click.native="toxianzhi(item.id)">
                <el-image :src="item.img"></el-image>
                <div class="title">{{item.title}}</div>
                <div class="time">{{item.create_time}}</div>
              </el-card>
            </el-col>
          </div>
        </div>
      </div>

      <!-- 新增兴趣 -->
      <div class="new-x">
        <div class="row">
          <div class="col main">
            <!-- 活动卡片 -->
            <el-col :span="8" v-for="(item,index) in xingqu" :key="index">
              <el-card shadow="hover" @click.native="tohuodong(item.id)">
                <el-image :src="item.img"></el-image>
                <div class="title">{{item.name}}</div>
                <div class="time"><div>开始时间：{{item.start_time}}</div><div>结束时间：{{item.end_time}}</div></div>
              </el-card>
            </el-col>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "v-icontent",
  data() {
    return {
      Lcard: [],
      firstCol: [],
      active: -1,
      newdata: {},
      xingqu:{}
    };
  },
  created() {
    axios.post("http://10.12.80.203/api/Index/get_index_data").then(res => {
      
      this.firstCol = res.data.article_list;
      this.Lcard = this.firstCol[0];
    });
    //请求闲置
    axios.post("http://10.12.80.203/api/Index/get_index_data").then(res => {
      console.log(res);
      this.newdata = res.data.idle_list;
      
    });
    //请求兴趣
    axios.post("http://10.12.80.203/api/Index/get_index_data").then(res => {
     
      this.xingqu = res.data.activity_list;
      
    });
  },
  methods: {
    tohuodong(id){
       this.$router.push({
        path: "activity_detail",
        query: { id: id }
      });
    },
    toxianzhi(id) {
      //跳转闲置页面
       this.$router.push({
        path: "xianzhi_detail",
        query: { id: id }
      });
    },
    dowm(index) {
      //将点击的元素的索引赋值给变量
      this.active = index;
    },
    clearActive() {
      //清空索引值
      this.active = -1;
    },
    toDetail(id) {
      this.$router.push({
        path: "article_detail",
        query: { id: id }
      });
    }
  }
};
</script>
<style lang="scss" >
.new-b {
  margin-bottom: 50px;
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
.new-x {
   margin-bottom: 50px;
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

.content {
  position: relative;
  top: 100px;
  overflow: hidden;
}
.small-card {
  min-height: 332px;
}
.l-card {
  max-width: 540px;
  max-height: 664px;
}
.mb-2 {
  padding: 10px;
}
.mb-3 {
  padding: 10px;
}
.card-title {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  -webkit-line-clamp: 1;
}
.card-text {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 1;
}
.addclass {
  margin-top: 0px;
  box-shadow: 0 0 20px 1px #cccaca;
  transition: all 0.3s ease 0s;
}
.bSide {
  margin-top: 2%;
  margin-bottom: 2%;
}
.badge {
  font-weight: lighter;
  font-size: 16px;
  position: absolute;
  top: 15px;
  left: 15px;
  line-height: 16px;
  background: rgba(0, 0, 0, 0.5);
  color: #ffffff;
  cursor: pointer;
  opacity: 0.8;
  padding-left: 15px;
  padding-right: 15px;
}
</style>