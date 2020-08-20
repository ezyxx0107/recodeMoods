<template>
	<view @click.nactive="handleClickFn" class="content"> 
		<!-- list  -->
		<scroll-view
			:scroll-y="true"
			:enable-back-to-top="true"
			class="content">
			<Moods :moods="moods"/>
		</scroll-view>
		<!-- the fixed button on the sceen right -->
		<FixedButton 
			@showAddModel="showAddModel"
			@showCalender="showCalender"/>
		<!-- add model -->
		<AddModel ref="addModel" @addMoodFn="addMoodFn"/>
		<!-- calendar model -->
		<CalendarModel ref="calendarModel" @chooseDateFn="initMoodsFn"/>
	</view>
</template>

<script>
	import Moods from '../../components/list.vue'
	import FixedButton from '../../components/fixedButton.vue'
	import AddModel from '../../components/addModel.vue'
	import CalendarModel from '../../components/calendar.vue'
	
	export default {
		name: 'Home',
		components: {
			Moods,
			FixedButton,
			AddModel,
			CalendarModel
		},
		data() {
			return {
				moods: [],
				all: [
					{
						year: 2020,
						month: 8,
						day: 1,
						emoji: 'sad',
						tags: ['rain'],
						mood: 'I cant go out'
					},{
						year: 2020,
						month: 8,
						day: 2,
						emoji: 'happy',
						tags: ['sun', 'salary'],
						mood: 'Salary！'
					},{
						year: 2020,
						month: 8,
						day: 3,
						emoji: 'peace',
						tags: ['sun', 'nothing'],
						mood: 'It is nothing happened...'
					},{
						year: 2020,
						month: 8,
						day: 20,
						emoji: 'anger',
						tags: ['rain'],
						mood: 'anger day'
					}
				],
				whetherDisplayModel: undefined 
			}
		},
		onReady() {
			const DateInfo = new Date()
			const params = {
				year: DateInfo.getFullYear(),
				month: DateInfo.getMonth()+1,
				day: DateInfo.getDate()
			}
			this.initMoodsFn(params)
		},
		methods: {
			initMoodsFn({ year , month, day }){
				this.whetherDisplayModel = undefined
				// limit moods which on year-month-day
				this.moods = this.all.filter(e => e.year == year && e.month == month && e.day == day)
			},
			showAddModel() {
				if (this.whetherDisplayModel) {
					this.$refs.calendarModel.close()
				}
				this.whetherDisplayModel = 'addModel'
				this.$refs.addModel.show()
			},
			addMoodFn(form) {
				this.whetherDisplayModel = undefined
				this.moods.unshift(form)
			},
			showCalender() {
				if (this.whetherDisplayModel) {
					this.$refs.addModel.close()
				}
				this.whetherDisplayModel = 'calendarModel'
				this.$refs.calendarModel.show()
			},
			handleClickFn(e) {
				if (this.whetherDisplayModel && !e.target.dataset.ref) {
					this.$refs[this.whetherDisplayModel].close()
					this.whetherDisplayModel = undefined
				}
			}
		},
	}
</script>

<style scoped>
</style>
