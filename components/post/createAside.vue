<template>
  <div class="post-draft">
    <h4>
      草稿箱 / <span>数量 ~ {{ form.length }}</span>
    </h4>
    <div class="draft-item" v-for="(item, index) in form" :key="index">
      <div class="draft-title">
       <span>标题</span> <span>{{ item.title }}</span> <i class="el-icon-edit">修改</i>
      </div>
      <span>发布时间</span>  <span>{{ item.date }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {}
  },
  computed: {
    form() {
      return this.$store.state.post.formPublish
    }
  },
  methods: {
    sendDataToStore(index) {
      this.$store.commit('post/addListenData', {
        isListen: true,
        index
      })
    }
  }
}
</script>

<style lang="less" scoped>
.post-draft {
  padding: 20px 0 20px 20px;
  border: 1px dashed green;
  h4 {
    margin-bottom: 20px;
    color: #666;
    span {
      color: orange;
      font-weight: 400;
      margin-left: 10px;
    }
  }
  .draft-item {
    margin-top: 20px;
    border-bottom: 1px solid #999;
    padding-bottom: 10px;
    .draft-title {
      margin-bottom: 10px;
      span {
        color: #666;
        font-size: 16px;
        margin-right: 10px;
      }
      i {
        color: blue;
        font-size: 15px;
      }
    }
    span {
      font-size: 14px;
      color: #999;
    }
  }
}
</style>
