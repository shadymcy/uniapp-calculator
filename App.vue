<script>
//app.js
export default {
    data() {
        return {};
    },
    onLaunch: function () {
        //调用API从本地缓存中获取数据
        var logs = uni.getStorageSync('logs') || [];
        logs.unshift(Date.now());
        uni.setStorageSync('logs', logs);
    },
    globalData: {
        userInfo: null,

        getUserInfo: function (cb) {
            var that = this;
            if (this.globalData.userInfo) {
                if (typeof cb == 'function') {
                    cb(this.globalData.userInfo);
                }
            } else {
                //调用登录接口
                uni.login({
                    success: function () {
                        uni.getUserInfo({
                            success: function (res) {
                                that.globalData.userInfo = res.userInfo;
                                if (typeof cb == 'function') {
                                    cb(that.globalData.userInfo);
                                }
                            }
                        });
                    }
                });
            }
        }
    }
};
</script>
<style>
@import './app.css';
</style>
