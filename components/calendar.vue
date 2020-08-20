<template>
	<view class="calendar-model" v-if="visiableCalendar">
		<view class="year-month">
			<text @click="computedMonth('pre')" class="button"> - </text>
			<text class="year"> {{ year }} </text>
			<text> {{ monthWord[month] }} </text>
			<text @click="computedMonth('next')" class="button"> + </text>
		</view>
		<view class="weeks">
			<text v-for="item in weeksEnum" :key="item.value" class="week">{{ item.label }}</text>
		</view>
		<view class="days">
			<text v-for="(day, index) in firstDayOfMonthOffset" class="day"></text>
			<text v-for="(day, index) in days" :key="index"
			 :class="{today: index+1 === date && month === (new Date()).getMonth()+1 && year === (new Date()).getFullYear() }"
			 class="day" @click="chooseDateFn(index+1)">{{ index+1 }}</text>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'CalendarModel',
		props: {
			showMonth: {
				type: Number,
				default: (new Date()).getMonth() + 1
			},
		},
		data() {
			return {
				visiableCalendar: false,
				year: '',
				month: '',
				date: '',
				weeks: '',
				choose: {
					month: undefined,
					day: undefined
				},
				weeksEnum: [{
						value: 0,
						label: '天',
					},
					{
						value: 1,
						label: '一',
					},
					{
						value: 2,
						label: '二',
					},
					{
						value: 3,
						label: '三',
					},
					{
						value: 4,
						label: '四',
					},
					{
						value: 5,
						label: '五',
					},
					{
						value: 6,
						label: '六',
					},
				], // weekEnum
				monthWord: {
					1: 'January',
					2: 'February',
					3: 'March',
					4: 'April',
					5: 'May',
					6: 'June',
					7: 'July',
					8: 'Auguest',
					9: 'September',
					10: 'October',
					11: 'November',
					12: 'December'
				},
				firstDayOfMonthOffset: 0, // the offset that the first day of month
				days: []
			}
		},
		methods: {
			show() {
				this.initDate().then(() => {
					this.visiableCalendar = true
				})
			},
			initDate() {
				return new Promise((reject) => {
					const DATE = new Date()
					const year = DATE.getFullYear() // year
					const month = DATE.getMonth() + 1 // month
					const date = DATE.getDate() // date
					const day = DATE.getDay() // week
					this.year = year
					this.month = month
					this.date = date
					this.day = day
					this.initMonth(year, month)
					reject()
				})
			},
			initMonth(year, month) {
				const DATE = new Date(year, month - 1, 1)
				const week = DATE.getDay() // week
				this.firstDayOfMonthOffset = week
				this.computedDays()
			},
			computedDays() {
				const { month, year } = this
				let days = 31
				if (month !== 2) {
					days = month % 2 === 0 ? 30 : 31
				} else {
					days =
						year % 400 === 0 || (year % 4 === 0 && year % 100 !== 0) ? 29 : 28
				}
				this.days = new Array(days)
			},
			computedMonth(type) {
				if (type === 'pre') {
					this.month-- 
					if (this.month < 0) {
						this.month = 12
						this.year--
					} 
				} else if (type === 'next') {
					this.month++
					if (this.month > 12) {
						this.month = 1
						this.year++
					} 
				}
				this.initMonth(this.month, this.year)
			},
			chooseDateFn(day) {
				this.$emit('chooseDateFn', { year:this.year, month: this.month, day })
				this.visiableCalendar = false
			},
			close() {
				this.visiableCalendar = false
			}
		},
	}
</script>

<style>
	.calendar-model {
		position: fixed;
		left: 50%;
		top: 10%;
		transform: translateX(-50%);
		border-radius: 20px;
		width: 80%;
		min-height: 200px;
		background-color: #fff;
		box-shadow: 1px 1px 10px #C0C0C0;
		padding: 20px;
		z-index: 99;
		text-align: center;
	}
	.calendar-model .year-month {
		display: flex;
		justify-content: space-around;
		margin-bottom: 20px;
		font-size: 16px;
	}
	.calendar-model .year-month .button {
		border-radius: 50%;
		border: 1px pink solid;
		width: 26px;
		height: 26px;
		line-height: 22px;
		color: #7fdbda;
	}
	.calendar-model .year {
		color: #999999;
	}
	.calendar-model .weeks,.calendar-model .days {
		display: grid;
	  grid-template-columns: repeat(7, 14%);
		place-content: space-around space-evenly;
		line-height: 2;
	}
	.calendar-model .week,.calendar-model .day{
		border-right: 1px pink solid;
	}
	.calendar-model .weeks,.calendar-model .day {
		border-bottom: 1px pink solid;
	}
	.calendar-model .today {
		background-color: pink;
		color: #f5efef;
	}
</style>
