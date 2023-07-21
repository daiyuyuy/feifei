<template>
	<view>
		<view class="nav-bar">
			<!-- <uni-nav-bar title="嘿嘿嘿">
				 <image src="/static/my1.png"></image> 
			</uni-nav-bar> -->

		</view>
		<view class="content">
			<view>{{ messageList }}</view>
		</view>
	</view>
</template>

<script>
// import { uni_request } from '@/utils/index.js';
export default {
	data() {
		return {
			messageList: [], // 消息列表
			inputValue: '' // 用户输入的消息内容
		}
	},
	onLoad() {
		this.connect()
	},
	onUnload() {
		uni.closeSocket()
	},
	onSocketMessage(res) {
		const message = JSON.parse(res.data);

		// 将消息添加到消息列表
		this.messageList.push(message);
	},
	methods: {
		// 连接WebSocket
		connect() {
			uni.connectSocket({
				url: 'wss://your-websocket-url', // WebSocket的地址
			});

			uni.onSocketOpen(function () {
				console.log('WebSocket连接已打开!');
			});

			uni.onSocketError(function (res) {
				console.log('WebSocket连接打开失败,请检查网络!');
			});
		},
		// 发送消息
		sendMessage() {
			const message = {
				content: this.inputValue, // 消息内容
				time: new Date().getTime() // 发送时间
			};

			// 将消息添加到消息列表
			this.messageList.push(message);

			// 清空输入框内容
			this.inputValue = '';

			// 发送消息给服务器
			uni.sendSocketMessage({
				data: JSON.stringify(message)
			});
		}
	},
}
</script>

<style></style>
