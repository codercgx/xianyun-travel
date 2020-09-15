<template>
  <div class="post-index">
    <el-row>
      <el-col :span="7">
        <div class="post-aside">
          <TourAside @gainCityName="gainCityName" />
        </div>
      </el-col>
      <el-col :span="17" class="post-main">
        <div class="post-content">
          <div class="post-search-input">
            <el-input
              placeholder="请输入想去的地方，比如 '广州'"
              v-model="input3"
              class="input-with-select"
              @change="searchArticleList"
            >
              <i class="el-icon-search el-input__icon" slot="suffix"></i
            ></el-input>
          </div>

          <div class="post-recommend">
            推荐：&nbsp;
            <span
              v-for="(item, index) in recommendList"
              :key="index"
              @click="addCityName(item)"
              >{{ item }}</span
            >
          </div>

          <div class="post-write-btn">
            <!-- <el-row
              type="flex"
              align="middle"
              justify="space-between"
              class="write-notes"
            >
              <el-col :span="3"><span>推荐攻略</span></el-col>
              <el-col :span="3">
                <el-button type="primary" icon="el-icon-edit">写游记</el-button>
              </el-col>
            </el-row> -->
            <div class="write-notes">
              <span>推荐攻略</span>
              <el-button type="primary" icon="el-icon-edit" @click="toWriteArticle">写游记</el-button>
            </div>
          </div>

          <div class="article-detail-item">
            <ArticleDetails
              v-for="(item, index) in articleList"
              :key="item.id + index"
              :cardContent="item"
            >
            </ArticleDetails>
          </div>

          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="start / limit + 1"
            :page-sizes="[3, 5, 10, 15]"
            :page-size="limit"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
          ></el-pagination>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import TourAside from '@/components/post/tourAside'
import ArticleDetails from '@/components/post/articleDetails'
export default {
  components: {
    TourAside,
    ArticleDetails
  },
  data() {
    return {
      input3: '',
      recommendList: ['广州', '上海', '北京'],
      articleList: [],
      start: 0,
      limit: 5,
      total: 0
    }
  },
  methods: {
    toWriteArticle() {
      this.$router.push({
        path: '/post/create'
      })
    },
    searchArticleList() {
      if (this.input3) {
        params.city = this.input3
      }
      this.$axios({
        url: '/posts',
        params: {
          _start: this.start, // 从哪一条数据开始
          _limit: this.limit // 拿多少条数据
        }
      }).then(res => {
        const { data, total } = res.data
        this.articleList = data
        this.total = total
      })
    },
    handleSizeChange(data) {
      this.pagesizes = data
      this.searchArticleList()
    },
    handleCurrentChange(index) {
      this.start = (index - 1) * this.limit
      this.$router.replace({
        url: this.$route.path,
        query: {
          start: this.st
        }
      })
    },

    gainCityName(data) {
      this.input3 = data
      this.currentIndex = 1
      this.searchArticleList()
    },
    searchArticleListInput() {
      this.currentIndex = 1
      this.searchArticleList()
    }
  },
  mounted() {
    this.searchArticleList()
  },
  beforeRouteUpdate(to, form, next) {
    next()
    this.currentIndex = 1
    this.input3 = ''
    this.searchArticleList()
    console.log(to)
  }
}
</script>

<style lang="less" scoped>
.post-index {
  width: 1000px;
  margin: 30px auto;
  .post-main {
    .post-content {
      .post-search-input {
        /deep/.el-input__inner {
          height: 40px;
          line-height: 40px;
          border: 3px solid orange;
          // text-align: center;
          padding: 0 20px;
          i {
            color: orange;
            font-size: 24px;
          }
        }
      }
      .post-recommend {
        margin-top: 20px;
        span {
          font-size: 14px;
          margin-right: 15px;
          &:hover {
            cursor: pointer;
            color: orange;
            text-decoration: underline;
          }
        }
      }
      .post-write-btn {
        width: 100%;
        .write-notes {
          margin-top: 20px;
          border-bottom: 1px solid #ddd;
          height: 60px;
          line-height: 60px;
          box-sizing: border-box;
          display: flex;
          align-items: center;
          justify-content: space-between;
          span {
            display: block;
            width: 80px;
            height: 100%;
            font-size: 20px;
            font-weight: 500;
            color: orange;
            border-bottom: 2px solid orange;
            //  padding-bottom: 10px;
          }
        }
      }
    }
  }
}
</style>
