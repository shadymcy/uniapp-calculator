<template>
    <!-- logs.wxml -->
    <view class="container log-list">
        <block v-for="(log, index) in logs" :key="index">
            <text class="log-item">{{ index + 1 }}. {{ log }}</text>
        </block>
    </view>
</template>

<script>
//logs.js
var util = require('../../utils/util.js');
export default {
    data() {
        return {
            logs: [],
            log: ''
        };
    },
    onLoad: function () {
        this.setData({
            logs: (uni.getStorageSync('logs') || []).map(function (log) {
                return util.formatTime(new Date(log));
            })
        });
    },
    methods: {}
};
</script>
<style>
@import './logs.css';
</style>
