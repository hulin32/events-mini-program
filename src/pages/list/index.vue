<template>
  <view>
    <view>{{ title }}({{ time }}) </view>
    <view>{{ desc }} </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      title: '',
      desc: '',
      time: '',
    };
  },
  onLoad() {
    const enrollments = wx.getStorageSync('enrollment') || [];
    if (enrollments.length > 0) {
      const enrollment = enrollments[0];
      this.title = enrollment.title;
      this.desc = enrollment.desc;
      this.time = enrollment.time;
    }
  },
};
</script>

