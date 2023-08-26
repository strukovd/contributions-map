<template>
	<div class="tooltip">
		<div class="contributionCount">{{ count }} contributions</div>
		<div class="contributionDate">{{ date }}</div>
	</div>
</template>

<script>
export default {
	name: 'ContributionTooltip',
	props: {
		count: Number,
		date: String
	},
	beforeMount() {
		// TODO: use vuex
		document.querySelectorAll(`.tooltip`).forEach(el => { el.remove(); });
		document.querySelectorAll(`.contribution-selected`).forEach(el => { el.classList.remove(`contribution-selected`); });
	}
}
</script>

<style lang="scss" scoped>
	.tooltip {
		$bg-color: rgba(0, 0, 0);

		position: absolute;
		display: inline-block;
		white-space: nowrap;
		background-color: $bg-color;
		padding: .3em .8em;
		border-radius: 3px;
		z-index: 100;
		bottom: 1.6em;
		transform: translate(-50%, 0);

		&:before {
			border: solid;
			border-color: $bg-color transparent;
			border-width: 6px 6px 0 6px;
			bottom: -6px;
			content: "";
			left: 50%;
			transform: translateX(-50%);
			position: absolute;
		}

		.contributionCount {
			color: #fff;
			font-size: 12px;
		}
		.contributionDate {
			font-size: 10px;
			color: #7C7C7C;
		}
	}
</style>
