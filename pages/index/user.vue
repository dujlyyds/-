<template>
	<view class="container">
		<view class="headInfo">
			<image class="head_img" :src="userinfo.avatarUrl" mode="widthFix"></image>
			<view class="">
				<view class="name">
					Hi {{userinfo.nickName}}
				</view>
				<view class="">
					欢迎您
				</view>
				
			</view>
			<!-- <view class="" @click="call">
				打电话
			</view> -->
		</view>
		
		<view class="grid">
			<view class="" @click="vibrateLong">
				震动
			</view>
			<view class="">
				电量{{batteryInfo.level ||50}}
			</view>
			<view class="" @click="choosepoi">
				今天吃什么
			</view>
		</view>
		<view class="" v-if="showFood">
			{{checkVal.title}} 位于:{{checkVal.address}}
		</view>
	
	</view>
</template>

<script>
	var bmap = require('../../libs/bmap-wx.js'); 
	var wxMarkerData = []; 
	export default {
		data() {
			return {
				userinfo:{},
				markers: [], 
				showFood:false,
				        latitude: '', 
				        longitude: '', 
				        placeData: {} ,
						batteryInfo:{},
						checkVal:{}
			}
		},
		onLoad() {
			this.userinfo =  uni.getStorageSync('userinfo')
			wx.getLocation({
				type: 'wgs84',
				 success :(res)=> {
				   const latitude = res.latitude
				   const longitude = res.longitude
				   const speed = res.speed
				   const accuracy = res.accuracy
				   this.latitude = latitude
				   this.longitude =longitude 
				   
				 }
			})
			this.getBatteryInfo()
			
		
			
		},
		methods: {
			call() {
				console.log(11);
				wx.chooseContact({
					complete: (res) => {
							console.log(res);
					
					}
				})
				
					
			},
			vibrateLong(){
				wx.vibrateLong();
			},
			getBatteryInfo(){
				wx.getBatteryInfo({
						success:(res)=>{
							console.log(res);
							this.batteryInfo =res
						}
				})
			},
			choosepoi(){
				
				var that = this;
				        // 新建百度地图对象 
				        var BMap = new bmap.BMapWX({ 
				            ak: 'plVR1bge0nlWIEeAduueDq3rE8Djx2yV' 
				        }); 
				        var fail = function(data) { 
				            console.log(data) 
				        }; 
				        var success = function(data) { 
				            wxMarkerData = data.wxMarkerData; 
							console.log(wxMarkerData);
						
				        } 
						// 发起POI检索请求 
						        BMap.search({ 
						            "query": '美食', 
									location:`${this.latitude},${this.longitude} `,
						            fail: fail, 
						            success: success, 
						           
						        }); 
						
						var arr = Math.floor(Math.random()*wxMarkerData.length);
						var that =this
						setTimeout(()=>{
							that.checkVal = wxMarkerData[arr];
							console.log(that.checkVal);
						},100)
						this.showFood = true
						
						
			}
		},
	}
</script>

<style lang="scss" scoped>
	.headInfo{
		display:flex;
		justify-content:left ;
		align-items: center;
		width: 100%;
		height: 300rpx;
		background-color: #E2EAF6;
		.head_img{
			width: 120rpx;
			border-radius: 50%;
			margin-right: 30rpx;
			margin: 100rpx;
		}
		.name{
			font-size: 34rpx;
			color: #fc7203;
		}
	}
	.map_container{ 
	    height: 300px; 
	    width: 100%; 
	} 
	
	.map { 
	    height: 100%; 
	    width: 100%; 
	} 
	.grid{
		display: flex;
		justify-content: left;
		align-items: center;
		view{
			width: 300rpx;
			height: 300rpx;
		}
	}
</style>
