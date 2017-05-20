<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support" @click="showDetail">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <div v-show="detailShow" class="detail" transition="fade">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h1 class="detail-main-name">{{seller.name}}</h1>
          <div class="star-wrapper">
            <star :size="48" :score="seller.score"></star>
          </div>
          <div class="detail-title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div>
          <ul v-if="seller.supports" class="detail-support">
            <li class="support-item" v-for="item in seller.supports">
              <span class="icon" :class="classMap[seller.supports[$index].type]"></span>
              <span class="text">{{seller.supports[$index].description}}</span>
            </li>
          </ul>
          <div class="detail-title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="seller-bulletin">
            <p class="content">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="hideDetail">
        <div class="icon-close">close</div>
      </div>
    </div>
  </div>
</template>
<style>
  .header {
    position: relative;
    overflow: hidden;
    color: #fff;
    background: rgba(7, 17, 27, 0.5);
  }

  .content-wrapper {
    padding: 24px 12px 18px 24px;
    font-size: 0;
    position: relative;
  }

  .avatar {
    display: inline-block;
    vertical-align: top;
  }

  img {
    border-radius: 2px;
  }

  .content {
    display: inline-block;
    margin-left: 16px;
    font-size: 14px;
  }

  .title {
    margin: 2px 0 8px 0;
  }

  .brand {
    display: inline-block;
    vertical-align: top;
    width: 30px;
    height: 18px;
    background-image: url("brand@2x.png");
    background-size: 30px 18px;
    background-repeat: no-repeat;
  }

  .name {
    margin-left: 6px;
    font-size: 16px;
    line-height: 18px;
    font-weight: bold;
  }

  .description {
    margin-bottom: 10px;
    line-height: 12px;
    font-size: 12px;
  }

  .support .icon {
    display: inline-block;
    vertical-align: top;
    width: 12px;
    height: 12px;
    margin-right: 4px;
    background-size: 12px 12px;
    background-repeat: no-repeat;
  }

  .support .decrease {
    background-image: url("decrease_1@2x.png");
  }

  .support .discount {
    background-image: url("discount_1@2x.png");
  }

  .support .guarantee {
    background-image: url("guarantee_1@2x.png");
  }

  .support .invoice {
    background-image: url("invoice_1@2x.png");
  }

  .support .special {
    background-image: url("special_1@2x.png");
  }

  .support .icon .text {
    vertical-align: top;
    font-size: 10px;
    line-height: 12px;
  }

  .support-count {
    position: absolute;
    right: 12px;
    bottom: 18px;
    padding: 0 8px;
    height: 24px;
    line-height: 24px;
    border-radius: 14px;
    background-color: gainsboro;
    text-align: center;
  }

  .support-count .count {
    vertical-align: top;
    font-size: 10px;
  }

  .bulletin-wrapper {
    height: 28px;
    line-height: 28px;
    padding: 0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .bulletin-title {
    display: inline-block;
    vertical-align: top;
    margin-top: 8px;
    width: 22px;
    height: 12px;
    background-image: url("bulletin@2x.png");
    background-size: 22px 12px;
    background-repeat: no-repeat;
  }

  .bulletin-text {
    vertical-align: top;
    margin: 0 4px;
    font-size: 10px;
  }

  .background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
  }

  .detail {
    position: fixed;
    z-index: 100;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background: rgba(7, 17, 27, 0.8);
    transition: all 0.5s;
  }

  .detail-wrapper {
    min-height: 100%;
    width: 100%;
  }

  .detail-main {
    margin-top: 64px;
    padding-bottom: 64px;
  }

  .detail-close {
    position: relative;
    width: 32px;
    height: 32px;
    margin: -64px auto 0 auto;
    clear: both;
    font-size: 32px;
  }

  .clearfix {
    display: inline-block;
  }

  .clearfix :after {
    display: block;
    content: ".";
    height: 0;
    line-height: 0;
    clear: both;
    visibility: hidden;
  }

  .detail-main-name {
    line-height: 16px;
    font-weight: 700;
    text-align: center;
    font-size: 16px;
  }

  .star-wrapper {
    margin-top: 18px;
    padding: 2px 0;
    text-align: center;
  }

  .detail-title {
    width: 80%;
    margin: 28px auto 24px auto;
    display: flex;
  }

  .detail-title .line {
    flex: 1;
    position: relative;
    top: -6px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  }

  .detail-title .text {
    padding: 0 12px;
    font-size: 14px;
    font-weight: 700;
  }

  .detail-support {
    width: 80%;
    margin: 0 auto;
  }

  .detail-support .support-item {
    padding: 0 12px;
    margin-bottom: 12px;
    font-size: 0;
  }

  .detail-support .icon {
    display: inline-block;
    width: 16px;
    height: 16px;
    vertical-align: top;
    margin-right: 6px;
    background-size: 16px;
    background-repeat: no-repeat;
  }

  .detail-support .decrease {
    background-image: url("decrease_2@2x.png");
  }

  .detail-support .discount {
    background-image: url("discount_2@2x.png");
  }

  .detail-support .guarantee {
    background-image: url("guarantee_2@2x.png");
  }

  .detail-support .invoice {
    background-image: url("invoice_2@2x.png");
  }

  .detail-support .special {
    background-image: url("special_2@2x.png");
  }

  .detail-support .text {
    line-height: 16px;
    font-size: 12px;
  }

  .seller-bulletin {
    width: 80%;
    margin: 0 auto;
  }

  .seller-bulletin content {
    line-height: 24px;
    font-size: 12px;
    padding: 0 12px;
  }

  .fade-transition {
    opacity: 1;
    background: rgba(7, 17, 27, 0.8);
  }

  .fade-enter {
    opacity: 0;
    background: rgba(7, 17, 27, 0.8);
  }

  .fade-leave {
    opacity: 0;
    background: rgba(7, 17, 27, 0);
  }
</style>
<script>
  import star from 'components/star/star.vue'
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        detailShow: false
      }
    },
    methods: {
      showDetail() {
        this.detailShow = true;
      },
      hideDetail() {
        this.detailShow = false;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    components: {
      star: star
    }
  }
</script>
