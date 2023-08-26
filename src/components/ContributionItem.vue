<template>
	<div class="contribution"
		@click="isSelected=true"
		:class="[
			getClass(contributions),
			{'contribution-selected': isSelected}
		]">
		<ContributionTooltip v-if="isSelected" :count="contributions" :date="customizeDate(isoDate)"></ContributionTooltip>
	</div>
</template>

<script>
import { DateTime } from 'luxon';
import ContributionTooltip from './ContributionTooltip.vue';

export default {
	components: { ContributionTooltip },
	name: 'ContributionItem',
	props: {
		isoDate: {
			type: String,
			required: true
		},
		contributions: {
			type: Number,
			required: false,
			default: 0
		}
	},
	data() {
		return {
			isSelected: false
		};
	},
	methods: {
		getClass(count) {
			if (count >= 30) {
				return "very-high-contributions";
			} else if (count >= 20) {
				return "high-contributions";
			} else if (count >= 10) {
				return "some-contributions";
			} else if (count >= 1) {
				return "low-contributions";
			} else {
				return "no-contributions";
			}
		},

		customizeDate(ISODate) {
			const date = DateTime.fromFormat(ISODate, 'yyyy-MM-dd');
			const formattedDate = date.toFormat('cccc, LLLL d, yyyy');
			return formattedDate;
		}
	}
}
</script>

<style scoped>
	.contribution {
		width: 15px;
		height: 15px;
		border: 1px solid #fff;
		position: relative;

		&:hover {
			border: 1px solid rgba(0, 0, 0, 50%);
		}
	}

	.contribution-selected {
		border: 1px solid rgba(0, 0, 0);
	}

	.no-contributions {
		background-color: #EDEDED;
	}
	.low-contributions {
		background-color: #ACD5F2;
	}
	.some-contributions {
		background-color: #7FA8C9;
	}
	.high-contributions {
		background-color: #527BA0;
	}
	.very-high-contributions {
		background-color: #254E77;
	}
</style>
