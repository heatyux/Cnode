<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="judge" class="pagebtn">.....</button>
    <button v-for="btn in pagebtns" 
    @click="changeBtn(btn)" 
    :class="[{'currentPage': btn === currentPage}, 'pagebtn']" v-else>
      {{btn}}
    </button>
    <button>下一页</button>
  </div>
</template>

<script>
import $ from 'jquery'
  export default {
    name: 'Pagination',
    data() {
      return {
        pagebtns: [1,2,3,4,5,'.....'],
        currentPage: 1,
        judge: false
      }
    },
    methods: {
      changeBtn(page) {
        if(typeof page !== 'number') {
          switch(page.target.innerText) {
            case '上一页': 
              $('button.currentPage').prev().click();
              break;
            case '下一页':
              $('button.currentPage').next().click();
              break;
            case '首页':
              this.pagebtns = [1,2,3,4,5,'.....'];
              this.changeBtn();
              break;
            default:
              break;
          }
          return;
        }
        this.currentPage = page;
        if(page > 4) {
          this.judge = true;
        }else {
          this.judge = false;
        }
        if(page === this.pagebtns[4]) {
          this.pagebtns.shift();//移除第一个元素
          this.pagebtns.splice(4, 0, this.pagebtns[3] + 1); //添加后一个元素
        } else if(page === this.pagebtns[0] && page !== 1) {
          this.pagebtns.unshift(this.pagebtns[0] - 1); //添加第一个元素
          this.pagebtns.slice(5, 1);
        }

        this.$emit('handleList', this.currentPage)
      }
    }
  }
</script>

<style scoped>
.currentPage {

}

.pagebtn {

}
</style>