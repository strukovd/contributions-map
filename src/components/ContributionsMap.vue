<template>
	<section class="contributionsMapSection">
		<table class="contributionsTable">
			<tr v-for="(week, weekIndex) of contributionMatrix" :key="weekIndex">
				<td class="weekName">{{ daysOfWeek[weekIndex].shortName }}</td>
				<td v-for="(day, dayIndex) in week" :key="dayIndex" class="ceilDay">
					<ContributionItem :contributions="day.contributions" :isoDate="day.ISODate"/>
				</td>
			</tr>
		</table>
		<ContributionLegend/>
	</section>
</template>

<script>
import axios from 'axios';
import ContributionItem from './ContributionItem.vue';
import ContributionLegend from './ContributionLegend.vue';
const { DateTime } = require('luxon');

export default {
	components: { ContributionItem, ContributionLegend },
	name: 'ContributionsMap',
	mounted() {
		this.contributionMatrix = this.generateContributionsMatrix();
		this.fetchContributionItems();
	},
	data() {
		return {
			contributionsMap: {},
			contributionsLoaded: false,
			daysOfWeek: [
				{id: 1,	name: "Понедельник",	shortName: "Пн"},
				{id: 2,	name: "Вторник",		shortName: ""},
				{id: 3,	name: "Среда",			shortName: "Ср"},
				{id: 4,	name: "Четверг",		shortName: ""},
				{id: 5,	name: "Пятница",		shortName: "Пт"},
				{id: 6,	name: "Суббота",		shortName: ""},
				{id: 0,	name: "Воскресенье",	shortName: ""}
			],
			contributionMatrix: []
		};
	},
	methods: {
		async fetchContributionItems() {
			axios
				.get(`https://dpg.gg/test/calendar.json`)
				.then((response) => {
					this.contributionsMap = response.data;
					this.contributionMatrix = this.generateContributionsMatrix();
					// this.tasksLoaded = true;
				})
				.catch((error) => {
					console.error(error);
				});
		},

		generateContributionsMatrix() {
			const matrix = [];
			const currentDate = DateTime.now();
			const firstWeek = currentDate.minus({ weeks: 50 });

			for (const dayOfWeek of this.daysOfWeek) {
				let curDayObject = firstWeek.set({ weekday: dayOfWeek.id }); // Перещаемся на нужный день недели (напр.: пн)
				const weekRow = [];

				for (let i = 0; i < 51; i++) {
					// Собираем данные текущего дня
					const curISODate = curDayObject.toISODate();
					const curDayData = {
						ISODate: curISODate
					};

					// Прверяем наличие контрибуций на текущую дату
					if(this.contributionsMap && !isNaN(this.contributionsMap[curISODate]) ) {
						curDayData["contributions"] = this.contributionsMap[curISODate];
					}
					weekRow.push(curDayData);

					// Переходим на след. неделю
					curDayObject = curDayObject.plus({ weeks: 1 });
				}

				matrix.push(weekRow);
			}

			return matrix;
		}
	},
}
</script>

<style lang="scss" scoped>
	.contributionsMapSection {
		background: #fff;
		display: inline-block;
		padding: 3em 5em;

		.contributionsTable {
			border-collapse: collapse;

			tr {

				td {
					// border:2px solid transparent;
					padding: 0;
					margin: 0;
				}

				.weekName {
					font-size: 12px;
					padding:0 .3em 0 0;
					text-align: center;
					color: #959494;
				}
			}
		}
	}
</style>
