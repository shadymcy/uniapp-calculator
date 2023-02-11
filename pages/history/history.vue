<template>
    <view class="content">
        <block v-for="(log, index) in logs" :key="index">
            <view class="item">{{ log }}</view>
        </block>
        <!-- <scroll-view scroll-y="true" scroll-x="true" style="height: 500px;" > -->

        <!--<view class="item">1</view>
        <view class="item">1</view>
        <view class="item">1</view>
        <view class="item">1</view>
        <view class="item">1</view>
        <view class="item">1</view>
        <view class="item">1</view>
        <view class="item">1</view>
        <view class="item">1</view>
        <view class="item">1</view>
        <view class="item">1</view>
        <view class="item">1</view>-->
        <!-- </scroll-view> -->
    </view>
</template>

<script>
export default {
    data() {
        return {
            // text:"这是一个页面"
            logs: [],

            log: ''
        };
    },
    onLoad: function (options) {
        // 页面初始化 options为页面跳转所带来的参数
        var logs = uni.getStorageSync('calclogs');
        this.setData({
            logs: logs
        });
    },
    onReady: function () {
        // 页面渲染完成
    },
    onShow: function () {
        // 页面显示
    },
    onHide: function () {
        // 页面隐藏
    },
    onUnload: function () {
        // 页面关闭
    },
    methods: {}
};
</script>
<style>
@import './history.css';
</style>
