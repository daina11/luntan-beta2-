<template>
  <div>
    <v-head></v-head>
    <div class="idle-top-wap">
      <div class="idle-top">
        <div class="left">
          <h5>兴趣</h5>
          <span>随时·发现·分享</span>
        </div>
        <div class="right-w">
          <b-link href="/interest" target="_blank" class="right iconfont icon-favorite">兴趣首页</b-link>
        </div>
      </div>
    </div>

    <div class="a">
      <div class="col-2"></div>
      <div class="col-5">
        <div class="interset-list-warp">
          <div class="interset-content-top">
            <div class="xx">
              <div class="tx">
                <img :src="user.avatar" alt />
              </div>
              <div class="imf">
                <div class="user_name">{{user.name}}</div>
                <div class="post_time">{{content.create_time}}</div>
              </div>
            </div>
            <div class="more">
              <i class="iconfont icon-elipsis"></i>
            </div>
          </div>
          <div class="interset-content">
            <div class="interset-main">{{content.content}}</div>
            <div class="interset-img">
              <el-image
                v-for="(item,index)  in content.img"
                :key="index"
                :src="item"
                block="true"
                fit="fill"
                style="width: 100px; height: 100px;padding:5px;"
              ></el-image>
            </div>
          </div>
          <div class="interset-content-footer">
            <div>
              <i class="iconfont icon-fabulous" @click="dianzan">
                <span>{{data.like_num}}</span>
              </i>
            </div>
            <div>
              <i class="iconfont icon-comment">
                <span>{{data.comment_num}}</span>
              </i>
            </div>
            <div>
              <i class="iconfont icon-link">
                <span>0</span>
              </i>
            </div>
          </div>
        </div>
       <!-- 添加评论开始 -->
            <div class="addcomment">
              <h5>说出你的心声吧</h5>
              <div class="comment-b">
                <!-- 头像 -->
                <div class="addcomment-left">
                  <img :src="touxiang" alt />
                </div>

                <!-- 输入评论框 -->
                <div class="addcomment-right">
                  <div v-if="login">
                    <textarea
                      v-model="comment"
                      class="form-control"
                      id="exampleFormControlTextarea1"
                      rows="2"
                      cols="200"
                      placeholder="你想说什么呢?"
                    ></textarea>
                    <span class="sub" @click="submit()">评论</span>
                  </div>
                  <div v-else="login==false ">
                    <textarea
                      class="form-control"
                      id="exampleFormControlTextarea1"
                      rows="2"
                      cols="200"
                      placeholder="请先登陆!"
                      readonly
                    ></textarea>
                  </div>
                </div>
              </div>

              <!-- 评论列表开始 -->
              <div class="comment-list">
                <div class="new-list">
                  <h5>最新评论</h5>
                </div>
                <div v-for="(item,index) in commentlist" class="comment-main">
                  <div class="tx">
                    <img :src="item.avatar" alt />
                  </div>
                  <div class="cr">
                    <div class="u-name">{{item.name}} · {{item.create_time}}</div>
                    <div class="comment-detail">{{item.content}}</div>
                  </div>
                </div>
                <div class="comment-end">评论已加载完毕～</div>
              </div>
              <!-- 评论列表结束 -->
            </div>
      </div>

      <div class="user col-3">
        <h6>发布人信息</h6>
        <div class="p-m">
          <div class="p-img">
            <img :src="content.user_avatar" alt />
          </div>
          <div>
            <div class="p-name">{{user.name}}</div>
            <div class="p-log">{{user.signature}}</div>
          </div>
        </div>
        <div class="p-b">
          <i class="iconfont icon-file-common"></i>发布了{{user.interest_num}}篇动态
        </div>
      </div>
    </div>
    <v-footer></v-footer>
    <v-backtop></v-backtop>
  </div>
</template>
<script>
import axios from "axios";
import vHead from "../components/head";
import vFooter from "../components/footer";
import vBacktop from "../components/backtop";
import vComment from "../components/comment";
export default {
  data() {
    return {
      id: this.$route.query.id,
      data: {},
      content: {},
      commentlist:{},
      touxiang: {},
      //评论内容
     login: true,
      user: {}
    };
  },
  components: {
    vHead,
    vFooter,
    vBacktop,
    vComment
  },
  created() {
    this.touxiang =JSON.parse(localStorage.getItem("tx"));
    axios.post("http://10.12.80.203/api/Interest/interest_details",{
      id:this.id
    }).then(res=>{
       this.content = res.data.interest_details;
        this.data=res.data
        this.user = res.data.user_data;
        
        this.commentlist = res.data.comment_list;
        console.log(this.commentData)
    }).catch(err=>{

    })

  },
  methods: {
    dianzan(){
      axios.post("http://10.12.80.203/api/Comment/add_like",{
        interest_id:this.id,
         send_user_id: localStorage.getItem("userid")
      }).then(res=>{
           this.$message({
                message: "点赞成功！",
                type: "success",
                offset: "80"
              });
        axios
                .post("http://10.12.80.203/api/Interest/interest_details", {
                  id: this.id
                })
                .then(res => {
                 this.data=res.data
                });
      }).catch(err=>{

      })
    },
   submit() {
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
        if (
          document.getElementById("exampleFormControlTextarea1").value != ""
        ) {
          // 提交评论
          //msg换成后台地址
          axios
            .post("http://10.12.80.203/api/Comment/add_Comment", {
              content: this.comment,
              //文章id
              interest_id: this.id,
              //用户id
              send_user_id: localStorage.getItem("userid")
            })
            .then(res => {
              axios
                .post("http://10.12.80.203/api/Interest/interest_details", {
                  id: this.id
                })
                .then(res => {
                  this.commentlist = res.data.comment_list;
                });
              this.comment = "";
              this.$message({
                message: "发布成功！",
                type: "success",
                offset: "80"
              });
            })
            .catch(e => {
              this.$message({
                message: "评论失败！",
                type: "warning",
                offset: "80"
              });
            });
        } else {
          this.$message({
            message: "评论不能为空",
            type: "warning",
            offset: "80"
          });
        }
      }
    }
  }
};
</script>
<style scoped lang="scss">

.addcomment {
  margin-top: 80px;
  color: #666666;
  h5 {
    font-size: 14px;
  }
}
.comment-b {
  display: flex;
}
.addcomment-left {
  width: 60px;
  height: 60px;
  margin-top: 20px;
  img {
    border-radius: 50%;
    width: 60px;
    min-width: 60px;
    height: 60px;
    border: 2px solid #ffffff;
  }
}
.addcomment-right {
  margin-top: 20px;
  margin-left: 20px;
}
.form-control {
  background: #f5f5f5;
  border-radius: 2px;
  padding: 20px;
  font-size: 12px;
  color: #999999;
  outline: none;
  border: none;
  resize: none;
}
.sub {
  float: right;
  background: linear-gradient(360deg, #7bc5e8 0%, #46b0ee 100%);
  width: 80px;
  border-radius: 100px;
  color: #fff;
  margin: 0;
  height: 30px;
  line-height: 30px;
  text-align: center;
  margin-top: 8px;
  border: 0px;
  cursor: pointer;
  &:hover {
    box-shadow: 0px 2px 2px 0px #46b0ee;
  }
}
.new-list {
  border-bottom: 1px dashed #e5e5e5;
}
/* 评论列表 */
.comment-list {
  color: #666666;
  margin-top: 50px;
}
.comment-main {
  display: flex;
  border-bottom: 1px dashed #e5e5e5;
  padding-bottom: 25px;
  padding-top: 10px;
}
.u-name {
  color: #999999;
  font-size: 12px;
  margin-top: 5px;
}
.cr {
      margin-left: 30px;
    margin-top: 10px;
}
.comment-detail {
  margin-top: 10px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
}
.tx {
  width: 40px;
  height: 40px;
  margin-top: 10px;
  img {
    border-radius: 50%;
    width: 40px;
    min-width: 40px;
    height: 40px;
    border: 2px solid #ffffff;
  }
}
.comment-end {
  width: 140px;
  margin: 40px auto;
  color: #999999;
  font-size: 12px;
}
.gray-bg-input {
  margin-top: 10px;
}
.new-comment {
  margin-bottom: 50px;
}
.btn {
  margin-top: 10px;
  float: right;
}
.pinglun {
  margin-top: 20px;
  background: #ffffff;
  padding: 26px 20px 20px;
  border-radius: 6px;
}
.a {
  display: flex;
}
.p-b {
  margin-left: 20px;
  margin-top: 30px;
  align-items: center;
  color: #7e94a2;
  i {
    margin-right: 5px;
  }
}
.p-m {
  display: flex;
  margin-top: 10px;
}
.p-img {
  img {
    border-radius: 50%;
    width: 70px;
    min-width: 70px;
    height: 70px;
    border: 2px solid #ffffff;
  }
}
.p-name {
  margin-top: 3px;
  margin-left: 5px;
  font-size: 23px;
  color: #0f2540;
  line-height: 33px;
}
.p-log {
  margin-left: 5px;
  color: #999999;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}
.user {
  margin-top: 20px;
  background: #ffffff;
  width: 285px;
  box-shadow: none;
  height: 200px;
  padding: 26px 20px 20px;
  border-radius: 6px;
}
.idle-top-wap {
  width: 100%;
  height: 140px;
  background: #ffffff;
  padding-top: 60px;
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
.right-w {
  display: flex;
}
span {
  i {
    margin-right: 14px;
  }
}
.right {
  font-size: 16px;
  color: #999999;
}
.tags-detail-top-img {
  margin-top: 60px;
  height: 290px;
  width: 100%;
  img {
    width: 100%;
    height: 290px;
    filter: blur(10px);
  }
}
.xx {
  display: flex;
}
.container {
  width: 590px;
}
.interset-content-top {
  display: flex;
  justify-content: space-between;
}
.tx {
  img {
    border-radius: 50%;
    width: 60px;
    min-width: 60px;
    height: 60px;
    border: 2px solid #ffffff;
  }
}
.interset-list-warp {
  width: 100%;
  border-radius: 6px;
  background: #ffffff;
  padding: 20px;
  box-sizing: border-box;
  display: block;
  margin-top: 20px;
  position: relative;
  cursor: pointer;
}
.imf {
  margin-top: 18px;
    margin-left: 25px;

}
.user_name {
  font-size: 14px;
  color: #435771;
  line-height: 22px;
  display: block;
}
.post_time {
  font-size: 12px;
  color: #c6c6c6;
  display: block;
}
.more {
  i {
    font-size: 30px;
    line-height: 30px;
    float: right;
    color: #435771;
  }
}
.interset-img {
  img {
    min-width: 330px;
    min-height: 330px;
    max-width: 330px;
    max-height: 330px;
  }
}
.interset-main {
  font-size: 20px;
  color: #0f2540;
  line-height: 28px;
  margin-bottom: 20px;
}
.interset-content {
  margin: 10px 10px 20px 65px;
  position: relative;
}
.interset-content-footer {
  display: flex;
  align-items: center;
  color: #c9d6de;
  padding-left: 65px;
  div {
    margin-right: 20px;
    display: flex;
  }
  i {
    margin-right: 8px;
    font-size: 16px;
    cursor: pointer;
  }
  span {
    margin-left: 8px;
  }
}
.tag-top {
  position: relative;
  width: 560px;
  top: -125px;
  left: 0;
  bottom: 0;
  right: 0;
  margin: 0 auto;
  z-index: 3;
  height: 50px;
}
.top-one {
  padding: 20px;
  border-bottom: 1px solid #e5e5e5;
  box-sizing: border-box;
  background: #ffffff;
  font-size: 20px;
  color: #0f2540;
  text-align: center;
}
.top-two {
  font-size: 14px;
  color: #bcbcbc;
  line-height: 24px;
  padding: 20px 20px 20px;
  background: #ffffff;
}
</style>