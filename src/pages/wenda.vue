<template>
  <div>
    <v-head></v-head>
    <div class="idle-top-wap">
      <div class="idle-top">
        <div class="left">
          <h5>尾巴问答</h5>
          <span>烦恼的去处</span>
        </div>
        <div class="t_title">
          <i class="iconfont icon-smile"></i>
          <span>求求了帮帮我吧</span>
        </div>
      </div>
    </div>
    <div class="container">
      <!-- 问题框开始 -->
      <div class="row m">
        <div class="col-2"></div>
        <div class="col-8">
          <div class="ask"><div>你想问什么？</div> <b-link href="/post_question" target="_blank"> <el-button type="primary" round>提问</el-button></b-link></div>
          <div class="wenti" v-for="(item,index) in wt"   @click="toDetail(item.id)">
            <el-card shadow="hover">
              <div class="wt_title">{{item.content}}</div>
              <div class="wt_detail">{{item.description}}</div>
              <div class="wt_bottom">
                <div>
                  <i class="iconfont icon-fabulous">
                    <span>{{item.like_num}}</span>
                  </i>
                </div>
                <div>
                  <i class="iconfont icon-comment">
                    <span>{{item.comment_num}}</span>
                  </i>
                </div>
                <div>
                  <i class="iconfont icon-link">
                    <span>0</span>
                  </i>
                </div>
              </div>
              <!-- 精选评论 -->
              <div class="top-coment">
                <div class="jx">精选评论</div>
                <div class="jx-c" v-for="(item,index) in item.answer" :key="index">
                    <div class="tx">
                         <img :src="item.avatar"></img>
                     <div>
                        <div class="c-name">
                            {{item.responder}}
                        </div>
                        <div class="c-main">
                            {{item.content}}
                        </div>
                    </div>
                    </div>
                   
                    <div class="c-time">
                        {{item.create_time}}
                    </div>
                </div>
              </div>
            </el-card>
          </div>
        </div>
        <div class="col-2"></div>
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
import vFooter from "../components/footer";
import vBacktop from "../components/backtop";
export default {
  data() {
    return {
      wt: {},
       page: 1,
      page_count:333,
   
    };
  },
  components: {
    vFooter,
    vHead,
    vBacktop
  },
  created() {
    axios
      .post("http://10.12.80.203/api/Question/get_question_list", {})
      .then(res => {
        this.wt = res.data.question_list;
        this.page_count=res.data.total_page;
      
      })
      .catch(err => {});
  },
  methods: {
      //下一页
    loadMore(){
       this.page += 1;
       axios
        .post("http://10.12.80.203/api/Question/get_question_list", {
          
            page: this.page
          
        })
        .then(res => {
          this.wt= this.wt.concat(res.data.question_list);
          console.log(res.data)
        })
        .catch(e => {});
    },
      toDetail(id){
          console.log(id)
        this.$router.push({
        path: "wenda_detail",
        query: { id: id }
      });
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
.top-coment {
  padding: 20px;
  margin: 50px 20px 20px 20px;
  box-shadow: rgba(18, 18, 18, 0.1) 0px 1px 3px;
  border-radius: 6px;
  .c-name{
      padding-right: 10px;
      margin-bottom: 5px;
      color: #121212; 
      font-size: 14px;
    color: #999999;
  }
  .c-time{
      font-size: 14px;
    color: #999999;
      margin-top: 10px;
      width: 100px;
  }
  .tx{
     img{
          border-radius: 50%;
    width: 50px;
    min-width: 50px;
    height: 50px;
    border: 2px solid #ffffff;
     }
      div{
          margin-top: 5px;
          margin-left: 10px;
      }
      .c-main{
          margin-top: 15px;
          font-size: 14px;
    color: #333333;
      }
      margin-right: 10px;
      display: flex;
  }
  .jx {
    background-color: rgb(246, 246, 246);
    color: grey;
    padding: 10px 20px;
    font-size: 14px;
   
  }
  .jx-c{
      justify-content: space-between;
        display: flex;
      margin: 20px,10px;
      padding: 10px 20px;
  }
}
.wt_bottom {
  float: right;
  margin-top: 10px;
  margin-bottom: 10px;
  display: flex;
  div {
      margin-right: 20px;
    margin-left: 10px;
    span {
      margin-left: 5px;
    }
  }
}
.container {
  min-height: 500px;
}
.m {
  margin-top: 50px;
}
.wenti {
  margin-bottom: 20px;
  cursor: pointer;
}
.wt_title {
    padding-top: 10px;
  color: #121212;
  font-size: 18px;
  font: bold;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
}
.wt_detail {
    
    padding-top: 20px;
  color: #999999;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  cursor: pointer;
}
.ask {
    display: flex;
    justify-content: space-between;
  width: 100%;
  border-radius: 6px;
  background: #ffffff;
  padding: 20px;
  box-sizing: border-box;
  position: relative;
  margin-bottom: 50px;
}

// .wenti{
// width: 100%;
//   border-radius: 6px;
//   background: #ffffff;
//   padding: 20px;
//   box-sizing: border-box;
//   display: block;
//   position: relative;
//   cursor: pointer;
//   margin-bottom: 20px;
// }
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