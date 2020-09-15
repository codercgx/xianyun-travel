<template>
  <div class="search-air">
    <div class="tab-list">
      <span
        class="tab-item"
        :class="{ active: currentIndex === index }"
        v-for="(item, index) in tabList"
        :key="index"
        @click="currentIndex = index"
        ><i :class="item.icon"></i>{{ item.name }}</span
      >
    </div>
    <div class="air-form">
      <el-form
        label-width="80px"
        :model="formData"
        ref="rulesForm"
        class="form-cotent"
      >
        <el-form-item label="出发城市" class="form-item">
          <el-autocomplete
            :fetch-suggestions="queryDepartSearch"
            placeholder="请搜索出发城市"
            v-model="formData.departCity"
            @select="handleDepartSelect"
            class="autocomplete"
          ></el-autocomplete>
        </el-form-item>
        <el-form-item label="到达城市" class="form-item"
          ><el-autocomplete
            :fetch-suggestions="queryDepartSearch"
            placeholder="请搜索到达城市"
            v-model="formData.destCity"
            @select="handleDepartSelect"
            class="autocomplete"
          ></el-autocomplete
        ></el-form-item>
        <el-form-item label="出发时间">
          <el-date-picker
            v-model="formData.departDate"
            align="center"
            type="date"
            placeholder="选择日期"
            :picker-options="pickerOptions"
          >
          </el-date-picker>
        </el-form-item>
        <el-form-item
          ><el-button
            style="width: 100%;"
            type="primary"
            size="medium"
            icon="el-icon-search"
            >搜索</el-button
          ></el-form-item
        >
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() < Date.now() - 24 * 3600 * 1000
        }
      },
      currentIndex: 0,
      tabList: [
        {
          icon: 'iconfont icondancheng',
          name: '单程'
        },
        {
          icon: 'iconfont iconshuangxiang',
          name: '往返'
        }
      ],
      formData: {
        departCity: '',
        departCode: '',
        destCity: '',
        destCode: '',
        departDate: ''
      }
    }
  },
  created() {},
  methods: {
    queryDepartSearch() {},
    handleDepartSelect() {}
  }
}
</script>

<style lang="less" scope>
.search-air {
  width: 360px;
  height: 350px;
  border: 1px solid #eee;
  .tab-list {
    background: #eee;
    width: 100%;
    height: 48px;
    line-height: 48px;
    text-align: center;
    .active {
      background-color: #fff;
      border-top: 3px solid orange;
    }
    .tab-item {
      width: 180px;
      height: 100%;
      cursor: pointer;
      display: inline-block;
      i {
        margin-right: 2px;
      }
    }
  }
  .air-form {
    width: 100%;
    padding: 15px 50px 15px 15px;
    position: relative;
    // background-color: yellow;
    box-sizing: border-box;

    .form-content {
      .form-item {
        width: 100%;
        .autocomplete {
          width: 100%;
          position: relative;

          // &:after {
          //   position: absolute;
          //   bottom: 0;
          //   right: 0;
          //   transform: translate(100%, 0);
          //   display: inline-block;
          //   content: '';
          //   width: 20px;
          //   height: 50%;
          //   border: 1px solid #999;
          // &::nth-child(2) {
          //   position: relative;
          //   left: 0;
          //   bottom: 50%;
          // }
        }
      }

      &::first-child {
        background-color: yellowgreen !important;
        &:after {
          position: absolute;
          bottom: 0;
          right: 0;
          transform: translate(100%, 0);
          display: inline-block;
          content: '';
          width: 20px;
          height: 50%;
          border: 1px solid #999;
        }
        &:last-child {
        }
      }
    }
  }
}
</style>
