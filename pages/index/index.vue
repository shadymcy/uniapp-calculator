<template>
    <!-- index.wxml -->
    <view class="container">
		<!-- tap和click都是点击事件。不过移动端有太多复杂的功能是click监听不到的，例如，触摸、按住和轻滑。这时候就要用tap方法了。
			另外，click事件是点击放开之后才触发的，所以时间上会有延迟，大概200-300ms，可是我们在移动端的话就比较追求速度，所以就不能出现说有延迟的情况。所以用tap来代替click事件的话，对于针对移动设备的产品都适合。-->

        <view @tap="bindViewTap" class="userinfo">
            <image class="userinfo-avatar" :src="userInfo.avatarUrl" background-size="cover"></image>
        </view>
        <view>
            <text class="userinfo-nickname">{{ userInfo.nickName }}（Shady）</text>
        </view>
        <view class="usermotto">
            <!-- <text class="user-motto">{{motto}}</text> -->
            <button type="default" :size="primarySize" :plain="plain" hover-class="button-hover" :disabled="disabled" @tap="toCalc">{{ motto }}</button>
        </view>
        <view>
            <navigator url="view" class="github" hover-class="navigator-hover">
                <icon :type="iconType" class="icon" size="20" />
                GitHub
            </navigator>
        </view>
    </view>
</template>

<script>
//index.js
//获取应用实例
var app = getApp();
export default {
    data() {
        return {
            motto: '简易计算器☞',

            userInfo: {
                avatarUrl: '',
                nickName: ''
            },

            defaultSize: 'default',
            disabled: false,
            iconType: 'info_circle',
            primarySize: '',
            plain: ''
        };
    },
    onLoad: function () {
        console.log('onLoad');
        var that = this;
        //调用应用实例的方法获取全局数据
        app.globalData.getUserInfo(function (userInfo) {
            //更新数据
            that.setData({
                userInfo: userInfo
            });
        });
    },
    methods: {
        //事件处理函数
        bindViewTap: function () {
            uni.navigateTo({
                url: '../logs/logs'
            });
        },

        toCalc: function () {
            uni.navigateTo({
                url: '../calc/calc'
            });
        }
    }
};
</script>
<style>
@import './index.css';
</style>
