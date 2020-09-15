<template>
  <div class="article-detail-item">
    <div class="article-layout-one" v-if="cardContent.images.length >= 3">
      <h3 @click="toPostDetail">{{ cardContent.title }}</h3>
      <p
        class="article-desc"
        v-html="cardContent.summary"
        @click="toPostDetail"
      ></p>
      <el-row
        type="flex"
        align="middle"
        justify="space-between"
        class="imgs-wrap-row"
      >
        <el-col
          :span="24 / cardContent.images.length"
          v-for="(item, index) in images"
          :key="index"
        >
          <img :src="item || defaultImg" alt />
        </el-col>
      </el-row>

      <el-row
        class="article-info"
        type="flex"
        align="middle"
        justify="space-between"
      >
        <el-row
          type="flex"
          align="middle"
          justify="space-between"
          class="left-info"
        >
          <span>
            <i class="el-icon-location-outline"></i>
            {{ cardContent.city.name }}
          </span>
          <nuxt-link to="/user/personal">
            <img
              :src="
                `${$axios.defaults.baseURL}${cardContent.account.defaultAvatar}`
              "
            />
            {{ cardContent.account.username }}
          </nuxt-link>
          <span>
            <i class="el-icon-view"></i>
            {{ cardContent.likeIds[0] }}
          </span>
        </el-row>
        <div class="right-info">78赞</div>
      </el-row>
    </div>

    <div class="article-layout-two" v-else>
      <el-row type="flex" align="middle" justify="space-between">
        <div><img :src="images || defaultImg" /></div>

        <div class="article-right-desc">
          <h3 @click="toPostDetail">{{ cardContent.title }}</h3>
          <p
            v-html="cardContent.summary"
            @click="toPostDetail"
          ></p>
          <el-row type="flex" align="middle" justify="space-between">
            <div class="left-info">
              <span>
                <i class="el-icon-location-outline"></i>
                北京市
              </span>
              <nuxt-link to="/user/personal">
                <img
                  src="http://157.122.54.189:9095/assets/images/avatar.jpg"
                />
                地球发动机
              </nuxt-link>
              <span>
                <i class="el-icon-view"></i>
                13667
              </span>
            </div>
            <div class="right-info">78赞</div>
          </el-row>
        </div>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cardContent: {
      type: Object,
      default() {
        return {
          images: [],
          account: {},
          city: {},
          likeIds: []
        }
      }
    }
  },
  data() {
    return {
      defaultImg:
        'http://b1-q.mafengwo.net/s12/M00/F3/48/wKgED1uwnOuAU_FaAALtym3as2g88.jpeg?imageMogr2%2Fthumbnail%2F%21220x130r%2Fgravity%2FCenter%2Fcrop%2F%21220x130%2Fquality%2F100'
    }
  },
  mounted() {},
  computed: {
    images() {
      if (this.cardContent.images < 2) {
        return this.cardContent.images[0]
      }
      return this.cardContent.images.slice(0, 3)
    }
  },
  methods: {
    toUserPersonal() {
      this.$router.push({
        path: '/user/personal'
      })
    },
    toPostDetail() {
      this.$router.push({
        path: '/post/detail',
        query: {
          id: this.cardContent.id
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
.article-detail-item {
  width: 100%;
  padding: 20px 0;
  overflow: hidden;
  .article-layout-one {
    border-bottom: 1px solid #ddd;
    padding: 10px 10px;
    h3 {
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      cursor: pointer;
      &:hover {
        color: orange;
      }
    }

    .article-desc {
      width: 100%;
      font-size: 14px;
      margin-top: 20px;
      height: 60px;
      overflow: hidden;
      color: #666;
      cursor: pointer;
      display: -webkit-box;
      -webkit-line-clamp: 3;
      -webkit-box-orient: vertical;
      &:hover {
        color: green;
      }
    }

    .imgs-wrap-row {
      width: 100%;
      margin-top: 20px;
      img {
        width: 220px;
        height: 150px;
      }
    }

    .article-info {
      height: 40px;
      line-height: 40px;
    }
  }
  .article-layout-two {
    border-bottom: 1px solid #ddd;
    width: 100%;
    // padding: 10px 10px;
    div{
      margin-right: 20px;
       > img {
      width: 230px;
      height: 150px;
    }
    }
    .article-right-desc {
      // padding: 10px 20px;
      h3 {
        vertical-align: top;
        overflow: hidden;
        // text-overflow: ellipsis;
        // white-space: nowrap;
        display: -webkit-box;
        -webkit-line-clamp: 1;
        -webkit-box-orient: vertical;
        margin-bottom: 15px;
        cursor: pointer;
        &:hover {
          color: orange;
        }
      }
      p{
        display: -webkit-box;
        -webkit-line-clamp: 3;
        -webkit-box-orient: vertical;
        overflow: hidden;
        color: #999;
        font-size: 14px;
        margin-bottom: 20px;
      }
    }
  }
}
// 公共样式
.left-info {
  // text-align: center;
  span {
    font-size: 14px;
    color: #999;
  }
  a {
    // vertical-align: middle;
    margin: 0 20px;
    color: orange;
    font-size: 14px;
    img {
      width: 24px;
      height: 24px;
      border-radius: 50%;
      vertical-align: middle;
    }
  }
}
.right-info {
  color: orange;
  font-size: 18px;
}
</style>
