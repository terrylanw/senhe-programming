<template>
	<view class="course-dir-container">
		<uni-collapse>
			<uni-collapse-item :show-animation="true" title="开启动画" 
				v-for="(chapter,chapterIndex) of chapterList" 
				:key="chapter.id"
				:title="`章节${chapterIndex + 1} : ${chapter.title}`"
				class="collapse-item"
			>
				<view class="collapse-content" 
					v-for="(section,sectionIndex) of chapter.sectionList"
					:key="section.id"
					:class="{active: chapterIndex === currentChapter && sectionIndex === currentSection}"
					@click="switchPlay(chapterIndex,sectionIndex,section.isTry)"
				>
					<text class="iconfont icon-24gf-playCircle play"></text>
					<text class="text">{{chapterIndex + 1}}-{{sectionIndex + 1}}.{{section.courseName}}</text>
					<template v-if="!isFree">
						<text class="free-text" v-if="section.isTry">试看</text>
						<text class="iconfont icon-suoding lock" v-else></text>
					</template>
				</view>
			</uni-collapse-item>
		</uni-collapse>
	</view>
</template>

<script>
	import { mapState } from 'vuex'
	export default {
		props: {
			chapterList: {
				type: Array,
				required: true
			},
			isFree: {
				type: Number,
				required: true
			},
			currentChapter: null,
			currentSection: null
		},
		computed: {
			...mapState('user',['authStatus'])
		},
		methods: {
			switchPlay(chapterIndex,sectionIndex,isTry) {
				if(this.isFree || isTry) {
					if(!this.authStatus) return this.$loginTip()
					uni.$emit('courseIndexChange',{chapterIndex, sectionIndex, navTo: true})
				}
			}
		}
	}
</script>

<style lang="scss">
	.course-dir-container {
		.collapse-content {
			padding: 30rpx 40rpx;
			font-size: 32rpx;
			border-bottom: 2rpx solid #f1f1f1;
			color: #959595;
			@include flex-layout($alignItems: center);
			.lock {
				font-size: 28rpx;
			}
			.play {
				font-size: 20rpx;
				margin-right: 10rpx;
			}
			.text {
				margin-right: auto;
			}
			.free-text {
				color: $senhe-theme-color;
				border: 2rpx solid $senhe-theme-color;
				font-size: 26rpx;
				padding: 0 3rpx;
			}
		}
		
		.active {
			color: $senhe-theme-color;
		}
	}
</style>
