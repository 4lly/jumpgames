<template>
	<view class="container">
		<view class="options">
			<view class="jumpBtn" @click="play('reset')">
				重玩
			</view>
			<view class="jumpBtn" @click="play('start')">
				开始
			</view>
			<!-- <view class="jumpBtn" @click="play('stop')">
				暂停
			</view> -->
			<view class="jumpBtn" @touchstart="jump" @touchend="jumpdown">
				跳跃
			</view>
		</view>
		<image src="../../static/img/1.png" :class="[isover?'over':'',jumpH]" :style="'width:'+itemW+'px;left:'+itemW+'px;'"></image>
		<view class="box" :style="'width:'+boxW+'px;left:-'+left+'px'">
			<view class="box-item" :style="'width:'+itemW+'px;'" :class="[item.type?'type1':'' ]" v-for="(item,index) in list"
			 :key="index">
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				isover: false,
				timer: null,
				isjump: false,
				left: 0,
				itemW: 30,
				boxW: 0,
				step: 1,
				list: []
			}
		},
		onLoad() {
			this.init()
		},
		computed: {
			getType0() {
				let type0arr = []
				this.list.filter((v, index) => {
					if (v.type === 0) {
						type0arr.push(index * this.itemW)
					}
				})
			},
			jumpH() {
				if (this.isover) return ''
				return this.isjump ? 'jump' : 'jumpdown'
			}
		},
		methods: {
			play(type) {
				if (type === 'start') {
					if (this.timer) {
						clearInterval(this.timer)
					}
					this.timer = setInterval(() => {
						this.left += this.step
						// console.log('游戏信息=====', this.left)
						// 只要挪过去一个itemW就删除第一个，然后再往数组追加一个item
						if (this.left === this.itemW) {
							// 随机生成柱子或者坑
							let type = parseInt(Math.random() * (1 + 1), 10)
							this.list.splice(0, 1)
							this.left -= this.itemW
							this.list.push({
								type: type
							})
							// 计算box的宽度
							this.boxW = this.list.length * this.itemW
							// 判断是否落坑
							if (!this.isjump && this.list[1].type === 0) {
								// 判断障碍物范围
								// todo
								this.isover = true
								uni.showToast({
									title: '游戏结束',
									icon: 'none'
								});
								if (this.timer) {
									clearInterval(this.timer)
								}
							}
						}
					}, 50)
				}
				if (type === 'stop') {
					if (this.timer) {
						clearInterval(this.timer)
					}
				}
				if (type === 'reset') {
					this.init()
				}
			},
			init() {
				if (this.timer) {
					clearInterval(this.timer)
				}
				this.isover = false,
					this.timer = null,
					this.isjump = false,
					this.left = 0,
					this.itemW = 30,
					this.boxW = 0,
					this.step = 1,
					this.list = [{
							type: 1
						},
						{
							type: 1
						},
						{
							type: 1
						},
						{
							type: 0
						},
						{
							type: 1
						},
						{
							type: 1
						}, {
							type: 0
						},
						{
							type: 1
						},
						{
							type: 1
						},
						{
							type: 0
						},
						{
							type: 1
						},
						{
							type: 1
						},
						{
							type: 1
						},
						{
							type: 0
						}
					]
				this.boxW = this.list.length * this.itemW
			},
			jump() {
				console.log('跳起来了')
				// if (this.isjump) return
				this.isjump = true
			},
			jumpdown() {
				this.isjump = false
				console.log('落下来了')
			}
		}
	}
</script>

<style>
	body {
		margin: 0;
		padding: 0;
	
	}
.container{
	    height: 100%;
	    width: 100%;
	    overflow: hidden;
	    -webkit-box-sizing: border-box;
	    box-sizing: border-box;
}
	image {
		height: 50rpx;
		position: absolute;
		bottom: 200px;
	}

	image.over {
		bottom: 0 !important;
		transition: all 1s;
	}

	image.jump {
		bottom: 350px !important;
		transition: all 1s;
	}

	image.jumpdown {
		bottom: 200px !important;
		transition: all 1s;
	}

	.box {
		position: absolute;
		bottom: 0;
		left: 0;
		display: block;
		white-space: nowrap !important;
		height: 200px;
	}

	.box-item {
		float: left;
		height: 200px;
		box-sizing: border-box;
	}

	.box-item.type1 {
		background-color: #000;
	}

	.options {
		padding-top: 100rpx;
		display: flex;
		justify-content: space-around;
	}

	.options view {
		background-color: #000;
		min-width: 100rpx;
		height: 100rpx;
		color: #FFFFFF;
		line-height: 100rpx;
		text-align: center;
		padding: 0 20rpx;
		border-radius: 100rpx;
		-webkit-touch-callout: none;
		-webkit-user-select: none;
		-moz-user-select: none;
		-ms-user-select: none;
		user-select: none;
	}
</style>
