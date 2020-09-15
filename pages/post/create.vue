<template>
  <div class="post-create">
    <el-row type="flex" justify="space-between">
      <el-col :span="18" class="main-content">
        <h3>发表新攻略</h3>
        <p>分享你的个人游记，让更多人看到哦！</p>
        <el-input
          placeholder="请输入标题"
          clearable
          v-model="form.title"
        ></el-input>
        <div>
          <Editor @quillEdit="quillEdit" :cleardata="clearEditor" />
        </div>
        选择城市：<el-autocomplete
          v-model="cityname"
          label="选择城市"
          placeholder="请搜索游玩城市"
          :fetch-suggestions="querySearchAsync"
          @select="handleSelect"
        ></el-autocomplete>
        <div class="post-or-save">
          <el-button icon="el-icon-position" type="primary" size="mini"
            >发布</el-button
          ><span>或者</span>
          <i class="el-icon-document" @click="cacheContent">保存到草稿</i>
        </div>
      </el-col>
      <el-col :span="6" class="post-save">
        <CreateAside />
      </el-col>
    </el-row>
  </div>
</template>

<script>
import Editor from '@/components/post/createContent'
import moment from 'moment'
import CreateAside from '@/components/post/createAside'
export default {
  components: {
    Editor,
    CreateAside
  },
  data() {
    return {
      form: {
        title: '',
        content: '',
        city: ''
      },
      cityname: '',
      clearEditor: false
    }
  },
  computed: {
    storeDataChange() {
      if (!this.$store.state.post.listenData) return ''
      if (this.$store.state.post.listenData.isListen) {
        const index = this.$store.state.post.listenData.index
        const { cityname, date, ...form } = this.$store.state.post.formPublish[
          index
        ]
        this.form = form
        this.cityname = cityname
        setTimeout(() => {
          this.$store.commit('post/addListenData', {
            isListen: false
          })
        }, 1000)
      }
      return ''
    }
  },
  methods: {
    articleSubmit() {
      if (!this.validData()) {
        return
      }
      this.$axios({
        method: 'post',
        url: '/posts',
        data: this.form,
        headers: {
          Authorization: 'Bearer ' + this.$store.state.user.userInfo.token
        }
      }).then(res => {
        this.$message.success(res.data.message)
        this.form = {}
        this.clearEditor = !this.clearEditor
        this.cityname = ''
      })
    },
    quillEdit(data) {
      this.form.content = data
    },
    handleSelect(data) {
      this.cityname = data.value
      this.form.city = data.id
    },
    async querySearchAsync(query, callback) {
      let arr = await this.querySearchAsyncCity(this.cityname)
      if (arr.length > 0) {
        this.cityname = arr[0].value
        this.form.city = arr[0].id
      }
      callback(arr)
    },
    querySearchAsyncCity(keywords) {
      return new Promise((resolve, reject) => {
        if (!keywords) {
          resolve([])
        }
        this.$store
          .dispatch('air/getCityName', { name: keywords })
          .then(({ data }) => {
            data = data.data.map(v => {
              v.value = v.name
              return v
            })
            resolve(data)
          })
      })
    },
    validData() {
      const rules = {
        title: {
          errMessage: '标题内容不能为空',
          validator: () => {
            return !!this.form.title
          }
        },
        content: {
          errMessage: '文章内容不能为空',
          validator: () => {
            return !!this.form.content
          }
        },
        city: {
          errMessage: '请输入城市',
          validator: () => {
            return !!this.form.city
          }
        }
      }
      let valid = true
      Object.keys(rules).forEach(v => {
        if (!valid) return
        valid = rules[v].validator()
        if (!valid) {
          this.$message.error(rules[v].errMessage)
        }
      })
      return valid
    },
    cacheContent() {
      if (!this.validData()) {
        return
      }
      let formPublish = this.$store.state.post.formPublish
      let date = moment(Date.now()).format('YYYY-MM-DD')
      let cityname = this.cityname
      const newForm = { ...this.form, date, cityname }
      formPublish = [newForm, ...formPublish]
      // 对象去重
      let map = new Map()
      formPublish.forEach(v => {
        if (!map.has(`${v.title + v.content + v.city}`)) {
          map.set(`${v.title + v.content + v.city}`, v)
        }
      })
      formPublish = [...map.values()]
      this.$store.commit('post/addFromPublish', formPublish)
      // 数据清空
      this.form = {}
      this.clearEditor = !this.clearEditor
      this.cityname = ''
    }
  }
}
</script>

<style lang="less" scoped>
.post-create {
  width: 1000px;
  min-width: 1000px;
  margin: 20px auto;

  .main-content {
    // width: 100%;
    h3 {
      margin-bottom: 10px;
    }
    p {
      color: #999;
      font-size: 14px;
      margin-bottom: 10px;
    }
    /deep/.el-input__inner {
      margin-bottom: 10px;
    }
    .post-or-save {
      span {
        margin: 0 10px;
      }
      i {
        color: orange;
        font-size: 15px;
        cursor: pointer;
      }
    }
  }
}
</style>
