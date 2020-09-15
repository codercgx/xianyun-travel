<template>
  <div class="index">
    <div class="backgroundImg">
      <el-carousel width="1000px" height="700px">
        <el-carousel-item v-for="(item, index) in banners" :key="index">
          <div
            :style="{
              background: `url(${$axios.defaults.baseURL}${item.url}) no-repeat center`
            }"
            class="carousel-box"
          ></div>
        </el-carousel-item>
      </el-carousel>
    </div>

    <div class="search-tab">
      <el-row class="tabList" type="flex" align="middle" justify="center">
        <span
          v-for="(item, index) in tabList"
          :key="index"
          @click="tabClick(index)"
          :class="{ active: index == currIndex }"
        >
          <i>{{ item.content }}</i>
        </span>
      </el-row>

      <el-row type="flex" align="middle" class="search-input" justify="start">
        <el-input v-model="searchInfo" :placeholder="tabList[currIndex].placeholder" @keydown.down="searchContent"></el-input>
        <i class="el-icon-search" @click="searchContent"></i>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      banners: [],
      currIndex: 0,
      searchInfo:'',
      tabList: [
        {
          content: '攻略',
          placeholder: '搜索城市'
        },
        {
          content: '酒店',
          placeholder: '请输入城市搜索酒店'
        },
        {
          content: '机票',
          placeholder: ''
        }
      ]
    }
  },
  created() {
    this.getBanners()
  },
  methods: {
    async getBanners() {
      const { data } = await this.$axios.$get('scenics/banners')
      // console.log(data)
      this.banners = data
    },
    tabClick(index) {
      if (index === 2) {
        this.$router.push('/air')
      }
      this.currIndex = index
    },
    searchContent(){
       this.$router.push('/hotel')
    }
  }
}
</script>

<style lang="less" scope>
.index {
  position: relative;
  .backgroundImg {
    .carousel-box {
      height: 700px;
      min-width: 1000px;
    }
  }
  .search-tab {
    position: absolute;
    z-index: 9;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    border-top: 1px transparent solid;
    .tabList {
      span {
        cursor: pointer;
        width: 82px;
        height: 36px;
        display: block;
        position: relative;
        margin-right: 8px;
        i {
          position: absolute;
          z-index: 2;
          display: block;
          width: 100%;
          height: 100%;
          line-height: 30px;
          text-align: center;
          // color: #fff;
          &:after {
            position: absolute;
            left: 0;
            top: 0;
            display: block;
            content: '';
            width: 100%;
            height: 100%;
            border: 1px rgba(255, 255, 255, 0.2) solid;
            border-bottom: none;
            transform: scale(1.1, 1.3) perspective(0.7em) rotateX(2.2deg);
            transform-origin: bottom left;
            background: rgba(255, 255, 255, 0.5);
            border-radius: 1px 2px 0 0;
            box-sizing: border-box;
          }
        }
      }

      .active {
        i {
          color: white !important;
        }
        &::after {
          background: #eee;
        }
      }
    }
    .search-input {
      margin: 10px 0;
      width: 400px;
      height: 46px;
      background-color: #fff;
      border-radius: 0 4px 4px 4px;
      border: 1px rgba(255, 255, 255, 0.2) solid;
      border-top: none;
      box-sizing: unset;
      input {
        // border: none;
        border: 0;
        height: 20px;
        padding: 13px 15px;
        outline: none;
        font-size: 16px;
      }
      el-icon-serch {
        font-size: 22px;
        cursor: pointer;
        padding: 0 10px;
        font-weight: bold;
      }
    }
  }
}
</style>
