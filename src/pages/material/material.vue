<template>
	<view class="container">
    <uni-nav-bar :border="false" backgroundColor="transparent" color="#1A1A1A" leftWidth='120rpx'>
      <template v-slot:left>
        <view class="nav-left-text">素材</view>
      </template>
      <template v-slot:right>
        <navigator url="/pages/basket/basket">
          <uni-badge :text="totalCartCount" absolute="rightTop" size="small" class="badge-car" :offset="[4, 4]">
            <image class="car" src="@/static/images/car.png"></image>
          </uni-badge>
        </navigator>
      </template>
    </uni-nav-bar>
    <view class="mat-swiper">
      <swiper autoplay="true">
        <template v-for="(item, index) in bannerList" :key="item.id">
          <swiper-item>
            <image :src="item.bannerUrl" mode="aspectFill" ></image>
          </swiper-item>
        </template>
      </swiper>
    </view>
    <view class="matter-list">
      <template
        v-for="(item, index) in matterList"
        :key="item.id"
      >
          <view class="matter" @tap="goDetail(item.id)">
            <view class="title">{{item.title}}</view>
            <view class="content">{{item.content}}{{item.id}}</view>
          </view>
      </template>
    </view>
    <uni-load-more :status="moreStatus" iconSize="14"></uni-load-more>
    <my-tabbar :value='2'></my-tabbar>
	</view>

</template>

<script setup>
  const totalCartCount = ref(0)
  const bannerList = ref([]);
  const pageNo = ref(1);
  const moreStatus = ref('loading')
  const matterObj = shallowRef({
    records: [],
    pages: 0,
    total: 0,
    size: 0,
    current: 1
  })
  const matterList = ref([]);
  onLoad(() => {
    getMatterBanner();
    getMatterList({pageNo: 1, pageSize: 10})
  })
  onShow(() => {
    http.getCartCount() // 重新计算购物车总数量
    setTimeout(() => {
      totalCartCount.value = getApp().globalData.totalCartCount
    }, 600)
  })
  onPullDownRefresh(() => {
    // 模拟加载
    setTimeout(() => {
      uni.stopPullDownRefresh() // 停止下拉刷新
    }, 100)
  })
  /**
   * 页面上拉触底事件的处理函数
   */
  onReachBottom(() => {
    if (pageNo.value < matterObj.value.pages) {
      pageNo.value = pageNo.value + 1;
      getMatterList({pageNo: pageNo.value, pageSize: 10})
    } else {
      moreStatus.value = "noMore";
    }

  })
  // banner
  const getMatterBanner = () => {
    http.request({
      url: '/matter/banner-list',
      method: 'GET',
    }).then((res) => {
      if (res.success) {
        bannerList.value = res.data
      }
    })
  }
  // 公共-素材列表
  const getMatterList = async (pageObje) => {
    await http.request({
      url: '/matter/list',
      method: 'GET',
      data: pageObje,
    }).then((res) => {
      if (res.success) {
        matterObj.value = res.data;
        const { current, pages } = res.data
        pageNo.value = current;
        if (pages >= current) {
          matterList.value = [...matterList.value, ...res.data.records]
          moreStatus.value = pages === 1 ? "no-more" : "more";
        }
      }
    })
  }
  const goDetail = (id) => {
    uni.navigateTo({
      url: '/pages/material/detail?id=' + id
    })
  }
</script>

<style lang="scss">
@use './material.scss';
</style>
