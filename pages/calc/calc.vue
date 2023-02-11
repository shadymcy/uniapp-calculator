<template>
	<view class="content">
		<view class="layout-top">
			<view class="screen">
				{{screenData}}
			</view>
		</view>
		<view class="layout-bottom">
			<view class="btnGroup">
				<view class="item purple" @tap="clickBtn" :id="idc">C</view>
				<view class="item purple" @tap="clickBtn" :id="idb">←</view>
				<view class="item purple iconBtn" @tap="history">
					<icon :type="iconType" :color="iconColor" class="icon" size="25"></icon>
				</view>
				<view class="item purple" @tap="clickBtn" :id="idadd">+</view>
			</view>
			<view class="btnGroup">
				<view class="item blue" @tap="clickBtn" :id="id9">9</view>
				<view class="item blue" @tap="clickBtn" :id="id8">8</view>
				<view class="item blue" @tap="clickBtn" :id="id7">7</view>
				<view class="item purple" @tap="clickBtn" :id="idj">-</view>
			</view>
			<view class="btnGroup">
				<view class="item blue" @tap="clickBtn" :id="id6">6</view>
				<view class="item blue" @tap="clickBtn" :id="id5">5</view>
				<view class="item blue" @tap="clickBtn" :id="id4">4</view>
				<view class="item purple" @tap="clickBtn" :id="idx">x</view>
			</view>
			<view class="btnGroup">
				<view class="item blue" @tap="clickBtn" :id="id3">3</view>
				<view class="item blue" @tap="clickBtn" :id="id2">2</view>
				<view class="item blue" @tap="clickBtn" :id="id1">1</view>
				<view class="item purple" @tap="clickBtn" :id="iddiv">÷</view>
			</view>
			<view class="btnGroup">				
				<view class="item blue zero" @tap="clickBtn" :id="id0">0</view>
				<view class="item blue" @tap="clickBtn" :id="idd">.</view>
				<view class="item purple" @tap="clickBtn" :id="ide">=</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				idb:"back",
				idc:"clear",
				idt:"toggle",
				idadd:"＋",
				id9:"9",
				id8:"8",
				id7:"7",
				idj:"－",
				id6:"6",
				id5:"5",
				id4:"4",
				idx:"×",
				id3:"3",
				id2:"2",
				id1:"1",
				iddiv:"÷",
				id0:"0",
				idd:".",
				ide:"＝",
				screenData:"0",
				operaSymbo:{"＋":"+","－":"-","×":"*","÷":"/",".":"."},
				lastIsOperaSymbo:false,
				iconType:'waiting_circle',
				iconColor:'white',
				arr:[],
				logs:[]
			}
		},
		onLoad() {

		},
		methods: {
			clickBtn: function(event){
				var id = event.target.id
				if (id == this.idb){
					var data = this.screenData
					if (data == "0"){
						return
					}
					data = data.substring(0, data.length - 1)
					if (data == "" || data == "-"){
						data = 0
					}
					this.setData({screenData:"0"})
					this.arr.pop()
				}else if (id == this.idc){
					this.setData({screenData:"0"})
					this.arr.length = 0
				}else if (id == this.ide){
					var data = this.screenData
					if(data == "0"){
						return
					}
					//eval是js中window的一个方法，而微信页面的脚本逻辑在是在JsCore中运行，JsCore是一个没有窗口对象的环境，所以不能再脚本中使用window，也无法在脚本中操作组件                 
					//var result = eval(newData);
					var lastWord = data.charAt(data.length)
					if(isNaN(lastWord)){
						return
					}
					var num = ""
					var lastoperator = ""
					var arr = this.arr
					var optarr = []
					for (var i in arr){
						if(isNaN(arr[i]) == false || arr[i] == this.idd){
							num += arr[i]
						}else{
							lastoperator = arr[i]
							optarr.push(num)
							optarr.push(arr[i])
							num = ""
						}
					}
					optarr.push(Number(num))
					var ans = Number(optarr[0]) * 1.0
					console.log(ans);
					for(var i = 1; i < optarr.length; i++){
						if(isNaN(optarr[i])){
							if(optarr[1] == this.idadd){
								ans += Number(optarr[i + 1])
							}else if(optarr[1] == this.idj){
								ans -= Number(optarr[i + 1])
							}else if(optarr[1] == this.idx){
								ans *= Number(optarr[i + 1])
							}else if(optarr[1] == this.iddiv){
								ans /= Number(optarr[i + 1])
							}
							
						}
					}
					// 存储历史记录
					this.logs.push(data + ans)
					wx.setStorageSync("calclogs", this.logs)
					this.arr.length = 0
					this.arr.push(ans)
					this.setData({screenData: ans + ""})
				}else{
					if(this.operaSymbo[id]){//如果符号是+-*/
						if(this.lastIsOperaSymbo || this.screenData == "0"){
							return
						}
					}
					var sd = this.screenData
					var data
					if (sd == 0){
						data = id
					}else{
						data = sd + id
					}
					this.setData({screenData:data})
					this.arr.push(id)
					if(this.operaSymbo[id]){
						this.setData({lastIsOperaSymbo:true})
					}else{
						this.setData({lastIsOperaSymbo:false})
					}
					
				}
				
			},
			history:function(){
				uni.navigateTo({
					url:'../history/history'
				})
			}
		}
	}
</script>
<style>
@import './calc.css';
</style>
