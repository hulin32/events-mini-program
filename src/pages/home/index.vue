<template>
  <form>
    <div class="item">
      <label>
        <view class="center">标题</view>
        <view class="center">
          <input v-model="title" placeholder="title" />
        </view>
      </label>
    </div>

    <div class="item">
      <div class="center">时间({{year}}年{{month}}月{{day}}日)</div>
      <picker-view indicator-style="height: 50px;" class="picker-view" :value="value" @change="change">
        <picker-view-column>
          <view v-bind:key="year" v-for="year in years" class="picker-view-column">{{year}}年</view>
        </picker-view-column>
        <picker-view-column>
          <view v-bind:key="month" v-for="month in months" class="picker-view-column">{{month}}月</view>
        </picker-view-column>
        <picker-view-column>
          <view v-bind:key="day" v-for="day in days" class="picker-view-column">{{day}}日</view>
        </picker-view-column>
      </picker-view>
    </div>

    <div class="item">
      <label>
        <view class="center">活动内容</view>
        <view class="desc">
          <textarea v-model="desc" auto-height placeholder="活动内容" />
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
const year = date.getFullYear();
const month = date.getMonth();
const day = date.getDate();
for (let i = 1990; i <= date.getFullYear(); i += 1) {
  years.push(i);
}

for (let i = 1; i <= 12; i += 1) {
  months.push(i);
}

for (let i = 1; i <= 31; i += 1) {
  days.push(i);
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
      desc: '',
      title: '',
      value: [years.indexOf(year), months.indexOf(month), days.indexOf(day)],
    };
  },
  methods: {
    change(e) {
      const val = e.target.value;
      this.year = this.years[val[0]];
      this.month = this.months[val[1]];
      this.day = this.days[val[2]];
      this.value = [val[0], val[1], val[2]];
    },
    onClick() {
      const enrollment = wx.getStorageSync('enrollment') || [];
      enrollment.push({
        title: this.title,
        desc: this.desc,
        time: `${this.year}.${this.month}.${this.day}`,
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

<style>
.picker-view {
  text-align:center;
  width: 100%;
  height: 50px;
}
.picker-view-column {
  line-height: 50px;
}
.center {
  text-align: center;
}
.item {
  margin-bottom: 10px;
}
.desc {
  padding: 20px;
}
.submmit {
  width: 100px;
}
</style>
