<template>
  <uv-tabbar
    :value="value"
    @change="handleChange"
    :fixed="true"
    :border="false"
    zIndex="999"
    inactiveColor="#1A1A1A"
    activeColor="#E5432C"
  >
    <template v-for="(item, index) in tabbarList">
      <uv-tabbar-item :text="item.text">
        <template v-slot:active-icon>
          <image class="icon" :src="item.activeIcon"></image>
        </template>
        <template v-slot:inactive-icon>
          <image class="icon" :src="item.inactiveIcon"></image>
        </template>
      </uv-tabbar-item>
    </template>
  </uv-tabbar>
</template>

<script setup>
  const { value } = defineProps(['value'])
  const toolLink = ref(null)
  const tabbarList = ref([
    {text: '首页', inactiveIcon: '/static/images/tabbar/homepage.png', activeIcon: '/static/images/tabbar/homepage-sel.png', url: '/pages/index/index'},
    {text: '工具集合', inactiveIcon: '/static/images/tabbar/toolset.png', activeIcon: '/static/images/tabbar/toolset-sel.png', url: '/pages/toolset/toolset'},
    {text: '素材', inactiveIcon: '/static/images/tabbar/material.png', activeIcon: '/static/images/tabbar/material-sel.png', url: '/pages/material/material'},
    {text: '我的', inactiveIcon: '/static/images/tabbar/user.png', activeIcon: '/static/images/tabbar/user-sel.png', url: '/pages/user/user'},
  ])
  const handleChange = (index) => {
    console.log(tabbarList.value[index].url)
    uni.switchTab({
      url: tabbarList.value[index].url
    })
    /* if (index !== 1) {

    } else {
      console.log()
      console.log(toolLink)
      uni.navigateTo({
        url: tabbarList.value[index].url
      })
    } */
  }
</script>

<style scoped lang="scss">
  .icon {
    width: 64rpx;
    height: 64rpx;
  }
</style>
