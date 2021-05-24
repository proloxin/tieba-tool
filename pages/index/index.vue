<template>
	<view class="content">
		<view class="common-page-head">
			<view class="common-page-head-title">tieba</view>
		</view>
		<view class="tarea">
			<textarea :value="mtext" @blur="bindTextAreaBlur" placeholder="在此输入/粘贴原始文本"/>
		</view>
		<button type="primary" class="btn" plain="" @click='doSalt'>加盐</button>
		<view class="tarea">
			<textarea :value="saltText" placeholder="美化后文本"/>
		</view>
		<view class="txt">
			<text>1.在上方输入框内输入文本\n2.点击“加盐”按钮\n3.在下方输入框内全选复制\n\ndone</text>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mtext: '',
				saltText: ''
			}
		},
		onLoad() {
			
			
		},
		methods: {
			bindTextAreaBlur: function (e) {
				 this.mtext = e.detail.value
			},
			doSalt(m){
				this.saltText = this.deUnicode(this.enUnicode(this.mtext))
				//console.log(this.saltText)
			},
			enUnicode(str) {
				let rs = "";
				for (let i = 0; i < str.length; i++) {
					//补零。不补有些库无法正常解析。保持4位
					//slice负数参数，与其方向相反。start=-1为最后一个元素，end=-1为第一个元素。start必须
					rs += "\\u" + ("0000" + str.charCodeAt(i).toString(16)).slice(-4);
				}
				return rs;
			},
			deUnicode(str) {
				let strArray = str.split("\\u");
				//防止\u开头或结尾，导致解析空串产生的“□”的结果
				if (str.startsWith("\\u")) {
					strArray = strArray.slice(1, strArray.length);
				}
				if (str.endsWith("\\u")) {
					strArray = strArray.slice(0, strArray.length - 1);
				}
				let rs = "";
				for (let i = 0; i < strArray.length; i++) {
					rs += String.fromCharCode(parseInt(strArray[i], 16))+'\u034f';
				}
				return rs;
			}

		}
	}
</script>

<style>
	.content {		
		height:100%;
		background-color: #efeff4;
	}
	.common-page-head {
		padding: 10rpx;
		text-align: center;
	}
	.common-page-head-title {
		display: inline-block;
		padding: 0 20rpx;
		font-size: 35rpx;
		height: 64rpx;
		line-height: 64rpx;
		color: #d8d8d8;
		-webkit-box-sizing: border-box;
		box-sizing: border-box;
		border-bottom: 1rpx solid #d8d8d8;
	}
	.tarea {
		margin: 30rpx 20rpx;
		height: 300rpx;
		background-color: #FFFFFF;
	}
	.btn {
		margin: 0 20rpx;
	}
	.txt{
		margin: 0 20rpx;
	}

</style>
