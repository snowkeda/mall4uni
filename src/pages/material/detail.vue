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
      <!-- <button @tap="saveImages(detail.imageUrls)">保存图片素材</button> -->
      <!-- <button class="previewBtn" @click="toDownload(imgs)">保存图片素材</button> -->
    </view>
  </view>
</template>

<script setup>
  import { onLoad } from '@dcloudio/uni-app';
  const detail = ref({});
  const fileUrl = ref(null);
  const fileType = ref(null)
  const imgs = ref([
    {
      type: '1',
      name: '下载图片',
      src: '/static/images/cloud-icon.png'
    },{
      type: '1',
      name: '下载图片',
      src: '/static/images/matter-bg.png'
    },
  ])
  onLoad((query) => {
    console.log(query)
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
  //H5下载图片到本地
  const downloadH5 = (url) => {
   uni.downloadFile({
  		url: url, //仅为示例，并非真实的资源
  		success: (res) => {
  			console.log(res)
  			if (res.statusCode === 200) {
  				console.log('下载成功');
  				var oA = document.createElement("a");
  				oA.download = Date.now(); // 设置下载的文件名，默认是'下载'
  				oA.href = res.tempFilePath; //临时路径再保存到本地
  				document.body.appendChild(oA);
  				oA.click();
  				// oA.remove(); // 下载之后把创建的元素删除
  			}
  		}
  	});
  }

  const saveImages = (urls, index) => {
    // #ifdef H5
    urls.forEach(function(url) {
      setTimeout(() => {
        downloadH5(url)

      }, 500 * index)
    })
    // #endif
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

