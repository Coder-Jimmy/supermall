<template>
  <div class="tab-bar-item" @click="itemClick">
    <div v-if="!isActive">
      <slot name="item-icon"></slot>
    </div>
    <div v-else>
      <slot name="item-icon-active"></slot>
    </div>
    <div :style="activeStyle">
      <slot name="item-text"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "TarBarItem",
  props: {
    path: String,
    activeColor: {
      type: String,
      default: "deeppink",
    },
  },
  computed: {
    isActive() {
      return this.$route.path.indexOf(this.path) !== -1;
    },
    activeStyle() {
      //indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。找不到则返回-1，区分大小写
      //  /home -> item1(/home) = true
      //  /home -> item1(/category) = false
      //  /home -> item1(/shopcart) = false
      //  /home -> item1(/profile) = false
      return this.isActive ? { color: this.activeColor } : {};
    },
  },
  methods: {
    itemClick() {
      this.$router.replace(this.path);
    },
  },
};
</script>

<style>
.tab-bar-item {
  flex: 1;
  text-align: center;
  height: 49px;
  font-size: 14px;
}

.tab-bar-item img {
  width: 20px;
  height: 20px;
  margin-top: 3px;
  vertical-align: middle;
  margin-bottom: 2px;
}
</style>