<template>
  <div class="PostList">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <div class="posts" v-else>
      <ul>
        <li>
          <div class="toobar">
            <span>全部</span>
            <span>精华</span>
            <span>分享</span>
            <span>问答</span>
            <span>招聘</span>
          </div>
        </li>
        <li v-for="post of posts">
          <img :src="post.author.avatar_url" alt="">
          <!-- 回复/浏览 -->
          <span class="allcount">
            <span class="reply_count">{{post.reply_count}}</span>
            /{{post.visit_count}}
          </span>
          <!-- 分类 -->
          <span :class="[{put_good:(post.good === true),put_top: (post.top === true),'topiclist-tab':(post.good !== true && post.top !== true)}]">
            <span>
              {{post | tabFormatter}}
            </span>
          </span>
          <!-- 标题 -->
          <router-link :to="{
            name: 'post_content',
            params: {
              id: post.id,
              name: post.author.loginname
            }
          }">
            <span>
              {{post.title}}
            </span>
          </router-link>
          <!-- 最终回复时间 -->
          <span class="last_reply">{{post.last_reply_at | formatDate}}</span>
        </li>
        <li>
          <!-- 分页 -->
          <Pagination @handleList="renderList"></Pagination>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import Pagination from './Pagination'
export default {
  name: 'PostList',
  components: {
    Pagination
  },
  data() {
    return {
      isLoading: false,
      posts: [],
      postPage: 1
    }
  },
  methods: {
    getData() {
      this.$http.get('https://cnodejs.org/api/v1/topics', {
        params: {
          limit: 20,
          page: this.postPage
        }
      }).then(res => {
        this.isLoading = false;
        this.posts = res.data.data
      }).catch(err => {
        console.log(err)
      })
    },

    renderList(value) {
      // alert(value);
      this.postPage = value;
      this.getData()
    }
  },
  beforeMount() {
    this.isLoading = true;
    this.getData();
  }
}
</script>

<style scoped>
.PostList {
  background: #e1e1e1;
}

.posts {
  margin-top: 10px;
}

.PostList img {
  width: 30px;
  height: 30px;
  vertical-align: middle;
}

ul {
  list-style: none;
  width: 100%;
  max-width: 1344px;
  margin: 0 auto;
}

ul li:not(:first-child) {
  padding: 9px;
  font-size: 15px;
  font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif!important;
  font-weight: 400;
  background: white;
  color: #333;
  border-top: 1px solid #f0f0f0;
}

li:not(:first-child):hover {
  background: #f5f5f5
}

li:last-child:hover {
  background: white;
}

li span {
  line-height: 30px;
}

.allcount {
  width: 70px;
  display: inline-block;
  text-align: center;
  font-size: 12px;
  color: #b4b4b4
}

.reply_count {
  color: #9e78c0;
  font-size: 14px;
}

.put_good,
.put_top {
  background: #80bd01;
  padding: 2px 4px;
  border-radius: 3px;
  color: #fff;
  font-size: 12px;
  margin-right: 10px;
}

.topiclist-tab {
  background: #e5e5e5;
  color: #999;
  padding: 2px 4px;
  border-radius: 3px;
  font-size: 12px;
  margin-right: 10px;
}

.last_reply {
  text-align: right;
  min-width: 50px;
  display: inline-block;
  white-space: nowrap;
  float: right;
  color: #778087;
  font-size: 12px;
}

.toobar {
  height: 40px;
  background: #f6f6f6;
}

.toobar span {
  font-size: 14px;
  line-height: 40px;
  color: #80bd01;
  margin: 0 10px;
  cursor: pointer;
}

.toobar span:hover {
  color: #9e78c0;
}

a {
  text-decoration: none;
  color: black;
}

a:hover {
  text-decoration: underline;
}

.loading {
  text-align: center;
  padding-top: 300px;
}

</style>