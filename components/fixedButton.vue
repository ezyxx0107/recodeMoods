<template>
	<view class="fixed-button">
		<image src="../static/images/add.png" mode="" class="image"
			@click.stop="showAddModel"></image>
		<image src="../static/images/calendar.png" mode="" class="image"
			@click.stop="showCalender"></image>
	</view>
</template>

<script>
	export default {
		name: 'FixedButton',
		methods: {
			showAddModel() {
				this.$emit('showAddModel')
			},
			showCalender() {
				this.$emit('showCalender')
			}
		},
	}
</script>

<style scoped>
	.fixed-button {
		position: fixed;
		right: 10px;
		bottom: 10%;
		z-index: 99;
	}
	.fixed-button .image {
		display: block;
		width: 30px;
		height: 30px;
		margin-bottom: 20px;
		border-radius: 50%;
		box-shadow: 1px 1px 30px #faf0af;
		background-color: transparent;
	}
</style>
