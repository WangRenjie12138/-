<template>
	<view>
		<u-navbar back-icon-color="#ffffff" :is-back="true" 
		back-text="返回" title="湿度折线图" :back-text-style="backTextStyle"
		:background="background" title-color="#ffffff" title-size="42">
		</u-navbar>
		
		<view>
			<view class="qiun-columns">
				<view class="qiun-bg-white qiun-title-bar qiun-common-mt" >
					<view class="qiun-title-dot-light white-font">24小时湿度变化图</view>
					<!-- <view class="space"></view> -->
				</view>
				<view class="qiun-charts" >
					<canvas canvas-id="canvasLineA" id="canvasLineA" class="charts" @touchstart="touchLineA"></canvas>
				</view>
			</view>
		</view>
		
		<view>
			<view class="qiun-columns">
				<view class="qiun-bg-white qiun-title-bar qiun-common-mt" >
					<view class="qiun-title-dot-light white-font">7天平均湿度变化图</view>
					<!-- <view class="space"></view> -->
				</view>
				<view class="qiun-charts" >
					<canvas canvas-id="canvasLineB" id="canvasLineB" class="charts" @touchstart="touchLineB"></canvas>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	import uCharts from '../../components/u-charts/u-charts.js';
	var _self;
	var canvaLineA=null;
	var canvaLineB=null;
	export default {
		data() {
			return {
				background:{
					backgroundColor:"#55aaff"
				},
				backTextStyle:{color:"#ffffff"},
				
				// 定义宽度
				cWidth:'',
				// 定义高度
				cHeight:'',
				// 分辨率
				pixelRatio:1,
				
				//源码
				res:{},
				RH:0,
				
				RHTimeList:[],
				RHList:[],
				
				RHDayList:[],
				RHDList:[],
			}
		},
		onLoad() {
			//把res缓存取出来
			try {
			    const res = uni.getStorageSync('res');
				console.log("缓存取出来啦！")
				this.res = res
			} catch (e) {
			    console.log("取缓存出错啦！");
			}
			// 为_this赋值，定义图表的宽度和高度
			_self = this;
			this.cWidth=uni.upx2px(750);
			this.cHeight=uni.upx2px(500);
			
			//把画图的数据搞出来
			this.getDataFromJson()
			
			//画一手图
			this.getRHData();
			
			//再画一手
			this.getRHDayData();
			
			
		},
		methods: {
			
			getDataFromJson(){
				this.RH= parseInt(this.res.humidity)
				for(var i=0;i<24;i++){
					this.RHList[i] = Math.ceil(this.RH-5+10*Math.random())
					this.RHTimeList[i] = parseFloat(this.res.hourly[i].time)
				}
				for(var i=0;i<7;i++){
					this.RHDayList[i] = Math.ceil(this.RH-5+10*Math.random())
					if(this.res.daily[i].week.split("")[2]=='一'){
						this.RHDList[i]=1
					}
					else if(this.res.daily[i].week.split("")[2]=='二'){
						this.RHDList[i]=2
					}
					else if(this.res.daily[i].week.split("")[2]=='三'){
						this.RHDList[i]=3
					}
					else if(this.res.daily[i].week.split("")[2]=='四'){
						this.RHDList[i]=4
					}
					else if(this.res.daily[i].week.split("")[2]=='五'){
						this.RHDList[i]=5
					}
					else if(this.res.daily[i].week.split("")[2]=='六'){
						this.RHDList[i]=6
					}
					else if(this.res.daily[i].week.split("")[2]=='日'){
						this.RHDList[i]=7
					}
				}
			},
			
			
			
			getRHData(){
				let LineA ={
					categories:this.RHTimeList,
					series:[{
						data:this.RHList,
						name:"湿度"
					}]
				}
				_self.showLineA("canvasLineA",LineA);
			},
			
			getRHDayData(){
				let LineB ={
					categories:this.RHDList,
					series:[{
						data:this.RHDayList,
						name:"湿度"
					}]
				}
				_self.showLineB("canvasLineB",LineB);
			},
			
			showLineA(canvasId,chartData){
				canvaLineA=new uCharts({
					$this:_self,
					canvasId: canvasId,
					type: 'line',
					fontSize:11,
					legend:{show:true},
					dataLabel:false,
					dataPointShape:true,
					background:'#FFFFFF',
					pixelRatio:_self.pixelRatio,
					categories: chartData.categories,
					series: chartData.series,
					animation: true,
					xAxis: {
						type:'grid',
						gridColor:'#ffffff',
						gridType:'dash',
						dashLength:24
					},
					yAxis: {
						gridType:'dash',
						gridColor:'#ffffff',
						dashLength:8,
						splitNumber:6,
						min:0,
						max:100,
						format:(val)=>{return val.toFixed(0)+'%'}
					},
					width: _self.cWidth*_self.pixelRatio,
					height: _self.cHeight*_self.pixelRatio,
					extra: {
						line:{
							type: 'straight'
						}
					}
				});				
			},
			
			showLineB(canvasId,chartData){
				canvaLineB=new uCharts({
					$this:_self,
					canvasId: canvasId,
					type: 'line',
					fontSize:11,
					legend:{show:true},
					dataLabel:false,
					dataPointShape:true,
					background:'#FFFFFF',
					pixelRatio:_self.pixelRatio,
					categories: chartData.categories,
					series: chartData.series,
					animation: true,
					xAxis: {
						type:'grid',
						gridColor:'#ffffff',
						gridType:'dash',
						dashLength:24
					},
					yAxis: {
						gridType:'dash',
						gridColor:'#ffffff',
						dashLength:8,
						splitNumber:6,
						min:0,
						max:100,
						format:(val)=>{return val.toFixed(0)+'%'}
					},
					width: _self.cWidth*_self.pixelRatio,
					height: _self.cHeight*_self.pixelRatio,
					extra: {
						line:{
							type: 'straight'
						}
					}
				});				
			},
			// 触碰折线图上的点获得数据
			touchLineA(e) {
				canvaLineA.showToolTip(e, {
					format: function (item, category) {
						return category + ' ' + item.name + ':' + item.data +"%"
					}
				});
			},
			
			touchLineB(e) {
				canvaLineB.showToolTip(e, {
					format: function (item, category) {
						return category + ' ' + item.name + ':' + item.data +"%"
					}
				});
			},
		}
	}
</script>

<style lang="scss">
	page{
		background-color: #55aaff;
		// font-family: "alibaba";
		
	}
	.white-font{
		color: #ffffff;
	}
	
	.qiun-charts {
		width: 750upx;
		height: 500upx;
		background-color: #FFFFFF;
	}
	
	.charts {
		width: 750upx;
		height: 500upx;
		background-color: #FFFFFF;
	}
</style>
