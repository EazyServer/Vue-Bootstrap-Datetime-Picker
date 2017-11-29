<template>
    <div class="input-group date">
        <input type="datetime-local"
               class="form-control input-md"
               :value="value"
               @click="toggleDisplay"
               @keydown.esc="reset"
        >
        <span class="input-group-addon" @click="toggleDisplay">
            &crarr;
        </span>
        <div v-show="showMyself" class="bootstrap-datetimepicker-widget dropdown-menu">
            <ul style="padding: 0">
                <li>
                    <div class="datepicker">
                        <div class="datepicker-days">
                            <table class="table-condensed">
                                <thead>
                                <tr>
                                    <th class="prev" @click.stop="goPrev" @keydown.left="goPrev">&#8249;</th>
                                    <th colspan="5" class="text-center">{{ getTitle(currentMonth) }}</th>
                                    <th class="next" @click.stop="goNext" @keydown.right="goNext">›</th>
                                </tr>
                                <tr>
                                    <th class="dow"
                                        v-for="dayIndex in 7">
                                        {{ (dayIndex - 1) | localeWeekDay(firstDay, locale) }}
                                    </th>
                                </tr>
                                </thead>
                                <tbody>
                                <tr v-for="(week, index) in weeks">
                                    <td v-for="day in week"
                                        :class="['day',
                                            day.isSelected? 'active': null,
                                            day.isToday? 'today': null,
                                            !day.isCurrentMonth? 'not-present': null]"
                                        @click="selectDate(day.date)"
                                    >{{ (showDst && day.date.isDST()?'*':'')+day.date.format('D') }}</td>
                                </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="timepicker">
                        <div class="timepicker-picker">
                            <table class="table-condensed">
                                <tbody>
                                <tr>
                                    <td v-if="showHours">
                                        <a href="#" class="btn" @click.prevent="incrementHours">&#x25B2;</a>
                                    </td>
                                    <td v-if="showHours" class="separator"></td>
                                    <td v-if="showMinutes">
                                        <a href="#" class="btn" @click.prevent="incrementMinutes">&#x25B2;</a>
                                    </td>
                                    <td v-if="showSeconds" class="separator"></td>
                                    <td v-if="showSeconds">
                                        <a href="#" class="btn" @click.prevent="incrementSeconds">&#x25B2;</a>
                                    </td>
                                </tr>
                                <tr>
                                    <td v-if="showHours">
                                        <span class="timepicker-hour">Hr:{{' '+selectedDateTimeObject.format('HH')}}</span>
                                    </td>
                                    <td v-if="showHours" class="separator">:</td>
                                    <td v-if="showMinutes">
                                        <span class="timepicker-minute">Min:{{' '+selectedDateTimeObject.format('mm')}}</span>
                                    </td>
                                    <td v-if="showSeconds" class="separator">:</td>
                                    <td v-if="showSeconds">
                                        <span class="timepicker-second">Sec:{{' '+selectedDateTimeObject.format('ss')}}</span>
                                    </td>
                                </tr>
                                <tr>
                                    <td v-if="showHours">
                                        <a href="#" class="btn" @click.prevent="decrementHours">&#x25BC;</a>
                                    </td>
                                    <td v-if="showHours" class="separator"></td>
                                    <td v-if="showMinutes">
                                        <a href="#" class="btn" @click.prevent="decrementMinutes">&#x25BC;</a>
                                    </td>
                                    <td v-if="showSeconds" class="separator"></td>
                                    <td v-if="showSeconds">
                                        <a href="#" class="btn" @click.prevent="decrementSeconds">&#x25BC;</a>
                                    </td>
                                </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
	import moment from "moment";

	export default {
		data() {
			return {
				originalValue: '',
				showMyself: false,
				currentMonth: moment().startOf('month'),
				selectedDateTimeObject: moment()
			};
		},
		props: [
			'value',
			'firstDay',
			'showDst',
			'showHours',
			'hourStep',
			'showMinutes',
			'minuteStep',
			'showSeconds',
			'secondStep',
		],
		mounted() {
			this.selectedDateTimeObject = moment(this.value);
			this.currentMonth = moment(this.selectedDateTimeObject).startOf('month');
			this.originalValue = (this.value);
		},
		computed: {
			weeks() {// calculate 2d-array of each month
				let me = this,
					monthMomentObject = this.getMonthViewStartDate(this.currentMonth, this.firstDay),
					numberOfWeeks = 5,
					weeks = [],
					lastMonth = moment(monthMomentObject),
					daysFromLastMonth = lastMonth.daysInMonth() - lastMonth.locale('en').format('D') + 1,
					daysInCurrentMonth = lastMonth.add(1, 'month').daysInMonth(),
					diff = daysFromLastMonth + daysInCurrentMonth - 35; //35 days size of current display

				if (diff === 1 || diff === 2) {
					numberOfWeeks = 6;
				}

				for (let weekIndex = 0; weekIndex < numberOfWeeks; weekIndex++) {

					weeks.push(me.getWeekObject(monthMomentObject));
				}

				return weeks;
			},
			locale : function () {
				return (typeof i18n !== 'undefined')?i18n.locale:'en';
			},
		},
		methods: {
			getMonthViewStartDate(date, firstDay) {
				firstDay = parseInt(firstDay);

				let start = moment(date),
					startOfMonth = moment(start.startOf('month'));

				start.subtract(startOfMonth.day(), 'days');

				if (startOfMonth.day() < firstDay) {
					start.subtract(7, 'days');
				}

				start.add(firstDay, 'days');

				return start;
			},

			getDayObject(monthMomentObject, dayIndex) {
				return {
					isToday: monthMomentObject.isSame(moment(), 'day'),
					isSelected: monthMomentObject.isSame(this.selectedDateTimeObject, 'day'),
					isPastDay: monthMomentObject.isBefore(moment(), 'day'),
					isCurrentMonth: monthMomentObject.isSame(this.currentMonth, 'month'),
					weekDay: dayIndex,
					isWeekEnd: (dayIndex === 5 || dayIndex === 6),
					date: moment(monthMomentObject)
				};
			},

			getWeekObject(monthMomentObject) {
				let week = [];
				for (let dayIndex = 0; dayIndex < 7; dayIndex++) {
					week.push(this.getDayObject(monthMomentObject, dayIndex));
					monthMomentObject.add(1, 'day');
				}
				return week;
			},

			getTitle(momentObject) {
				if (!momentObject) return;
				return momentObject.locale(this.locale).format('MMMM YYYY');
			},

			selectDate(date) {
				this.selectedDateTimeObject = date.hours(this.selectedDateTimeObject.hours())
					.minutes(this.selectedDateTimeObject.minutes())
					.seconds(this.selectedDateTimeObject.seconds());

				this.currentMonth = this.selectedDateTimeObject;

				this.$emit('input', this.selectedDateTimeObject.locale('en').format('YYYY-MM-DDTHH:mm:ss'));
			},

			goPrev() {
				this.currentMonth = moment(this.currentMonth).subtract(1, 'months')
					.hours(this.selectedDateTimeObject.hours())
					.minutes(this.selectedDateTimeObject.minutes())
					.seconds(this.selectedDateTimeObject.seconds());
			},
			goNext() {
				this.currentMonth = moment(this.currentMonth).add(1, 'months')
					.hours(this.selectedDateTimeObject.hours())
					.minutes(this.selectedDateTimeObject.minutes())
					.seconds(this.selectedDateTimeObject.seconds());
			},
			goToday() {
				this.currentMonth = moment();
			},

			incrementHours() {
				let step = this.hourStep?this.hourStep:1;
				this.selectedDateTimeObject = moment(this.selectedDateTimeObject).add(step, 'hours');
				this.selectDate(this.selectedDateTimeObject);
			},
			decrementHours() {
				let step = this.hourStep?this.hourStep:1;
				this.selectedDateTimeObject = moment(this.selectedDateTimeObject).subtract(step, 'hours');
				this.selectDate(this.selectedDateTimeObject);
			},

			incrementMinutes() {
				let step = this.minuteStep?this.minuteStep:1;
				this.selectedDateTimeObject = moment(this.selectedDateTimeObject).add(step, 'minutes');
				this.selectDate(this.selectedDateTimeObject);
			},
			decrementMinutes() {
				let step = this.minuteStep?this.minuteStep:1;
				this.selectedDateTimeObject = moment(this.selectedDateTimeObject).subtract(step, 'minutes');
				this.selectDate(this.selectedDateTimeObject);
			},

			incrementSeconds() {
				let step = this.secondStep?this.secondStep:1;
				this.selectedDateTimeObject = moment(this.selectedDateTimeObject).add(step, 'seconds');
				this.selectDate(this.selectedDateTimeObject);
			},
			decrementSeconds() {
				let step = this.secondStep?this.secondStep:1;
				this.selectedDateTimeObject = moment(this.selectedDateTimeObject).subtract(step, 'seconds');
				this.selectDate(this.selectedDateTimeObject);
			},
			reset(){
				this.$emit('input', this.originalValue);
				this.showMyself = false;
			},
			hideMenu(){
				this.showMyself = false;
			},
			toggleDisplay() {
				this.showMyself = !this.showMyself;
			}
		},
		filters: {
			localeWeekDay(weekday, firstDay, locale) {
				firstDay = parseInt(firstDay);
				let localMoment = moment().locale(locale);
				return localMoment.localeData().weekdaysMin()[(weekday + firstDay) % 7];
			}
		}
	};
</script>