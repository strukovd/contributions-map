<template>
	<div class="legend">
		<span class="label">Меньше</span>
		<span class="contributionItems">
			<span class="no-contributions"></span>
			<span class="low-contributions"></span>
			<span class="some-contributions"></span>
			<span class="high-contributions"></span>
			<span class="very-high-contributions"></span>
		</span>
		<span class="label">Больше</span>
	</div>
</template>

<script>

export default {
	name: 'ContributionLegend',
}
</script>

<style lang="scss" scoped>
	.legend {
		display: flex;
		padding:1em 0;

		.label {
			font-size:8px;
			color: #959494;
			line-height: 15px;
			padding:0 .5em;
		}

		.contributionItems {
			display: flex;

			> span {
				height: 15px;
				width: 15px;
				margin:0 1px;
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
		}
	}
</style>
