<template>
  <div class="order-content">
    <el-row class="main-content" type="flex" justify="space-between">
      <div class="order-form">
        <order-form :data="infoData" @sendAllPrice="recPrice"></order-form>
      </div>
      <div class="order-info">
        <order-aside :data="infoData" :allPrice="allPrice"></order-aside>
      </div>
    </el-row>
  </div>
</template>

<script>
import OrderForm from '@/components/air/orderForm'
import OrderAside from '@/components/air/orderAside'
export default {
  components: {
    OrderForm,
    OrderAside
  },
  data() {
    return {
      infoData: {},
      allPrice: {}
    }
  },
  mounted() {
    const userId = this.$route.query
    this.$axios({
      url: `/airs/${userId.id}`,
      params: { seat_xid: userId.seat_xid }
    }).then(({ data }) => {
      this.infoData = data
    })
  },
  methods: {
    recPrice(data) {
      this.allPrice = data
    }
  }
}
</script>
<style lang="less" scoped>
.order-content {
  width: 1000px;
  margin: 20px auto;
  .main-content {
    .order-form {
     padding: 2px;
    }
    .order-aside {
      width: 350px;
      height: fit-content;
      border: 1px #ddd solid;
    }
  }
}
</style>
