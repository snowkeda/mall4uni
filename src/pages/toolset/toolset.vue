<template>
  <web-view src="http://192.168.101.3:9527/home?id=122"></web-view>
	<my-tabbar :value='1'></my-tabbar>
</template>

<script setup>
  uni.getStorageSync('Token')
</script>

<style scoped lang="scss">

</style>
