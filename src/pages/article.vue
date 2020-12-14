<template>
  <div>
    <v-head></v-head>
    <v-add></v-add>
    <!-- 文章推荐 -->
    <div class="container">
      <!-- <b-card-group deck>
        <b-card
          :key="index"
          v-for="(item,index) in tjarticle.slice(3 , 5)"
          @click="toDetail(item.id)"
          :title="item.title"
          :img-src="item.imgurl"
          img-alt="Image"
          img-top
          class="t-card"
          img-height="300px"
          img-width="150px"
        >
          <b-card-text>{{item.xiangqi}}</b-card-text>
          <b-card-text class="small text-muted">{{item.time}}</b-card-text>
          <b-badge pill class="badge">{{item.tag}}</b-badge>
        </b-card>
      </b-card-group> -->
      <!-- 分类导航栏 -->
      <b-tabs class="fenlei-tabs" card :fill="true" :pills="true">
        <b-tab title="全部" class="all" @click="getAll">
          <el-row class="row">
            <el-col :span="12" v-for="(item,index) in categorywz" :key="index" class="card">
              <div class="h">
                <el-card shadow="hover" @click.native.stop="toDetail(item.id)">
                  <img :src="item.background_img" class="image" />
                  <div style="padding: 14px;">
                    <span>{{item.title}}</span>
                    <div class="bottom clearfix">
                      <time class="time">{{item.time}}</time>
                    </div>
                  </div>
                </el-card>
              </div>
            </el-col>
            <b-button
              pill
              variant="outline-secondary"
              class="load-more"
              v-if="page<page_count"
              @click="loadMore"
            >加载更多...</b-button>
            <b-button pill variant="outline-secondary" class="load-more" v-else>没有更多了</b-button>
          </el-row>
        </b-tab>
        <b-tab
          @click="getcategory(item.id)"
          class="fenlei-tab"
          v-for="(item,index) in category"
          :key="index"
          :title="item.name"
        >
          <v-category
            :id="item.id"
            :categorywz="categorywz"
            v-loading="loading"
            element-loading-text="拼命加载中"
            element-loading-spinner="el-icon-loading"
            element-loading-background="rgba(245,245,245 0.8)"
          ></v-category>
        </b-tab>
      </b-tabs>
    </div>
    <v-footer></v-footer>
    <v-backtop></v-backtop>
  </div>
</template>
<script>
import axios from "axios";
import vHead from "../components/head";
import vAdd from "../components/add_top";
import vFooter from "../components/footer";
import vBacktop from "../components/backtop";
import vCategory from "../components/wz_category";
export default {
  data() {
    return {
      //推荐文章
      tjarticle: {},
      //分类导航栏数据
      category: {},
      categoryid: {},
      //分类下的文章
      categorywz: {},
      loading: true,
      page: 1,
      page_count:333
    };
  },
  components: {
    vHead,
    vAdd,
    vFooter,
    vBacktop,
    vCategory
  },
  created() {
    this.loading = true;
    // axios
    //   .get("ms", {
    //     data: {
    //       page: 1,
    //       flid: 1
    //     }
    //   })
    //   .then(res => {
    //     this.loading = false;
    //     this.tjarticle = res.data.articles;
    //     // this.category = res.data.fenlei;
    //     // this.categorywz = res.data.flwz;
    //     this.categoryid = this.category[0].id;
    //   });
    axios
      .post("http://10.12.80.203/api/article/get_article_list", {})
      .then(res => {
        this.category = res.data.column_list;
        this.categorywz = res.data.article_list;
        this.page_count=res.data.total_page
        console.log(this.categorywz);
      });
  },
  methods: {
    //下一页
    loadMore(){
       this.page += 1;
       axios
        .post("http://10.12.80.203/api/article/get_article_list", {
          
            page: this.page
          
        })
        .then(res => {
          this.categorywz= this.categorywz.concat(res.data.article_list);
          console.log(res.data)
        })
        .catch(e => {});
    },
    getcategory(categoryid) {
      this.loading = true;
      this.categoryid = categoryid;
      console.log(this.categoryid);
      axios
        .post("http://10.12.80.203/api/article/get_article_list", {
          column_id: this.categoryid
        })
        .then(res => {
          this.loading = false;
          this.categorywz = res.data.article_list;
        })
        .catch(e => {});
    },
    toDetail(id) {
      this.$router.push({
        path: "article_detail",
        query: { id: id }
      });
    },
    getAll() {
      axios
        .post("http://10.12.80.203/api/article/get_article_list", {})
        .then(res => {
          this.loading = false;
          this.categorywz = res.data.article_list;
        })
        .catch(e => {});
    },
    //分类导航栏
    tofenlie() {}
  }
};
</script>

<style lang="scss">
.container{

}
.all {
  .card {
    margin-bottom: 30px;
    width: 200px;
    margin-left: 20px;
  }
  .row {
    display: flex;
    position: relative;
  }
  .time {
    font-size: 13px;
    color: #999;
  }
  .bottom {
    margin-top: 13px;
    line-height: 12px;
  }
  .button {
    padding: 0;
    float: right;
  }
  .image {
    width: 100%;
    height: 150px;
    display: block;
  }
  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix {
    &:after {
      clear: both;
    }
  }
  span {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 1;
  }
  .h {
    &:hover {
      cursor: pointer;
      box-shadow: 0 0 20px 1px #cccaca;
      transition: all 0.3s ease 0s;
    }
    &.el-card__body {
      padding: 5px;
    }
  }
  .load-more {
    position: absolute;
    margin-left: 50%;
    bottom: -50px;
  }
}
.t-card {
  margin-top: 50px;
}
.fenlei-tabs {
  margin-top: 60px;
  .nav-link {
    background-color: #ecf0f2;
    color: #999999;
    margin-right: 5px;
    margin-left: 5px;
    margin-bottom: 10px;
    font-size: 20px;
  }
}
.card-header {
  border: 0;
  background-color: #ffffff !important;
}
.fenlei-tabs .nav-pills .nav-link.active,
.nav-pills .show > .nav-link {
  color: black;
  background-color: #ecf0f2;
}
</style>