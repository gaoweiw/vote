<template>
	<view class="content-box">
		<view class="mask-box">
			<view class="charts-box">
				<qiun-data-charts type="column" :opts="opts" :chartData="chartData" />
			</view>
			<uni-section title="保费王" class="result-box" type="line">
				<view class="table-box">
					<view class="title">
						<ul>
							<li>区域</li>
							<li>姓名</li>
							<li>保费</li>
						</ul>
					</view>
					<view class="text" v-for="item in tableList">
						<div>{{item.name}}区：</div>
						<div>{{item.userName}}</div>
						<div>{{item.premium}}</div>
					</view>
				</view>
			</uni-section>
		</view>
	</view>
</template>

<script>
	import baseUrl from '@/util/baseUrl';
	export default {
		data() {
			return {
				chartData: {},
				opts: {},
				tableList: [],
				arrayList: ['大一', '大二', '石一', '石二', '江北', '沙区', '金阳', '万达', '奥体', '嘉州'],
			}
		},
		onShow() {
			this.opts = {
				color: ["#FB243A", "#24F8F2", "#3EAEFF"],
				padding: [15, 15, 0, 5],
				height: 300,
				legend: {
					position: 'top',
					fontColor: "#FFFFFF",
					float: 'right',
					margin: 10,
				},
				xAxis: {
					fontColor: "#FFFFFF",
					disableGrid: true,
				},
				yAxis: {
					disabled: true,
					disableGrid: true, //不绘制横向网格
					data: [{
						fontColor: "#FFFFFF",
						min: 0
					}]
				},
				extra: {
					column: {
						width: 6,
						activeBgColor: "#000000",
						activeBgOpacity: 0.08,
						seriesGap: 0.5
					}
				}
			}
		},
		onReady() {
			this.getServerData();
		},
		methods: {
			getServerData() {
				uni.request({
					url: `${baseUrl}/auth/target/list`,
					success: (response) => {
						//表格数据
						this.tableList = response.data.data.list2;

						//图表数据

						let chartData = response.data.data.list;
						let premiumMax = []; //总保费
						let numberMax = []; //总件数
						let quotaMax = []; //总旅游名额
						console.log(chartData)

						this.arrayList.forEach(item => {
							let d = chartData.filter(s => item == s.name);
							console.log(d)
							if (d.length > 0) {

								// 总保费：以万单位
								// 超过万以上四舍五入
								// 万以下用0.几万表示，不四舍五入
								premiumMax.push(d[0].premium);
								numberMax.push(d[0].number);
								quotaMax.push(d[0].quota);
							} else {
								premiumMax.push(0);
								numberMax.push(0);
								quotaMax.push(0);
							}
						})

						console.log(premiumMax, numberMax, quotaMax)


						let res = {
							categories: this.arrayList,
							series: [{
									name: '总保费',
									textColor: '#FFFFFF',
									format: 'targetFormat',
									textSize: 10,
									data: premiumMax
								},
								{
									name: '总件数',
									textColor: '#FFFFFF',
									textSize: 10,
									data: numberMax
								},
								{
									name: '总旅游名额',
									textColor: '#FFFFFF',
									textSize: 10,
									data: quotaMax
								}
							]
						};
						this.chartData = JSON.parse(JSON.stringify(res));

						console.log(this.formatNumber(1))
						console.log(this.formatNumber(15999))
					},
				})
			},
			formatNumber(num) {
				// 总保费：以万单位
				// 超过万以上四舍五入
				// 万以下用0.几万表示，不四舍五入
				let n = Number(num);
				let result = '';
				if (n > 10000) {
					let w = (n / 10000).toString();
					result = w.substring(0, w.indexOf(".") + 2);
				} else {
					let w = (n / 10000).toString();
					result = w.substring(0, w.indexOf(".") + 2);
				}
				return result;
			}
		},
	}
</script>

<style lang="scss" scoped>
	.content-box {
		background: url('@/static/target/detail.png') no-repeat;
		background-size: cover;

		.mask-box {
			width: 100%;
			height: 100%;
			background: rgba(0, 0, 0, 0.4);
		}

		.charts-box {
			padding: 14px;
			box-sizing: border-box;
			min-height: 278px;
		}

		.result-box {
			padding: 14px;
			box-sizing: border-box;
			background: none;

			.table-box {
				padding: 16px 22px;
				box-sizing: border-box;
				background: rgba(255, 255, 255, 0.8);
				border-radius: 10px;

				.title {
					ul {
						padding: 0px;
						width: 100%;
						display: flex;
						margin-bottom: 16px;
					}

					ul li {
						font-family: PingFang SC;
						font-size: 14px;
						list-style: none;
						flex: 1;

					}
				}

				.text {
					display: flex;

					div {
						flex: 1;
						height: 42px;
						line-height: 42px;
						font-size: 14px;
						border-top: 1px solid rgb(211, 211, 211);
					}
				}
			}
		}
	}

	:deep(.uni-section__content-title) {
		color: #FFFFFF !important;
		font-size: 16px;
	}

	:deep(.uni-section .uni-section-header__decoration) {
		background-color: #FF1E1E !important;
	}
</style>
