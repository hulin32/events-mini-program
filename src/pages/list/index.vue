<template>
  <scroll-view>
    <view class="publish-wrapper">
      <navigator url="/pages/events-edit/main">
        <button class="publish-btn" type="primary">发布活动</button>
      </navigator>
    </view>
    <view class="title">已参加活动</view>
    <view class="event" v-for="enrollment in enrollments" :key="enrollment.time">
      <navigator url="/pages/events-edit/main">
        <text class="event-title">{{ enrollment.title }}({{ enrollment.time }}) </text>
        <text class="event-desc">{{ enrollment.desc }}</text>
        <text class="enrolled">已报名 10</text>
      </navigator>
    </view>
  </scroll-view>
</template>

<script>
export default {
  data() {
    return {
      enrollments: [],
    };
  },
  onLoad() {
    const enrollments = wx.getStorageSync('enrollment') || [];
    if (enrollments.length > 0) {
      this.enrollments = enrollments;
    }
  },
};
</script>

<style lang="scss" scoped>
#border {
  border-bottom: 1rpx solid #E5E5E5;
  border-top: 1rpx solid #E5E5E5;
}

.publish-btn {
  width: 50%;
  margin: 50rpx auto;
}
.title {
  font-size: 14px;
  padding: 10rpx 30rpx;
  border-top: 1rpx solid #E5E5E5;
}

.event {
  @extend #border;
  background-color: white;
  margin-bottom: 10rpx;
  padding: 10rpx 30rpx;
  .event-title {
    display: block;
    padding: 5rpx 0;
  }
  .event-desc {
    color: #999;
    font-size: 14px;
  }
  .enrolled {
    display: block;
    font-size: 14px;
    color: #888;
  }
}
</style>
<style>
page {
  background-color: #EFEFF4;
  height: 100%;
}
</style>
