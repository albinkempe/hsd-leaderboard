<script>
	export let title = 'Diamond 4';
	export let progress = 90;
	export let total = 100;

	/**
	 * @param {number} progress
	 * @param {number} total
	 */
	function calculateProgress(progress, total) {
		return Math.min((progress / total) * 100, 100);
	}

	/**
	 * @param {number} progress
	 * @param {number} total
	 */
	function calculateDifference(progress, total) {
		return Math.max(total - progress, 0);
	}

	$: progressPercent = calculateProgress(progress, total);
	$: progressColor = progressPercent === 100 ? 'rgb(0, 123, 255)' : 'rgb(224, 88, 24)'; // blue if 100%, orange otherwise
</script>

<div class="card">
	<p class="card-title">{title}</p>
	<div class="progress-bar">
		<div
			class="progress"
			style="width: {progressPercent}%; background-color: {progressColor};"
		>
			<p class="progress-text">{progress}/{total}</p>
		</div>
	</div>
	{#if calculateDifference(progress, total) > 0}
		<p class="progress-difference">{calculateDifference(progress, total)} to Diamond</p>
	{:else}
		<p class="progress-difference" style="color: {progressColor};">0 to Diamond</p>
	{/if}
</div>

<style>
	.progress-difference {
		color: rgb(224, 88, 24);
		font-size: 0.9rem;
		margin: 0.7rem 0 0 0;
		padding: 0;
	}

	.progress-text {
		color: white;
		font-size: 1rem;
		text-align: center;
		margin: 0;
		padding: 0;
	}

	.progress-bar {
		width: 200px;
		height: 30px;
		background-color: rgb(56, 39, 32);
		overflow: hidden;
	}

	.progress {
		height: 100%;
		transition: width 0.5s ease-in-out, background-color 0.3s ease-in-out;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.card-title {
		font-size: 1rem;
		margin: 0 0 0.7rem 0;
	}

	.card {
		padding: 0 1rem;
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	@media (min-width: 769px) {
		.card-title {
			font-size: 1.3rem;
		}
		.card {
			width: 15vw;
		}
	}
</style>
