<script>
	export let title = 'Diamond 4';
	export let progress = 90;
	export let total = 100;

	/**
	 * @param {number} current
	 * @param {number} total
	 */
	function calculateProgress(current, total) {
		return Math.min((current / total) * 100, 100);
	}
</script>

<div class="card">
	<p class="card-title">{title}</p>
	<div class="progress-bar">
		<div class="progress" style="width: {calculateProgress(progress, total)}%;">
			<p class="progress-text">{progress}/{total}</p>
		</div>
	</div>
</div>

<style>
	.progress-text {
		color: white;
		font-size: 0.8rem;
		text-align: center;
		margin: 0;
		padding-top: 2px;
	}

	.progress-bar {
		width: 200px;
		height: 20px;
		background-color: rgb(56, 39, 32);
		overflow: hidden;
	}

	.progress {
		height: 100%;
		background-color: rgb(224, 88, 24);
		transition: width 0.5s ease-in-out;
	}

	.card-title {
		font-size: 1.5rem;
		font-weight: bold;
	}

	.card {
		background-color: rgb(14, 14, 14);
		padding: 2rem;
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
	}
</style>
