<template>
	<view
    class="commodity"
    :class="direction"
    :data-prodid="prod.prodId"
    @tap="toProdPage">
    <view class="commodity-imagecont">
      <img-show
        imgMode="aspectFill"
        :src="prod.pic"
        :class-list="['hotsaleimg']"
      />
    </view>
    <view class="prodName-brief">
      <view class="prodName">
        {{ prod.prodName }}
      </view>
    <!--  <view class="prod-info">
        {{ prod.brief }}
      </view> -->
      <view class="prod-text-info">
        <view class="price">
          <text class="symbol">￥</text>
          <text class="big-num">
            {{ wxs.parsePrice(prod.price)[0] }}.{{ wxs.parsePrice(prod.price)[1] }}
          </text>
        </view>
      </view>
    </view>
	</view>
</template>

<script setup>
  const { prod, direction } = defineProps(['prod', 'key', 'direction']);
  const wxs = number()
  const toProdPage = (e) => {
    const prodid = e.currentTarget.dataset.prodid
    if (prodid) {
      uni.navigateTo({
        url: '/pages/prod/prod?prodid=' + prodid
      })
    }
  }
</script>

<style lang="scss">
  .commodity{
    background-color: #fff;
    width: 340rpx;
    height: 520rpx;
    border-radius: 12rpx;
    margin-bottom: 24rpx;
    &.horizontal{
      height: auto;
      width: auto;
      margin: 0 24rpx 24rpx 24rpx;
      display: flex;
      padding: 24rpx;
      .commodity-imagecont{
        width: 180rpx;
        height: 180rpx;
        overflow: hidden;
        margin: 0;
        .hotsaleimg{
          width: 180rpx;
          height: 180rpx;
        }
      }
      .prodName-brief{
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        width: 450rpx;
        .prodName{
          padding: 0 0 0 24rpx;
        }
      }
    }
    .commodity-imagecont{
      width: 316rpx;
      height: 316rpx;
      .hotsaleimg{
        width: 316rpx;
        height: 316rpx;
      }
      border-radius: 12rpx;
      overflow: hidden;
      margin: 12rpx;
    }
    .prodName-brief{
      .prodName{
        color: #1A1A1A;
        font-size: 30rpx;
        font-weight: 400;
        height: 80rpx;
        line-height: 40rpx; /* 133.333% */
        overflow: hidden;
        display: -webkit-box;
        word-break: break-all;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
        padding: 12rpx 12rpx 0 12rpx;
      }
      .prod-text-info{
        margin-top: 24rpx;
        .price{
          color: #E5432C;
          padding-left: 24rpx;
          .symbol{
            font-size: 28rpx;
          }
          .big-num{
            font-size: 36rpx;
          }
        }

      }
    }
  }
</style>
