<script>
	import { createEventDispatcher } from 'svelte';

	export let title = 'Diamond 4';
	export let progress = 90;
	export let total = 100;

	const dispatch = createEventDispatcher();

	$: progressPercent = Math.min((progress / total) * 100, 100);
	$: remaining = Math.max(total - progress, 0);
	$: isComplete = progressPercent === 100;
	$: progressColor = isComplete ? 'rgb(0, 123, 255)' : 'rgb(224, 88, 24)';

	// Notify parent if completion changes
	$: dispatch('completeChange', { complete: isComplete });
</script>

<div class="card">
	<p class="card-title">{title}</p>

	<div class="progress-bar">
		<div class="progress" style="width: {progressPercent}%; background-color: {progressColor};">
			<p class="progress-text">{progress}/{total}</p>
		</div>
	</div>

	<p class="progress-difference" style="color: {isComplete ? progressColor : ''};">
		{remaining} to Diamond
	</p>
</div>

<style>
	.card {
		padding: 0 1rem;
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
		border: 2px solid transparent;
		border-radius: 10px;
		transition: border 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
	}

	.progress-bar {
		width: 200px;
		height: 30px;
		background-color: rgb(56, 39, 32);
		overflow: hidden;
	}

	.progress {
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		transition: width 0.5s ease-in-out, background-color 0.3s ease-in-out;
	}

	.progress-text {
		color: white;
		font-size: 1rem;
		margin: 0;
	}

	.progress-difference {
		font-size: 0.9rem;
		margin-top: 0.7rem;
		color: rgb(224, 88, 24);
	}
</style>
