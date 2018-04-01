<template>
  <form class="page">
    <view class="item">
      <label>
        <view class="title">标题</view>
        <view class="input-wrapper">
          <input class="input" v-model="title" placeholder="标题" />
        </view>
      </label>
    </view>

    <div class="item">
      <div class="title">时间({{year}}/{{month}}/{{day}}/{{hour}}/{{min}})</div>
      <picker-view indicator-style="height: 44rpx;" class="picker-view" :value="value" @change="change">
        <picker-view-column>
          <view v-bind:key="year" v-for="year in years" class="picker-view-column">{{year}}年</view>
        </picker-view-column>
        <picker-view-column>
          <view v-bind:key="month" v-for="month in months" class="picker-view-column">{{month}}月</view>
        </picker-view-column>
        <picker-view-column>
          <view v-bind:key="day" v-for="day in days" class="picker-view-column">{{day}}日</view>
        </picker-view-column>
        <picker-view-column>
          <view v-bind:key="hour" v-for="hour in hours" class="picker-view-column">{{hour}}时</view>
        </picker-view-column>
        <picker-view-column>
          <view v-bind:key="min" v-for="min in mins" class="picker-view-column">{{min}}分</view>
        </picker-view-column>
      </picker-view>
    </div>

    <div class="item">
      <label>
        <view class="title">活动内容</view>
        <view class="desc-wrapper">
          <textarea cursor-spacing="50" class="desc" v-model="desc" auto-height placeholder="活动内容" />
        </view>
      </label>
    </div>

    <div class="item">
      <button class="submmit" type="primary" @click.exact="onClick">提交</button>
    </div>
  </form>
</template>

<script>
const date = new Date();
const years = [];
const months = [];
const days = [];
const hours = [];
const mins = [];
const year = date.getFullYear();
const month = date.getMonth();
const day = date.getDate();
const hour = date.getHours();
const min = date.getMinutes();
for (let i = date.getFullYear(); i <= date.getFullYear() + 2; i += 1) {
  years.push(i);
}

for (let i = 1; i <= 12; i += 1) {
  months.push(i);
}

for (let i = 1; i <= 31; i += 1) {
  days.push(i);
}

for (let i = 1; i <= 24; i += 1) {
  hours.push(i);
}

for (let i = 1; i <= 60; i += 1) {
  mins.push(i);
}
export default {
  data() {
    return {
      years,
      year,
      months,
      month,
      days,
      day,
      hours,
      hour,
      mins,
      min,
      desc: '',
      title: '',
      value: [
        years.indexOf(year),
        months.indexOf(month),
        days.indexOf(day),
        hours.indexOf(hour),
        mins.indexOf(min),
      ],
    };
  },
  onLoad() {
    console.log('this.$route', this.$mp.query.id);
  },
  methods: {
    change(e) {
      const val = e.target.value;
      this.year = this.years[val[0]];
      this.month = this.months[val[1]];
      this.day = this.days[val[2]];
      this.hour = this.hours[val[3]];
      this.min = this.mins[val[4]];
      this.value = [val[0], val[1], val[2], val[3], val[4]];
    },
    onClick() {
      const enrollment = wx.getStorageSync('enrollment') || [];
      enrollment.push({
        title: this.title,
        desc: this.desc,
        time: `${this.year}/${this.month}/${this.day}/${this.hour}/${this.min}`,
      });
      wx.setStorage({
        key: 'enrollment',
        data: enrollment,
      });
      const url = '/pages/list/main';
      wx.navigateTo({ url });
    },
    getUserInfo() {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo;
              console.log('res', res);
            },
          });
        },
      });
    },
  },
};
</script>

<style lang="scss" scoped>
#border {
  border-bottom: 1rpx solid #E5E5E5;
  border-top: 1rpx solid #E5E5E5;
}
.picker-view {
  text-align:center;
  @extend #border;
  height: 44rpx;
  background-color: white;
}
.picker-view-column {
  background-color: white;
}

.title {
  color: #888;
  font-size: 14px;
  line-height: 88rpx;
  padding: 0 30rpx;
}

.input-wrapper {
  background-color: white;
  padding: 0 30rpx;
  @extend #border;

  .input {
    font-size: 17px;
    height: 88rpx;
  }
}

.desc-wrapper {
  padding: 0 30rpx;
  background-color: white;
    padding-top: 10rpx;
  margin-bottom: 20rpx;
  @extend #border;

  .desc {
    min-height: 200rpx;
  }
}
.submmit {
  width: 200rpx;
}
</style>
<style>
page {
  background-color: #EFEFF4;
  height: 100%;
}
</style>

