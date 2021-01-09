<template>
  <div class="container">
    <div
      class="container-resource"
      v-for="(item, index) in resourceList"
      :key="index"
    >
      <div
        class="container-resource-title"
        v-text="item.name"
        :class="[{ titleActive: item.active }]"
        @click="showDetails(item)"
      ></div>
      <div
        class="container-resource-innerbox"
        v-show="item.isShow"
        v-for="(subItem, subIndex) in item.data"
        :key="subIndex"
      >
        <div class="container-resource-innerbox-item">{{ subItem.name }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "device-monitor",
  props: {
    resourceList: {
      type: Array
    }
  },
  methods: {
    resourceListTitle(index) {
      this.$emit("resourceListTitle", index);
    },
    resourceListContext(subIndex) {
      this.$emit("resourceListContext", subIndex);
    },
    showDetails(item) {
      item.isShow === false ? (item.isShow = true) : (item.isShow = false);
    }
  },
  mounted() {
    this.resourceList.forEach(i => {
      i.isShow === undefined
        ? this.$set(i, "isShow", false)
        : (i.isShow = false);
    });
  }
};
</script>

<style lang="scss" scoped>
.container {
  width: 450px;
  position: absolute;
  top: 500px;
  right: 10px;
  z-index: 2;
  &-resource {
    &-title {
      width: 100%;
      height: 49px;
      margin: 0 0 5px 0;
      background: url("../assets/images/echartsTitle.png") no-repeat center;
      background-size: 100% 100%;
      line-height: 49px;
      color: aqua;
      text-indent: 25px;
      font-weight: bold;
      font-size: 15px;
      letter-spacing: 2px;
    }
    &-innerbox {
      position: relative;
      left: 10px;
      &-item {
        width: 100%;
        height: 40px;
        margin: 0 0 5px 0;
        background: url("../assets/images/echartsTitle.png") no-repeat center;
        background-size: 100% 100%;
        line-height: 40px;
        color: aqua;
        text-indent: 25px;
        font-weight: bold;
        font-size: 12px;
        letter-spacing: 2px;
      }
    }
  }
}
</style>
