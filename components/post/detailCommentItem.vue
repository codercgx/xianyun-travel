<template>
  <div class="comment-list">
    <div
      class="comment-item"
      v-for="(item, index) in dataMap"
      :key="`${item.id}${index}`"
    >
      <div class="comment-desc">
        <div class="comment-user-info">
          <img
            :src="
              `${$axios.defaults.baseURL}${item.account &&
                item.account.defaultAvatar}`
            "
            alt
          />
          <span>{{ item.account.nickname }}</span>
          <span>{{ item.created_at | dateForm }}</span>
        </div>
        <div class="comment-level">
          <span>{{ item.level }}</span>
        </div>
      </div>

      <div class="comment-content">
        <p class="comment-message">{{ item.content }}</p>
        <div class="comment-images">
          <div class="img-item" v-for="item1 in item.pics" :key="item1.id">
            <img :src="`${$axios.defaults.baseURL}${item1.url}`" alt="" />
          </div>
        </div>
        <div class="comment-res">
          <strong @click="replyUser(item.id, item.account.nickname)"
            >回复</strong
          >
        </div>
      </div>

      <comment-item v-if="item.parent" :data="item.parent"  @recUserId="recUserId"></comment-item>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  name: 'CommentItem',
  props: ['data'],
  filters: {
    dateForm(time) {
      return moment(new Date(time)).format('YYYY-MM-DD HH:MM')
    }
  },
  computed: {
    dataMap() {
      if (this.data.length) {
        return this.data
      } else {
        return [this.data]
      }
    }
  },
  methods: {
    replyUser(id, nickname) {
      this.$emit('recUserId', {
        id,
        nickname
      })
    },
    recUserId(data) {
      this.$emit('recUserId', data)
    }
  }
}
</script>

<style lang="less" scoped>
.comment-list {
  .comment-item {
    border: 1px solid #999;
    border-bottom: 1px dashed green;
    padding: 20px;
    margin-bottom: 20px;
    .comment-desc {
      display: flex;
      align-items: center;
      justify-content: space-between;
      .comment-user-info {
        display: flex;
        align-items: center;
        margin-bottom: 20px;
        img {
          width: 30px;
          height: 30px;
          margin-right: 20px;
        }
        span {
          font-size: 14px;
          color: #999;
          margin-right: 20px;
        }
      }
      .comment-level {
        span {
          color: orange;
          font-weight: 400;
        }
      }
    }
    .comment-content{
       margin-bottom: 20px;
      .comment-message{
          margin-bottom: 20px;
      }
      .comment-images{
        display: flex;
        align-items: center;
        justify-content: space-around;
        .img-item{
          width: 120px;
          height: 120px;
          img{
            width: 100%;
            height: 100%;
          }
        }
      }
      .comment-res{
        text-align: right;
        strong{
          color: blue;
          cursor: pointer;
        }
      }
    }
  }
}
</style>
