<template>
	<view class="content-box" :style="{'background':bgk,'background-size':'cover'}">
		<uni-segmented-control :current="current" :values="items" @clickItem="onClickItem" styleType="text"
			activeColor="#FF1E1E"></uni-segmented-control>
		<view class="content">
			<view v-show="current === 0">
				<view class="content-text">
					<view class="title">
						老北京驴打滚
					</view>
					<view class="desc">
						驴打滚是老北京和天津卫传统小吃之一，又叫豆面糕，起源于东北地区，在北京称驴打滚，北京小吃中的古老品种之一。成品黄、白、红三色分明，煞是好看。因其最后制作工序中撒上的黄豆面，犹如老北京郊外野驴撒欢打滚时扬起的阵阵黄土，因此而得名“驴打滚”。做好的“驴打滚”外层粘满豆面，呈金黄色，豆香馅甜，入口绵软，别具风味，豆馅入口即化，香甜入心，黄豆面入嘴后可以不嚼，细细品，是老少皆宜的传统风味小吃。老北京驴打滚入选“中国地域十大名小吃”北京榜。
					</view>
					<view class="img">
						<img src="@/static/vote/bj1.png" mode="aspectFit" alt="bj">
					</view>
				</view>
			</view>
			<view v-show="current === 1">
				<view class="content-text">
					<view class="title">
						内蒙烤全羊
					</view>
					<view class="desc">
						烤全羊可以说是内蒙古的一种特色美食，特别适合多人一同食用，色、香、味、形俱全，烤出来的羊肉色泽金黄，外焦里嫩，皮脆肉滑，肥美鲜香，可以说是草原上的餐中“至尊”，特别适合拿来招待好朋友。烤全羊是羊肉类食物中的“王”，那一整只烤全羊经过火炉的烘烤后呈现在我们面前时，被烤的金黄的烤全羊外表色泽金黄，切一大快下来再蘸上孜然、香辣调料，吃一口那肉质鲜嫩的羊肉绝对会幸福感瞬间爆棚，那美味已经溢于言表了！
					</view>
					<view class="img">
						<img src="@/static/vote/nm1.png" mode="aspectFit" alt="bj">
					</view>
				</view>
			</view>
			<view v-show="current === 2">
				<view class="content-text">
					<view class="title">
						新疆手抓饭
					</view>
					<view class="desc">
						新疆手抓饭是新疆著名特色风味小吃，是新疆各族人民普遍喜爱的食物，维吾尔语称其为“坡罗”。做法是先用油炒洋葱、黄萝卜丝(用黄萝卜是因为它是新疆特产，别地黄萝卜产量少，所以其他地区只能用胡萝卜代替)、羊肉块，
						然后放入淘净的大米加水焖蒸。新疆本地的正宗抓饭一般是不放孜然粉、山楂、酱油的，而清真菜肴更是不能放含酒精的料酒的。不过根据所在地食材的品质和个人的口味可以酌量添加自己喜欢的配料。新疆手抓饭更是入选由中国烹饪协会主办“中国地域十大名小吃”新疆榜名单。
					</view>
					<view class="img">
						<img src="@/static/vote/xj1.png" mode="aspectFit" alt="bj">
					</view>
				</view>
			</view>
		</view>
		<view class="vote-result" @click="jumpResult">
			投票结果
		</view>
		<view type="default" class="button" @click="voteClick">投票</view>
		<uni-popup ref="popup" type="dialog">
			<view class="pop-wrapper">
				<view class="pop-success">
					投票成功
				</view>
				<view class="pop-close" @click="close">
					确定
				</view>
			</view>
		</uni-popup>

	</view>
</template>

<script>
	import bg0 from "@/static/vote/bj.png";
	import bg1 from "@/static/vote/nm.png";
	import bg2 from "@/static/vote/xj.png";
	import baseUrl from '@/util/baseUrl';
	export default {
		data() {
			return {
				items: ['北京', '内蒙', '新疆'],
				current: 0,
				bgk: `url(${bg0}) no-repeat`,
				voteNum: 0,
				isRun: true,
			};
		},
		methods: {
			onClickItem(e) {
				if (this.current != e.currentIndex) {
					this.current = e.currentIndex;
					if (e.currentIndex == 0) {
						this.bgk = `url(${bg0}) no-repeat`;
					} else if (e.currentIndex == 1) {
						this.bgk = `url(${bg1}) no-repeat`;
					} else if (e.currentIndex == 2) {
						this.bgk = `url(${bg2}) no-repeat`;
					}
				}
			},
			voteClick() {
				//每个人只能投票两次，结束时间10月12日
				let nowTime = Date.now();
				let endTime = (new Date('2022/10/12 23:59:59')).valueOf()
				console.log()
				if (nowTime > endTime) {
					uni.showToast({
						icon: 'error',
						title: ' 投票已经结束 ',
					})
					return;
				}

				this.voteNum = window.localStorage.getItem('voteNmuber');
				if (this.voteNum >= 2) {
					uni.showToast({
						icon: 'error',
						title: ' 每人只能投两次 ',
					})
				} else {
					this.voteRequest();

				}
			},
			voteRequest() {
				if (!this.isRun) return;
				this.isRun = false;
				uni.request({
					url: `${baseUrl}/auth/vote/save`,
					method: "POST",
					data: {
						cityId: this.current + 1,
						city: this.current + 1,
						cityName: this.current == 0 ? '北京' : this.current == 1 ? '内蒙' : this.current == 2 ? '新疆' :
							''
					},
					success: (res) => {
						this.$refs.popup.open();
						window.localStorage.setItem('voteNmuber', Number(this.voteNum) + 1); //投票次数
						this.isRun = true;
					}
				});
			},
			close() {
				this.$refs.popup.close()
			},
			jumpResult() {
				uni.navigateTo({
					url: '/pages/vote/detail',
					animationType: 'slide-in-right',
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content-box {
		position: relative;
		background-size: cover;
		padding-bottom: 66px;

		.content {
			margin: 14px;
			background: rgba(255, 255, 255, 0.8);
			border-radius: 10px;

			.content-text {
				padding: 15px 18px;
				box-sizing: border-box;

				.title {
					font-size: 16px;
					font-weight: 400;
					line-height: 24px;
					font-weight: bold;
				}

				.desc {
					font-size: 14px;
					line-height: 21px;
					text-indent: 33px;
					text-align: justify;
					justify-content: space-around;
				}

				.img {
					margin-top: 19px;

					img {
						width: 100%;
						border-radius: 8px;
					}
				}
			}
		}

		.vote-result {
			position: absolute;
			top: 10px;
			right: 0;
			width: 64px;
			height: 21px;
			line-height: 21px;
			background: rgb(255, 30, 30);
			color: #FFFFFF;
			text-align: center;
			font-size: 11px;
			border-radius: 20px 0px 0px 20px;
		}

		.pop-wrapper {
			width: calc(100vw - 56px);
			margin: 14px;
			box-sizing: border-box;
			background: #FFFFFF;
			border-radius: 10px;
			padding: 14px;
			text-align: center;

			.pop-success {
				font-size: 16px;
				line-height: 80px;
				margin-bottom: 20px;
			}

			.pop-close {
				color: #FFFFFF;
				border-radius: 50px;
				width: 100px;
				text-align: center;
				line-height: 32px;
				margin: 0 auto;
				background-color: #007aff;
			}
		}

		.button {
			background: #FF1E1E;
			color: #FFFFFF;
			width: 153px;
			height: 42px;
			text-align: center;
			line-height: 42px;
			border-radius: 50px;
			margin: 61px auto 0 auto;
		}
	}

	:deep(.segmented-control) {
		.segmented-control__text {
			font-family: PingFang SC;
			font-size: 16px;
			font-weight: 400;
		}
	}

	:deep(.segmented-control__item) {
		flex: 0 0 70px;
	}

	:deep(.segmented-control__text) {
		color: #FFFFFF !important;

		&.segmented-control__item--text {
			color: #FF1E1E !important;
			border-bottom-width: 4px;
		}
	}
</style>
