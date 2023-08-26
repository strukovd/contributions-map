<template>
	<div class="contribution" :class="getClass(contributions)">

	</div>
</template>

<script>
export default {
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
		}
	}
}
</script>

<style scoped>
	.contribution {
		width: 15px;
		height: 15px;
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
