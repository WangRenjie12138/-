<template>
	<view>
		
		<u-navbar :is-back="false" :title="city" :background="background" title-color="#ffffff" title-size="42">
			<view class="navbar-right" slot="right" >
				<view class="right-item">
					<u-image src="@/static/img/qiehuan.png" width="40rpx" height="40rpx" :fade="false" @click="showPickCity = true" ></u-image>
				</view>
			</view>
			
		</u-navbar>
		
		<!-- 定义样式 -->
		<view>
			
			<!-- 城市选择层 -->
			<u-picker v-model="showPickCity" mode="region" :params="cityParams" @confirm="confirm" :default-region="defaultRegion"></u-picker>
			
			<view class="space"></view>
			<view class="space"></view>
			<view class="space"></view>

			<!-- 温湿度 -->
			<view class="row-padding">
				<!-- 温度 -->
				<view >
					<!--提示字 -->
					<view class="content" >
						<text class="text">温度 | {{temDesc}}</text>
					</view>
					<view class="space"></view>
					<!--温度值 -->
					<view @click="navigateTo('/pages/tempCharts/tempCharts')">
						<text class="Val" >{{temperatureVal}}</text>  <text class="white-font">&#176;C</text>
					</view>
					
				</view>
					
					<!-- 湿度 -->
				<view class="content left-padding">
					<!--提示字 -->
					<view>
						<text class="text">湿度 | {{RHDesc}}</text>
					</view>
					<view class="space"></view>
					<!--湿度值 -->
					<view class="u-row" @click="navigateTo('/pages/RHCharts/RHCharts')">
						<text class="Val">{{RHVal}}</text>  <text class="white-font">%</text>
					</view>
				</view>
			
			</view>
			
			<view class="space"></view>
			<view class="space"></view>
			<view class="space"></view>
			
			
			<!-- 空气质量 -->
			<view class="content">
				<view class="row-bet-padding white-font">
					<!-- 空气质量 -->
					<view class="right">
						<view class="small-left-padding">
							<u-image :src="qualityImage" width="50rpx" height="50rpx" ></u-image>
						</view>
						<text class="small-left-padding">{{quality}}</text>  
						<text class="small-left-padding">{{aqi}}</text>
					</view>
					
				</view>
			</view>

			<!-- 三天天气预览 -->
			<view class="three-day-weather">
				<view class="item" v-for="(item,index) in tdwList">
					<view class="left">
						<view class="img"><u-image :src="item.url" width="70rpx" height="63rpx"></u-image></view>
						<view class="desc">{{item.desc}}</view>						
					</view>

					<view class="right">
						{{item.value}}
					</view>
				</view>
			</view>

			<view class="space"></view>
			<!-- 24小时天气 -->
			<view >
				<scroll-view class="scroll-view_H" scroll-x="true" @scroll="scroll">
					<view v-for="(item,index) in tfhourList" class="scroll-view-item_H">
						<view>
							<view class="top">{{item.time}}</view>
							<view>{{item.temp}}</view>
							<image :src="item.image" class="img-style"></image>
						</view>
					</view>
				</scroll-view>
			</view>
			
			
			<!-- 杂项 -->
			<u-line color="#ffffff" length="90%" margin="50rpx 50rpx"> </u-line>
			<view class="content">
				<view class="other-box">
					<view class="sun-rise-and-sun-set">
						<view class="sun-rise">
							<u-image src="../../static/img/日出.png" width="45rpx" ></u-image>
							<text class="text white-font">日出 {{sunRiseTime}}</text>
						</view>
						
						<view class="sun-set">
							<u-image src="@/static/img/日落.png" width="45rpx" ></u-image>
							<text class="text white-font">日落 {{sunSetTime}}</text>
						</view>
						
						
					</view>
					
					<view class="other-data">
						<!-- 风 -->
						<view class="up-down">
							<!-- 风级 -->
							<view class="up white-font">
								{{windpower}}
							</view>
							<!-- 风向 -->
							<view class="down white-font">
								{{winddirect}}
							</view>
							
						</view>
						
						<!-- pm2.5 -->
						<view class="up-down">
							<view class="up white-font">
								{{pm2_5}}
							</view>
							<!-- 风向 -->
							<view class="down white-font">
								pm2.5
							</view>
						</view>
						
						<!-- 体感温度 -->
						<view class="up-down">
							<view class="up white-font">
								{{temperatureVal}}°
							</view>
							<!-- 风向 -->
							<view class="down white-font">
								体感
							</view>
						</view>
						
						<!-- 气压 -->
						<view class="up-down">
							<view class="up white-font">
								{{pressure}}hPa
							</view>
							<!-- 风向 -->
							<view class="down- white-font">
								气压
							</view>
						</view>	
							
					</view>
				</view>
			</view>
			
			
			
			<!-- 九宫格提示  -->
			<view class="space"></view>
			<view class="space"></view>
			<view class="content">
				<view class="grid-view">
					<u-grid :col="3" :border="false" @click="gridClick">
						<u-grid-item bg-color="#55aaff" :index="0">
							<view class="up-down" >
								<view class="up">
									<u-image :src="gridList[0]" width="130rpx" height="130rpx"></u-image>								
								</view>
								<text class="down white-font">空调指数</text>							
							</view>
						</u-grid-item>
						<u-grid-item bg-color="#55aaff" :index="1">
							<view class="up-down" >
								<view class="up">
									<u-image :src="gridList[1]" width="130rpx" height="130rpx"></u-image>
								</view>
								<text class="down white-font">运动指数</text>
							</view>
						</u-grid-item>
						<u-grid-item bg-color="#55aaff" :index="2">
							<view class="up-down" >
								<view class="up">
									<u-image :src="gridList[2]" width="130rpx" height="130rpx"></u-image>
								</view>
								<text class="down white-font">紫外线指数</text>
							</view>
						</u-grid-item>
						<u-grid-item bg-color="#55aaff" :index="3">
							<view class="up-down" >
								<view class="up">
									<u-image :src="gridList[3]" width="130rpx" height="130rpx"></u-image>
								</view>
								<text class="down white-font">感冒指数</text>
							</view>
						</u-grid-item>
						<u-grid-item bg-color="#55aaff" :index="4">
							<view class="up-down" >
								<view class="up">
									<u-image :src="gridList[4]" width="130rpx" height="130rpx"></u-image>
								</view>
								<text class="down white-font">洗车指数</text>
							</view>
						</u-grid-item>
						<u-grid-item bg-color="#55aaff" :index="5">
							<view class="up-down" >
								<view class="up">
									<u-image :src="gridList[5]" width="130rpx" height="130rpx"></u-image>
								</view>
								<text class="down white-font">穿衣指数</text>
							</view>
						</u-grid-item>
					</u-grid>
				</view>
			</view>
			
			
			<!--九宫格弹出层  -->
			<u-popup mode="bottom" v-model="show" border-radius="50" height="40%" >

				<view class="grid-detail white-font">
					<!-- 关于九宫格那个指数的问题答案 -->
					<view class="up-part">
						<view class="text">
							<view class="up">
								<text>{{gridText.left}}</text>
								<text class="right">{{gridText.right}}</text>
							</view>
							
							<view class="center">
								{{gridText.center}}
							</view>
							
							<view class="bottom">
								{{gridText.desc}}
							</view>
						</view>
					</view>
					<!-- 三天的天气温度预报 -->
					<view class="three-day-tem">
						<view class="column">
							<view class="day">今天</view>
							<view class="tem">{{gridText.day1tem}}</view>
						</view>
						<view class="column">
							<view class="day">明天</view>
							<view class="tem">{{gridText.day2tem}}</view>
						</view>
						<view class="column">
							<view class="day">后天</view>
							<view class="tem">{{gridText.day3tem}}</view>
						</view>

					</view>
				</view>
				
			</u-popup>
		</view>
	</view>
	
	
	
</template>

<script>
	export default {
		data() {
			return {
				//顶栏背景
				background:{
					backgroundColor:"#55aaff"
				},
				//返回json数据
				res:{},
				
				tdwList:[{},{},{}],
				
				//是否显示城市选择层
				showPickCity:false,
				
				//令城市只选择省和市
				cityParams:{
					province: true,
					city: true,
					area: false,
				},
				
				// 温度值
				temperatureVal:"",
				// 湿度值
				RHVal:"",

				// 温度描述
				temDesc:"",
				// 湿度描述
				RHDesc:"",

				city:"北京市",
				defaultRegion:[0,0],
				
				//空气质量
				quality:"",
				aqi:"",
				qualityImage:"",

				
				//24小时json
				tfhourList:[],
				
				//日出日落时间
				sunRiseTime:"",
				sunSetTime:"",
				
				
				//杂项
				winddirect:"",
				windpower:"",
				pressure:"",
				pm2_5:"",
				
				//九宫格图图地址列表
				gridList:["../../static/img/空调.png",
					"../../static/img/运动.png",
					"../../static/img/伞.png",
					"../../static/img/胶囊.png",
					"../../static/img/车辆.png",
					"../../static/img/衣服.png"],
				show:false,
				//九宫格弹出层内容
				gridText:{
					left:"",
					right:"",
					center:"",
					desc:"",
					day1tem:"",
					day2tem:"",
					day3tem:""
				},
				
				scrollTop: 0,
				old: {
					scrollTop: 0
				}
				
			}
		},
		
		// 页面加载之后执行
		async onLoad() { 			


			//获取网络json数据
			await this.getWebJson();
			
			//判断当前温湿度的描述
			this.determineVal();
				
			},
		
		methods: {
			
			//页面跳转
			navigateTo(url){
				uni.navigateTo({
					url:url
				})
			},
			
			gridClick(index){
				if(index==1){
					this.gridText.left="空气" + this.res.aqi.quality;
					this.gridText.right=this.res.aqi.aqi;
					this.gridText.center = this.res.index[index].ivalue;
					this.gridText.desc = this.res.index[index].detail;
				}
				else if(index==5){
					this.gridText.left=this.res.daily[0].day.weather;
					this.gridText.right = this.gridText.day1tem;
					this.gridText.center = this.res.index[index+1].ivalue;
					this.gridText.desc = this.res.index[index+1].detail;
				}
				else{
					this.gridText.left=this.res.daily[0].day.weather;
					this.gridText.right = this.gridText.day1tem;
					this.gridText.center = this.res.index[index].ivalue;
					this.gridText.desc = this.res.index[index].detail;
				}
				this.show=true;
			},
			
			//选择地区之后的回调函数，将选择的城市剥离出来
			async confirm(e) {
				//设置下次弹出地区选择框时的默认选项
				this.defaultRegion[0]=e.province.label;
				this.defaultRegion[1]=e.city.label;
				if(e.province.label=="北京市"||e.province.label=="天津市"||e.province.label=="重庆市"||e.province.label=="上海市"||e.province.label=="香港"||e.province.label=="澳门"){
					this.city = e.province.label;
					
					//切换天气后重新请求天气
					await this.getWebJson();	
					//判断当前温湿度的描述
					this.determineVal();
				}
				else{
					this.city = e.city.label;
					//切换天气后重新请求天气
					await this.getWebJson();
					//判断当前温湿度的描述
					this.determineVal();
				}
				
			},
			
			scroll: function(e) {
				console.log(e)
				this.old.scrollTop = e.detail.scrollTop
			},
			
			getWebJson(){
				return new Promise((resolve, reject) => {
					uni.request({
						url: "https://api.jisuapi.com/weather/query",//这个是你要请求的地址
						data: {
							appkey:"2240709f50c9dad4",
							city:this.city,
							//这个data是指，你看你从网络地址请求数据，你总得打个招呼吧，所以要给它一些信息，但是如果对方不要，我们就不给，这个情况我们也不需要给
						},
						timeout:120000,
						header: {
							//请求头信息，可不填（如果 没特殊要求就不填）
						},
						success: (res) => {//这句话就是，如果请求成功了，那么返回res，下面是请求成功之后的操作
							console.log(res)
							var res = res.data.result;
							//温湿度
							this.res = res;
							//存缓存
							try {
							    uni.setStorageSync('res', this.res);
							} catch (e) {
							    console.log("存缓存出错")
							}
							
							this.temperatureVal = res.temp;
							this.RHVal = res.humidity;
							
							//空气质量
							this.aqi = res.aqi.aqi
							if(res.aqi.quality=="优"||res.aqi.quality=="良"){
								this.quality = "空气"+res.aqi.quality;
								this.qualityImage="../../static/img/空气质量.png";
							}
							else{
								this.quality = res.aqi.quality;
								this.qualityImage="../../static/img/雾霾.png";
							}
							
							//3天天气预报
							for(var i=0;i<3;i++){
								this.tdwList[i].url = "../../static/img/"+res.daily[i].day.weather+".png"
								this.tdwList[i].value = res.daily[i].day.temphigh+"°/"+res.daily[i].night.templow+"°"
							}
							this.tdwList[0].desc = "今天  " + res.daily[0].day.weather;
							this.tdwList[1].desc = "明天  " + res.daily[1].day.weather;
							this.tdwList[2].desc = this.transWeek(res.daily[2].week) +"  "+ res.daily[2].day.weather;
							console.log(this.tdwList);
							
							//修改24小时天气列表的内容
							this.tfhourList = res.hourly;
							for(let item of this.tfhourList){
								item.image = "../../static/img/" + item.weather +".png";
								item.temp = item.temp+"°"
								//0：00的时候，把时间改成日期
								if(item.time == "0:00")
								{
									item.time = this.transDate(this.res.daily[1].date);
								}
							}
							
							//杂项
							this.sunRiseTime = res.daily[0].sunrise;
							this.sunSetTime = res.daily[0].sunset;
							this.winddirect = res.winddirect;
							this.windpower = res.windpower;
							this.pressure = res.pressure;
							this.pm2_5 = res.aqi.ipm2_5;
							
							//九宫格弹出框里公用的
							this.gridText.day1tem = res.daily[0].day.temphigh+"°/"+res.daily[0].night.templow+"°";
							this.gridText.day2tem = res.daily[1].day.temphigh+"°/"+res.daily[1].night.templow+"°";
							this.gridText.day3tem = res.daily[2].day.temphigh+"°/"+res.daily[2].night.templow+"°";
							resolve('suc');

						},
						fail: (res) => {
							console.log(res);
							reject('err');
						}
					});
				})


				
			},
			
			//处理2020-11-20这样的日期，将其转变为11月20日
			transDate(date){
				var list = date.split("-");
				var result = list[1]+"月"+list[2]+"日";
				return result;
			},
			
			//把星期几改成周几
			transWeek(str){
				var list = str.split("");
				return "周"+list[2];
			},
			
			// 判断
			determineVal(){
				//温度，字符串不能和数值直接比较大小，故用parseFloat将字符串转换为浮点型数据输出
				if(parseInt(this.temperatureVal)>25)
				{
					this.temDesc="偏热";
				}
				else if(parseInt(this.temperatureVal)<=25 && parseInt(this.temperatureVal)>15)
				{
					this.temDesc="适宜";
				}
				else if(parseInt(this.temperatureVal)<=15){
					this.temDesc="偏冷";
				}
				
				//湿度
				if(parseInt(this.RHVal)>70)
				{
					this.RHDesc="潮湿";
				}
				else if(parseInt(this.RHVal)<=70 && parseInt(this.RHVal)>40)
				{
					this.RHDesc="适宜";
				}
				else if(parseInt(this.RHVal)<=40){
					this.RHDesc="干燥";
				}				
			},
		},
	}

</script>


<style lang="scss">
	page{
		background-color: #55aaff;
		// font-family: "alibaba";
		
	}
	//todo:引入字体
	// @font-face{
	// 	font-family: 'alibaba';
	// 	src:url('@/static/Alibaba-PuHuiT.ttf');
	// }
	.navbar-right {
		margin-right: 24rpx;
		display: flex;
	}
	
	.scroll-view_H {
		padding-top: 35rpx;
		white-space: nowrap;
		width: 90%;
		padding-left: 5%;
	}
	
	.scroll-view-item_H {
		color: white;
		display: inline-block;
		width: 25%;
		height: 80rpx;
		line-height: 70rpx;
		text-align: center;
		font-size: 36rpx;
		.top{
			color:#E4E7ED;
		}
		.img-style{
			width: 70rpx;
			height:70rpx;
		}
	}
	
	.three-day-weather{
		padding-left: 5%;
		padding-right: 5%;
		color: #ffffff;
		font-size: 36rpx;
		.item{
			padding-top: 30rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			.left{
				display: flex;
				align-items: center;
				.img{
					
				}
				.desc{
					padding-left: 15rpx;
				}
			}
			
			.right{
				
			}
		}
	}
	
	.right-item {
		margin: 0 12rpx;
		position: relative;
		color: #ffffff;
		display: flex;
	}
	
	.white-font{
		color: #FFFFFF;
	}
	
	.Val{
		font-weight: 600;
		color:#FFFFFF;
		font-family:Arial, Helvetica, sans-serif;
		font-size: 100rpx;
	}
	
	.text{
		color: #ffffff;
	}
	
	.space{
		height: 60rpx;
	}
	
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	
	.row-padding{
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	
	
	
	.row-bet-padding{
		display: flex;
		width: 90%;
		justify-content: space-between;
		.left{
			display: flex;
			align-items: center;
			.small-left-padding{
				float: left;
				padding-left: 20rpx;
			}			
		}
		
		.right{
			display: flex;
			align-items: center;
			.small-left-padding{
				float: left;
				padding-left: 20rpx;
			}	
		}
	}
	
	.other-box{
		width: 90%;
		
		.sun-rise-and-sun-set{
			display: flex;
			align-items:flex-start;
			.sun-rise{
				display: flex;
				.text{
					padding-left: 10rpx;
				}
			}
			.sun-set{
				padding-left:80rpx;
				display: flex;
				.text{
					padding-left: 10rpx;
				}
			}
		}
		
		.other-data{
			justify-content: space-between;
			display: flex;
			padding-top: 40rpx;
			
			.up-down{
				display: flex;
				flex-direction: column;
				justify-content: center;
				.up{
					font-size:50rpx;
					font-weight: 500;
				}
				.down{
					padding-top: 10rpx;
				}
			}
			
		}
	}
	
	.grid-view{
		width: 90%;
		.up-down{
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			padding-top: 20rpx;
			.up{
				padding-bottom: 30rpx;
			}
			.down{
				
			}
		}
	}
	
	.grid-detail{
		background-color:#55aaff ;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100%;
		.text{
			padding-left: 5%;
			padding-right: 5%;
			float: left;
			.up{
				color: #E4E7ED;
				.right{
					padding-left: 10rpx;
				}
				
			}
			.center{
				padding-top: 20rpx;
				font-size: 50rpx;
			}
			.bottom{
				padding-top: 10rpx;
			}
		}

		.three-day-tem{
			display: flex;
			width: 90%;
			justify-content: space-between;
			.column{
				display: flex;
				flex-direction: column;
				align-items: center;
				justify-content: center;
				.day{
					padding-top: 180rpx;
				}
				.tem{
					color:#E4E7ED ;
					padding-top: 25rpx;
					padding-bottom: 0rpx;
				}
			}
		}
	}
	
	.left-padding{
		padding-left: 90rpx;
	}
	

	
	.column-padding{
		margin-top: 0rpx;
		display:flex;
		flex-direction:column;
		
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 50rpx;
		color: #8f8f94;
	}
	
</style>
