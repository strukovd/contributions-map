<template>
	<section class="ContributionsMap">
		{{ contributionMatrix }}
	</section>
</template>

<script>
// import axios from 'axios';
const { DateTime } = require('luxon');

export default {
	name: 'ContributionsMap',
	data() {
		return {
			contributionsMap: {},
			contributionMatrix: this.generateContributionsMatrix(),
		};
	},
	methods: {
		generateContributionsMatrix() {
			const matrix = [];
			const daysOfWeek = [
				{id: 1,	name: "Понедельник",	shortName: "Пн"},
				{id: 2,	name: "Вторник",		shortName: "Вт"},
				{id: 3,	name: "Среда",			shortName: "Ср"},
				{id: 4,	name: "Четверг",		shortName: "Чт"},
				{id: 5,	name: "Пятница",		shortName: "Пт"},
				{id: 6,	name: "Суббота",		shortName: "Сб"},
				{id: 0,	name: "Воскресенье",	shortName: "Вс"}
			];
			const currentDate = DateTime.now();
			const firstWeek = currentDate.minus({ weeks: 50 });

			for (const dayOfWeek of daysOfWeek) {
				let curDayObject = firstWeek.set({ weekday: dayOfWeek.id }); // Перещаемся на нужный день недели (напр.: пн)
				const weekRow = [
					{shortName: dayOfWeek.shortName}
				];
				// weekRow.push(dayOfWeek.shortName);

				for (let i = 0; i < 51; i++) {
					// Собираем данные текущего дня
					const curDayFormated = curDayObject.toISODate();
					const curDayData = {
						ISODate: curDayFormated
					};

					// Прверяем наличие контрибуций на текущую дату
					if(this.contributionsMap && !isNaN(this.contributionsMap[curDayFormated]) ) {
						curDayData["contributions"] = this.contributionsMap[curDayFormated];
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

<style scoped>
	.ContributionsMap {

	}
</style>
