<template>
  <div class="bottom-menu">
    <!-- <check-button
      class="select-all"
      @checkBtnClick="checkBtnClick"
      :is-checked="isSelectAll"
    ></check-button> -->
    <check-button
      class="select-all"
      @click.native="checkBtnClick"
      :is-checked="isSelectAll"
    ></check-button>
    <span>全选</span>
    <span class="total-price">合计: ¥{{ totalPrice }}</span>
    <span class="buy-product" @click="calcClick">去结算({{ checkCount }})</span>
  </div>
</template>

<script>
import CheckButton from "components/content/checkButton/CheckButton";
import { mapGetters } from "vuex";

export default {
  name: "CarBottomBar",
  components: {
    CheckButton,
  },
  computed: {
    ...mapGetters(["cartList"]),
    totalPrice() {
      const cartList = this.$store.getters.cartList;
      return cartList
        .filter((item) => {
          return item.checked;
        })
        .reduce((preValue, item) => {
          return preValue + item.count * item.price;
        }, 0)
        .toFixed(2);
    },
    checkCount() {
      return this.cartList
        .filter((item) => (item.checked, item.count))
        .reduce((preValue, item) => {
          return preValue + item.count * item.checked;
        }, 0);
    },
    isSelectAll() {
      //第一种方法  filter()会遍历完，性能差
      //if (this.cartList.length === 0) return false;
      //return !this.cartList.filter((item) => !item.checked).length;

      //第二种方法 find()性能高
      if (this.cartList.length === 0) return false;
      return !this.cartList.find((item) => !item.checked);

      //第三种 普通遍历
      // if (this.cartList.length === 0) return false;
      // for (let item of this.cartList) {
      //   if (!item.checked) {
      //     if (!item.checked) {
      //       return false;
      //     }
      //   }
      // }
    },
  },
  methods: {
    checkBtnClick() {
      // 有未选中的内容, 则全部选中
      if (this.isSelectAll) {
        this.$store.state.cartList.forEach((item) => {
          item.checked = false;
        });
      } else {
        this.$store.state.cartList.forEach((item) => {
          item.checked = true;
        });
      }
    },
    calcClick() {
      if (!this.cartList.filter((item) => item.checked).length) {
        this.$toast.show("请选择购买的商品", 1500);
      }
    },
  },
};
</script>

<style scoped>
.bottom-menu {
  width: 100%;
  height: 44px;
  background-color: #eee;
  position: fixed;
  bottom: 50px;
  left: 0;
  box-shadow: 0 -2px 3px rgba(0, 0, 0, 0.2);
  font-size: 14px;
  color: #888;
  line-height: 44px;
  padding-left: 35px;
  box-sizing: border-box;
}

.bottom-menu .select-all {
  position: absolute;
  line-height: 0;
  left: 12px;
  top: 13px;
}

.bottom-menu .total-price {
  margin-left: 15px;
  font-size: 16px;
  color: #666;
}

.bottom-menu .buy-product {
  background-color: orangered;
  color: #fff;
  width: 100px;
  height: 44px;
  text-align: center;
  line-height: 44px;
  float: right;
}
</style>
