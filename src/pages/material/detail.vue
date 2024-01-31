<template>
  <view class="container">
    <view class="detail">
      <view class="bg">
        <view class="title">{{detail.title}}</view>
        <view class="content">
          <text>{{detail.content}}</text>
          <template v-for="(item, index) in detail.imageUrls">
            <image :src="item" mode="aspectFit"></image>
          </template>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup>
  import { onLoad } from '@dcloudio/uni-app';
  const detail = ref({});
  onLoad((query) => {
    query && getDetail(query.id)
  })
  const getDetail = async (id) => {
   await http.request({
      url: '/matter/detail',
      method: 'GET',
      data: { id }
    }).then((res) => {
      if (res.success) {
        detail.value = res.data;
      }
    })
  }
</script>

<style scoped lang="scss">
  .container{
    background: #F6F6F6;
    padding: 24rpx;
    box-sizing: border-box;
    min-height: calc(100vh - var(--window-top));
    .detail{
      border-radius: 12rpx;
      overflow: hidden;
      background: linear-gradient(180deg, #FFF8EF 0%, #FFF 100%);
      .bg{
        background: url('@/static/images/matter-bg.png') no-repeat;
        background-size: contain;
        padding: 60rpx 24rpx;
      }
      .title{
        color: #1A1A1A;
        text-align: center;
        font-size: 36rpx;
        font-style: normal;
        font-weight: 500;
        line-height: 40rpx;
      }
      .content{
        margin-top: 48rpx;
        text{
          line-height: 40rpx;
          color: #1A1A1A;
          font-size: 28rpx;
          font-weight: 400;
        }
        image {
          width: 100%;
          height: 240rpx;
          margin-top: 24rpx;
        }
      }
    }

  }
</style>

