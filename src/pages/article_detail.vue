<template>
  <div>
    <div>
      <v-head></v-head>
      <v-add></v-add>
      <div class="container">
        <div class="row">
          <div class="col-8 content">
            <b-breadcrumb class="crumb" :items="items"></b-breadcrumb>
            <div>
              <div>
                <div v-html="content.content" id="mcontent" class="mcontent"></div>
              </div>
            </div>

            <!-- <div class="like">
            <i class="iconfont icon-fabulous"></i>
            </div>-->

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
            <!-- 添加评论结束 -->
          </div>

          <!-- 右侧发布人信息开始 -->
          <div class="col-4">
            <div class="sticky-top">
              <div class="author-xx">发布人信息</div>
              <div class="author">
                <div class="author-tx">
                  <img :src="postuser.avatar" alt />
                </div>
                <div class="author-r">
                  <div class="author-id">{{postuser.nickname}}</div>
                  <div class="author-word">{{postuser.signature}}</div>
                </div>
              </div>
              <!-- <div class="author-detail">
                <span>{{postuser.dongtai}}</span>动态
                <span>{{postuser.wznumber}}</span>文章
                <span>{{postuser.zongce}}</span>众测体验
              </div>-->

              <!-- 相关文章 -->
              <div class="about-wz">相关文章</div>
              <div>
                <b-card-group deck>
                  <b-card
                    img-alt="Image"
                    img-top
                    tag="article"
                    style="max-width: 20rem;"
                    class="mb-2"
                    v-for="(item,index) in data"
                    :key="index"
                    :img-src="item.background_img"
                    img-height="100px"
                    img-width="100px"
                    @click="toDetail(item.id)"
                  >
                    <b-card-text class="about-title">{{item.title}}</b-card-text>
                  </b-card>
                </b-card-group>
              </div>
            </div>
          </div>

          <!-- 发布人信息结束 -->
        </div>
      </div>
      <v-footer></v-footer>
    </div>
    <v-backtop></v-backtop>
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
      //评论
      comment: null,
      data: {},
      id: this.$route.query.id,
      content: {},
      login: true,
      commentlist: {},
      postuser: {},
      touxiang: {},
      // 面包屑内容
      items: [
        {
          text: "首页",
          to: { name: "index" }
        },
        {
          text: "文章",
          to: { name: "article" }
        },
        {
          text: "文章详情",
          active: true
        }
      ]
      //面包屑结束
    };
  },
  components: {
    vHead,
    vAdd,
    vFooter,
    vBacktop
  },
  mounted() {},
  methods: {
    toDetail(id) {
      let routeData = this.$router.resolve({
        path: "article_detail",
        query: {
          id: id
        }
      });
      window.open(routeData.href, "_blank");
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
              article_id: this.id,
              //用户id
              send_user_id: localStorage.getItem("userid")
            })
            .then(res => {
              axios
                .post("http://10.12.80.203/api/article/article_details", {
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
  },
  created() {
    this.touxiang =JSON.parse(localStorage.getItem("tx"));
    axios
      .post("http://10.12.80.203/api/article/article_details", {
        id: this.id
      })
      .then(res => {
        this.data = res.data.article_list;
        this.content = res.data.article_details;
        this.postuser = res.data.user_data;
        //console.log(this.data)
        this.commentlist = res.data.comment_list;
        console.log(111);
        console.log(res.data);
        // for (var i = 0; i < this.data.length; i++) {
        //   if (this.data[i].id == this.id) {
        //     this.content = this.data[i];
        //   }
        // }
      });
  }
};
</script>
<style scoped lang="scss">
.mcontent /deep/ img {
  max-width: 100%;
}
* {
  background-color: #fafafa;
}
.container {
  margin-top: 50px;
  margin-bottom: 20px;
}
/* 面包屑样式开始 */
.crumb {
  background-color: #fafafa;
  padding: 0px;
  a {
    color: black;
  }
}
/* 面包屑样式结束 */
/* .like{
      text-align: center;
      width: 60px;
    height: 60px;
    cursor: pointer;
    margin: 0 auto 10px;
}
.like i{
    cursor: pointer;
    font-size: 54px;
    color: #fff;
} */
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
  margin-left: 10px;
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
.sticky-top {
  top: 60px;
  margin-bottom: 500px;
}
.author {
  display: flex;
  margin-top: 10px;
  img {
    border-radius: 50%;
    width: 70px;
    min-width: 70px;
    height: 70px;
    border: 2px solid #ffffff;
  }
}
.author-r {
  margin-left: 10px;
}
.author-id {
  margin-top: 5px;
  font-size: 16px;
  color: #333333;
  line-height: 14px;
  margin-bottom: 10px;
}
.author-word {
  font-size: 14px;
  color: #999999;
  margin-top: 5px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
}
.author-xx {
  color: #999999;
}
.author-detail {
  margin-top: 30px;
  display: flex;
  color: #999999;
  span {
    color: black;
    margin-left: 20px;
  }
}
.about-title {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 1;
}
.mb-2 {
  margin-top: 30px;
}
.about-wz {
  margin-top: 50px;
  color: #999999;
}
</style>