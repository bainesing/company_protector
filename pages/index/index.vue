<template>
	<view>

		<!-- banner -->
		<swiper class="index_banner">
			<block v-for="(item, index) in bannerList" :key="index">
				<swiper-item>
					<image class="banner_item" :src="item.url"></image>
				</swiper-item>
			</block>
		</swiper>


		<!-- 根据用户类别展示不同的内容 -->
		<!-- 专家用户 开始 -->
		<block v-if="user_info.type==='professor'">



		</block>
		<!-- 专家用户 结束 -->

		<!-- 企业用户 开始 -->
		<block v-if="user_info.type==='company'">
			<view class="notify_title">
				<image class="icon" src="../../static/icon/notify_green.png"></image>
				<view class="text">您有新的安全隐患待整改</view>
			</view>

			<view class="message_list">
				<block v-for="(item, index) in messageList" :key="index">
					<view class="item">
						<!-- 缩略图用背景展示 -->
						<view class="picture" style="background-image: linear-gradient(to bottom, #888888,#888888)">
							<block v-if="item.status === 0">
								<view class="cover danger">
									<image class="icon" src="../../static/icon/danger_icon.png"></image>
								</view>
							</block>
							<block v-else>
								<view class="cover success">
									<image class="icon" src="../../static/icon/success_icon.png"></image>
								</view>
							</block>
						</view>
						<view class="title">{{item.title}}</view>
						<view v-show="item.status === 0" class="button">提交整改</view>
					</view>
				</block>
			</view>

		</block>
		<!-- 企业用户 结束 -->



	</view>
</template>

<script>
	export default {
		data() {
			return {
				bannerList: [
					{name:"最新热点新闻",url:"../../static/banner/banner1.jpg"}
				],
				//用户信息。这里的用户信息应当通过登录时从服务器获取。数据只是测试用，具体字段根据接口返回而定。
				user_info: {
					user_name: '测试用户',
					type: 'company'
				},

				//整改列表。如果是专家用户，这里存储待审核列表
				messageList: [
					{title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
					{title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
					{title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
					{title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
					{title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
					{title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
					{title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
					{title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
					{title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 1},
					{title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
				]
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	page{
		background-color: #f8f8f8;
	}

	/*banner*/
	.index_banner{
		width: 100%;
		height: 420upx;
	}
	.index_banner .banner_item{
		width: 730upx;
		height: 400upx;
		display: block;
		margin: 10px auto;
	}

	/*通知列表*/
	.notify_title{
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		margin: 100upx auto 50upx auto;
	}

	.notify_title .icon{
		width: 48upx;
		height: 48upx;
		display: block;
		margin-right: 20upx;
	}

	.notify_title .text{
		font-size: 36upx;
		color: #2ac080;
		font-weight: bold;
		line-height: 100%;
	}

	.message_list{
		width: 100%;
	}

	.message_list .item{
		width: 690upx;
		box-sizing: border-box;
		padding: 30upx;
		display: flex;
		align-items: center;
		justify-content: space-between;
		background-color: #ffffff;
		margin: 20upx auto;
		box-shadow: 0 0 20upx 0 rgba(0,0,0,0.1);
		border-radius: 20upx;
	}

	.message_list .item .picture{
		width: 80upx;
		height: 80upx;
		background-size: cover;
		background-position: center center;
		background-repeat: no-repeat;
		margin-right: 20upx;
		border-radius: 10upx;
		overflow: hidden;
	}

	.message_list .item .picture .cover{
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.message_list .item .picture .cover.danger{
		background-color: rgba(255,0,0,0.1);
	}

	.message_list .item .picture .cover.success{
		background-color: rgba(0,255,0,0.1);
	}

	.message_list .item .picture .cover .icon{
		width: 40upx;
		height: 40upx;
		display: block;
	}

	.message_list .item .title{
		width: 50%;
		flex: auto;
		font-size: 25upx;
		color: #888888;
	}

	.message_list .item .button{
		width: fit-content;
		height: fit-content;
		display: block;
		padding: 15upx;
		background-color: #23b574;
		color: #ffffff;
		font-size: 25upx;
		line-height: 100%;
		border-radius: 10upx;
	}

	.message_list .item .button:active{
		opacity: .7;
	}

</style>
