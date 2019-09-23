<template>
  <div class="slidebar">
    <div class="authorsummay">
      <div class="topbar">作者</div>
      <router-link :to="{
        name: 'user_info',
        params: {
          name: userinfo.loginname
        }
      }">
        <img :src="userinfo.avatar_url" alt="">
      </router-link>
    </div>
    <div class="recent_topics">
      <div class="topbar">作者最近主题</div>
      <ul>
        <li v-for="list in topiclimitby5">
          <router-link :to="{
            name: 'post_content',
            params: {
              id: list.id,
              name: list.author.loginname
            }
          }">
            {{list.title}}
          </router-link>
        </li>
      </ul>
    </div>
    <div class="recent_replies">
      <div class="topbar">作者最近回复</div>
      <ul>
        <li v-for="list in replylimitby5">
          <router-link :to="{
            name: 'post_content',
            params: {
              id: list.id,
              name: list.author.loginname
            }
          }">
            {{list.title}}
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'SlideBar',
    data() {
      return {
        isLoading: false,
        userinfo: {}
      }
    },
    methods: {
      getData() {
        this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
          .then(res => {
            this.isLoading = false;
            this.userinfo = res.data.data
          })
          .catch(err => {
            console.log(err)
          })
      },
    },
    computed: {
      topiclimitby5() {
        if(this.userinfo.recent_topics) {
          return this.userinfo.recent_topics.slice(0, 5);
        }
      },
      replylimitby5() {
        if(this.userinfo.recent_replies) {
          return this.userinfo.recent_replies.slice(0, 5);
        }
      }
    },
    beforeMount() {
      this.isLoading = true;
      this.getData();
    },
    watch: {
      '$route'(to, from) {
        this.getData();
      }
    }
  }
</script>

<style scoped>
.slidebar {
  width: 328px;
  float: right;
  margin-top: 0;
}

.authorsummay,
.recent_topics,
.recent_replies {
  background: #fff;
}

li {
  padding: 3px 0;
}

.recent_replies ul,
.recent_topics ul {
  margin-top: 0;
  margin-bottom: 0;
  list-style: none;
  padding-left: 14px;
}

ul a {
  font-size: 12px;
  text-decoration: none;
  color: #778087;
}

.topbar {
  height: 16px;
  margin-top: 10px;
  padding: 10px;
  font-size: 12px;
  background: #f6f6f6;
}

img {
  width: 48px;
  height: 48px;
  border-radius: 3px;
  margin: 10px;
}

/* .loginname {
  width: 100px;
  float: right;
  margin-top: 22px;
  margin-right: 159px;
  font-size: 14px;
} */

</style>