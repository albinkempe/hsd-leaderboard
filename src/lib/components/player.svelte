<script>
	import { onMount } from 'svelte';
	import Card from './card.svelte';

	export let name = '';
	export let steamname = '';
	export let progress = 0;
	export let league = '';

	export let step = 2500;
	export let maxTotal = 47500;

	onMount(async () => {
		try {
			const encodedName = encodeURIComponent(name);
			const res = await fetch(
				`https://api.the-finals-leaderboard.com/v1/leaderboard/s8/crossplay?name=${encodedName}`
			);
			if (!res.ok) throw new Error(`HTTP ${res.status}`);
			const data = await res.json();
			progress = data.data[0].rankScore;
			league = data.data[0].league;
		} catch (err) {
			console.error('Failed to fetch rank:', err);
		}
	});

	$: total = Math.min(Math.max(Math.ceil(progress / step) * step, step), maxTotal);
</script>

<div class="player">
	<p class="player-steamname">{steamname}</p>
	<Card title={league} {progress} {total} />
</div>

<style>
	.player {
		background-color: rgb(10, 10, 10);
		border-radius: 3px;
		padding: 1rem 2rem;
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
		border: 2px solid transparent;
		transition:
			border 0.3s ease-in-out,
			box-shadow 0.3s ease-in-out;
	}

	.player-steamname {
		font-size: 1.5rem;
		font-weight: bold;
		margin: 0 0 1rem 0;
	}

	@media (min-width: 769px) {
		.player {
			width: 20vw;
			padding: 2rem 2rem;
		}

		.player-steamname {
			font-size: 2rem;
			margin: 0.5rem 0 1.5rem 0;
		}
	}
</style>
