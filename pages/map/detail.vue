<template>
	<view class="container">
		<!-- 图幅预览区域 -->
		<view class="preview-container" @click="showFullscreen">
			<image class="preview-image placeholder-image" :src="mapInfo.image"></image>
			
			<!-- 图幅标题 -->
			<view class="map-title">{{mapInfo.title}}</view>
		</view>
		
		<!-- 交互区域 -->
		<view class="interaction-bar">
			<view class="action-btn" @click="likeMap">
				<text class="iconfont" :class="{'active': isLiked}">♥</text>
				<text class="count">{{likeCount}}</text>
			</view>
			
			<view class="action-btn" @click="collectMap">
				<text class="iconfont" :class="{'active': isCollected}">★</text>
				<text class="count">收藏</text>
			</view>
			
			<view class="action-btn" @click="focusCommentInput">
				<text class="iconfont">✎</text>
				<text class="count">{{commentCount}}</text>
			</view>
		</view>
		
		<!-- 评论区 -->
		<view class="comment-section">
			<view class="section-title">评论区</view>
			
			<!-- 评论列表 -->
			<view class="comment-list">
				<block v-if="comments.length > 0">
					<view class="comment-item" v-for="(item, index) in comments" :key="index">
						<view class="comment-user">{{item.userName}}</view>
						<view class="comment-content">{{item.content}}</view>
						<view class="comment-time">{{item.time}}</view>
					</view>
				</block>
				<view class="no-comment" v-else>
					暂无评论
				</view>
			</view>
		</view>
		
		<!-- 评论输入框 -->
		<view class="comment-input-container">
			<input 
				class="comment-input"
				v-model="commentContent"
				placeholder="写下你的评论..."
				confirm-type="send"
				:focus="inputFocus"
				@confirm="submitComment"
			/>
			<button class="send-btn primary-bg" @click="submitComment">发送</button>
		</view>
		
		<!-- 全屏预览 -->
		<view class="fullscreen-preview" v-if="showFullscreenPreview">
			<image 
				class="fullscreen-image placeholder-image" 
				:src="mapInfo.image"
				:style="{ transform: `scale(${fullscreenScale}) translate(${fullscreenTranslateX}px, ${fullscreenTranslateY}px)` }"
				@touchstart="handleFullscreenTouchStart"
				@touchmove="handleFullscreenTouchMove"
				@touchend="handleFullscreenTouchEnd"
			></image>
			
			<view class="close-btn" @click="hideFullscreen">×</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mapId: '',
				// 地图信息（示例数据）
				mapInfo: {
					id: '',
					title: '湖北省地质图',
					image: '/static/placeholder.png',
					description: '展示湖北省地质结构分布'
				},
				// 交互数据
				isLiked: false,
				isCollected: false,
				likeCount: 123,
				commentCount: 5,
				// 评论相关
				comments: [
					{
						id: '1',
						userName: '地图爱好者',
						content: '这张地图非常详细，对我的研究很有帮助！',
						time: '2025-04-05 10:23'
					},
					{
						id: '2',
						userName: '资源研究员',
						content: '色彩搭配很合理，易于识别不同地质类型。',
						time: '2025-04-03 16:45'
					},
					{
						id: '3',
						userName: '湖北游客',
						content: '终于找到了家乡的详细地质信息，感谢提供！',
						time: '2025-04-01 09:12'
					},
					{
						id: '4',
						userName: '地理老师',
						content: '已经下载用于教学，学生们都很喜欢这个地图。',
						time: '2025-03-28 14:37'
					},
					{
						id: '5',
						userName: '自然爱好者',
						content: '图例清晰，内容全面，非常专业的一份地图。',
						time: '2025-03-25 20:18'
					}
				],
				commentContent: '',
				inputFocus: false,
				// 全屏预览
				showFullscreenPreview: false,
				fullscreenScale: 1,
				fullscreenTranslateX: 0,
				fullscreenTranslateY: 0,
				// 触摸事件相关变量
				lastTouchDistance: 0,
				lastTouchX: 0,
				lastTouchY: 0
			}
		},
		onLoad(options) {
			this.mapId = options.id || '';
			this.getMapDetail();
		},
		onShareAppMessage() {
			return {
				title: this.mapInfo.title,
				path: `/pages/map/detail?id=${this.mapId}`
			}
		},
		methods: {
			// 获取地图详情
			getMapDetail() {
				// 这里应该是从API获取数据
				console.log('获取地图详情，地图ID:', this.mapId);
				// 模拟API调用
				// uni.request({
				//   url: `/api/maps/${this.mapId}`,
				//   success: (res) => {
				//     this.mapInfo = res.data;
				//     this.isLiked = res.data.isLiked;
				//     this.isCollected = res.data.isCollected;
				//     this.likeCount = res.data.likeCount;
				//     this.commentCount = res.data.commentCount;
				//     this.getComments();
				//   }
				// });
				
				// 假设我们已经获取到了详情
				this.mapInfo.id = this.mapId;
			},
			
			// 获取评论列表
			getComments() {
				// 这里应该是从API获取数据
				console.log('获取评论列表，地图ID:', this.mapId);
				// 模拟API调用
				// uni.request({
				//   url: `/api/maps/${this.mapId}/comments`,
				//   success: (res) => {
				//     this.comments = res.data.comments;
				//   }
				// });
			},
			
			// 点赞地图
			likeMap() {
				// 模拟状态切换
				this.isLiked = !this.isLiked;
				this.likeCount += (this.isLiked ? 1 : -1);
				
				// 模拟API调用
				// uni.request({
				//   url: `/api/maps/${this.mapId}/like`,
				//   method: 'POST',
				//   data: {
				//     liked: this.isLiked
				//   }
				// });
			},
			
			// 收藏地图
			collectMap() {
				// 模拟状态切换
				this.isCollected = !this.isCollected;
				
				// 模拟API调用
				// uni.request({
				//   url: `/api/maps/${this.mapId}/collect`,
				//   method: 'POST',
				//   data: {
				//     collected: this.isCollected
				//   }
				// });
				
				uni.showToast({
					title: this.isCollected ? '已收藏' : '已取消收藏',
					icon: 'none'
				});
			},
			
			// 聚焦评论输入框
			focusCommentInput() {
				this.inputFocus = true;
			},
			
			// 提交评论
			submitComment() {
				if (!this.commentContent.trim()) {
					uni.showToast({
						title: '评论内容不能为空',
						icon: 'none'
					});
					return;
				}
				
				// 模拟API调用
				// uni.request({
				//   url: `/api/maps/${this.mapId}/comment`,
				//   method: 'POST',
				//   data: {
				//     content: this.commentContent
				//   },
				//   success: () => {
				//     this.commentContent = '';
				//     uni.showToast({
				//       title: '评论提交成功，待审核',
				//       icon: 'none'
				//     });
				//   }
				// });
				
				// 模拟成功
				this.commentContent = '';
				this.inputFocus = false;
				uni.showToast({
					title: '评论提交成功，待审核',
					icon: 'none'
				});
			},
			
			// 显示全屏预览
			showFullscreen() {
				this.showFullscreenPreview = true;
				this.fullscreenScale = 1;
				this.fullscreenTranslateX = 0;
				this.fullscreenTranslateY = 0;
			},
			
			// 隐藏全屏预览
			hideFullscreen() {
				this.showFullscreenPreview = false;
			},
			
			// 以下三个方法处理全屏模式下的缩放和平移
			handleFullscreenTouchStart(e) {
				const touches = e.touches;
				
				// 双指缩放
				if (touches.length === 2) {
					const touch1 = touches[0];
					const touch2 = touches[1];
					this.lastTouchDistance = Math.sqrt(
						Math.pow(touch2.pageX - touch1.pageX, 2) +
						Math.pow(touch2.pageY - touch1.pageY, 2)
					);
				} 
				// 单指平移
				else if (touches.length === 1) {
					this.lastTouchX = touches[0].pageX;
					this.lastTouchY = touches[0].pageY;
				}
			},
			
			handleFullscreenTouchMove(e) {
				const touches = e.touches;
				
				// 双指缩放
				if (touches.length === 2) {
					const touch1 = touches[0];
					const touch2 = touches[1];
					const currentDistance = Math.sqrt(
						Math.pow(touch2.pageX - touch1.pageX, 2) +
						Math.pow(touch2.pageY - touch1.pageY, 2)
					);
					
					if (this.lastTouchDistance > 0) {
						// 计算新的缩放比例
						let newScale = this.fullscreenScale * (currentDistance / this.lastTouchDistance);
						
						// 限制缩放范围 (1.0 到 5.0)
						newScale = Math.max(1, Math.min(5, newScale));
						this.fullscreenScale = newScale;
					}
					
					this.lastTouchDistance = currentDistance;
				} 
				// 单指平移
				else if (touches.length === 1 && this.fullscreenScale > 1) {
					const currentX = touches[0].pageX;
					const currentY = touches[0].pageY;
					
					// 计算新的平移位置
					this.fullscreenTranslateX += (currentX - this.lastTouchX) / this.fullscreenScale;
					this.fullscreenTranslateY += (currentY - this.lastTouchY) / this.fullscreenScale;
					
					this.lastTouchX = currentX;
					this.lastTouchY = currentY;
				}
			},
			
			handleFullscreenTouchEnd() {
				this.lastTouchDistance = 0;
			}
		}
	}
</script>

<style>
	.container {
		padding-bottom: 100rpx;
	}
	
	/* 图幅预览区域 */
	.preview-container {
		position: relative;
		width: 100%;
		height: 500rpx;
	}
	
	.preview-image {
		width: 100%;
		height: 100%;
	}
	
	.map-title {
		position: absolute;
		bottom: 0;
		left: 0;
		width: 100%;
		padding: 20rpx;
		background-color: rgba(0, 0, 0, 0.5);
		color: #FFFFFF;
		font-size: 32rpx;
	}
	
	/* 交互区域 */
	.interaction-bar {
		display: flex;
		padding: 20rpx;
		background-color: #FFFFFF;
		border-bottom: 1px solid #EEEEEE;
	}
	
	.action-btn {
		flex: 1;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	
	.iconfont {
		font-size: 48rpx;
		line-height: 1;
		color: #999999;
	}
	
	.iconfont.active {
		color: #2E8B57;
	}
	
	.count {
		font-size: 24rpx;
		color: #666666;
		margin-top: 10rpx;
	}
	
	/* 评论区 */
	.comment-section {
		background-color: #FFFFFF;
		padding: 20rpx;
		margin-top: 20rpx;
	}
	
	.section-title {
		font-size: 32rpx;
		font-weight: bold;
		margin-bottom: 20rpx;
		color: #333333;
	}
	
	.comment-list {
		padding-bottom: 30rpx;
	}
	
	.comment-item {
		padding: 20rpx 0;
		border-bottom: 1px solid #EEEEEE;
	}
	
	.comment-user {
		font-size: 28rpx;
		font-weight: bold;
		color: #333333;
		margin-bottom: 10rpx;
	}
	
	.comment-content {
		font-size: 28rpx;
		color: #666666;
		line-height: 1.5;
		margin-bottom: 10rpx;
	}
	
	.comment-time {
		font-size: 24rpx;
		color: #999999;
	}
	
	.no-comment {
		text-align: center;
		padding: 30rpx 0;
		color: #999999;
	}
	
	/* 评论输入框 */
	.comment-input-container {
		position: fixed;
		bottom: 0;
		left: 0;
		width: 100%;
		padding: 20rpx;
		background-color: #FFFFFF;
		border-top: 1px solid #EEEEEE;
		display: flex;
		align-items: center;
	}
	
	.comment-input {
		flex: 1;
		height: 80rpx;
		border: 1px solid #EEEEEE;
		border-radius: 40rpx;
		padding: 0 20rpx;
		font-size: 28rpx;
		background-color: #F8F8F8;
	}
	
	.send-btn {
		width: 120rpx;
		height: 80rpx;
		line-height: 80rpx;
		font-size: 28rpx;
		color: #FFFFFF;
		margin-left: 20rpx;
		border-radius: 40rpx;
	}
	
	/* 全屏预览 */
	.fullscreen-preview {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		background-color: #000000;
		z-index: 999;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.fullscreen-image {
		width: 100%;
		height: 100%;
		object-fit: contain;
	}
	
	.close-btn {
		position: absolute;
		top: 40rpx;
		right: 40rpx;
		width: 80rpx;
		height: 80rpx;
		line-height: 80rpx;
		text-align: center;
		font-size: 60rpx;
		color: #FFFFFF;
		background-color: rgba(0, 0, 0, 0.5);
		border-radius: 50%;
	}
</style>