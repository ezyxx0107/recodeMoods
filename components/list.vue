<template>
<view class="moods">
		<view v-for="(mood, index) in moods" :key="index" :style="{backgroundColor: emojiColor[mood.emoji],color: mood.emoji === 'peace' || mood.emoji === 'sick'? '#333' : '#fff'}"
		 class="row">
			<text class="day">{{ mood.day < 10 ? `0${mood.day}` : mood.day }}</text>
			<image :src="emojiImage[mood.emoji]" mode="" class="emoji"></image>
			<text class="mood">{{ mood.mood }}</text>
			<text class="tags">
				<text v-for="(tag, ind) in mood.tags" :key="ind" class="tag">
					{{ tag }}
				</text>
			</text>
		</view>
		<view v-if="moods.length === 0" class="none-mood">
			<image src="../static/images/nothing.png" mode="" class="nothing-bg"></image>
			<text class="nothing-hint">please recode your mood now</text>
		</view>
	</view>
	</template>

	<script>
		export default {
			name: 'Moods',
			props: {
				moods: {
					required: true,
					type: Array,
					default: () => []
				}
			},
			data() {
				return {
					// the background color of row which has that emoji
					emojiColor: {
						sad: '#00b7c2',
						happy: '#ffd31d',
						sick: '#ebecf1',
						peace: '#f7f7f7',
						cool: '#b4f2e1',
						anger: '#fa744f'
					},
					// the background image of row which has that emoji
					emojiImage: {
						sad: require('../static/images/sad.png'),
						happy: require('../static/images/happy.png'),
						sick: require('../static/images/sick.png'),
						peace: require('../static/images/peace.png'),
						cool: require('../static/images/cool.png'),
						anger: require('../static/images/anger.png')
					}
				}
			}
		}
	</script>

	<style scoped>
		text {
			display: inline-block;
		}

		.moods .row {
			display: block;
			margin: 0 10px 10px 10px;
			padding: 0 10px;
			box-shadow: 0 1px 5px #C0C0C0;
			border-radius: 5px;
			min-height: 76px;
			text-align: center;
			position: relative;
		}

		.moods .day {
			display: inline-block;
			font-size: 28px;
			opacity: 0.6;
			position: absolute;
			left: 3%;
			top: 5%;
		}

		.moods .emoji {
			display: inline-block;
			width: 46px;
			height: 46px;
			opacity: 0.8;
			position: absolute;
			left: 15%;
			top: 50%;
			transform: translateY(-50%);
		}

		.moods .mood {
			position: relative;
			line-height: 1.2;
			margin-top: 20px;
			z-index: 99;
			width: 80%;
			margin-left: 24%;
		}

		.moods .tags {
			position: absolute;
			right: 5px;
			bottom: 5px;
		}

		.moods .tag {
			padding: 0 5px;
			border-radius: 10px;
			line-height: 1.2;
			border: 1px #fce2ce solid;
			font-size: 12px;
			margin-left: 10px;
		}

		.moods .none-mood {
			position: absolute;
			left: 50%;
			top: 20%;
			text-align: center;
		}

		.moods .nothing .nothing-bg {
			display: inline-block;
			width: 50%;
			margin-bottom: 20px;
		}

		.moods .none-mood .nothing-hint {
			color: #999999;
			font-size: 14px;
		}
	</style>
