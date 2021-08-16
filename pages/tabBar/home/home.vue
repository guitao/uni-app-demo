<template>
	<view class="container">
		<homeHeader v-if='showHeader'></homeHeader>

		<!-- 分类导航栏 -->
		<scroll-view :scroll-into-view='selectTabId' scroll-x="true" class="tab-bar" scroll-with-animation='true'>
			<view @tap="onTabTap(index)" class="uni-tab" v-for="(tab,index) in tabList" :id="tab.id" :key="tab.id">
				<text :class="{'tab-cur' : tabIndex == index}" class="uni-tab-item">{{tab.name}}</text>
			</view>
		</scroll-view>
		
		<!-- 占位 -->
		<view class="place"></view>
		
		<!-- 内容 -->
		<swiper @change='changeSwiper' :current='tabIndex' class="tab-box" :duration="300">
			<swiper-item class="swiper-item" v-for="(page, index) in tabList" :key="page.id">
					{{index}}
			</swiper-item>
		</swiper>
		
		
	</view>
</template>

<script>
	import interfaces from '../../../utils/interfaces.js'
	import homeHeader from '../../../componments/home/homeHeader.vue'
	export default {
		components: {
			homeHeader
		},
		data() {
			return {
				showHeader: true,
				tabList: [],
				tabIndex: 0,
				selectTabId: ''
			}
		},
		onLoad() {
			// #ifdef APP-PLUS
			this.showHeader = false
			// #endif

			this.getTabsData()
		},
		methods: {
			onTabTap(index) {
				this.tabIndex = index
			},
			changeSwiper(val) {
				// console.log("=====changeSwiper======", val)
				this.tabIndex = val.detail.current || val.target.current
				this.selectTabId = this.tabList[this.tabIndex].id
			},
			getTabsData() {
				// uni.request({
				// 	url: interfaces.getTabList,
				// 	success: res => {
				// 		console.log(res)
				// 	}
				// })

				this.$request({
					url: interfaces.getTabList,
					success: res => {
						console.log(res)
						this.tabList = res.data
					}
				})
			}

		}
	}
</script>

<style lang="scss">
	// 分类样式
	.tab-bar {
		background-color: #f4f5f6;
		white-space: nowrap;
		position: fixed;
		z-index: 99;
		top: 100upx;
		// #ifdef APP-PLUS
		top: 0;
		// #endif

		.uni-tab {
			display: inline-block;
			white-space: nowrap;
			padding: 20upx 0 20upx 20upx;

			.uni-tab-item {
				padding: 0 20upx;
				height: 52upx;
				line-height: 52upx;
				color: #505050;
			}

			.tab-cur {
				color: #f85959;
			}
		}
	}

	.tab-bar ::-webkit-scrollbar {
		width: 0 !important;
		display: none;
		height: 0 !important;
		-webkit-appearance: none;
		background: transparent;
	}


	// 内容样式
	.tab-box {
		flex: 1;

		.swiper-item {
			display: flex;
			flex: 1;
			flex-direction: column;
			overflow: hidden;

			.panel-scroll-box {
				height: 100%;
			}
		}
	}

	// 占位
	.place {
		height: 100upx;
	}
</style>
