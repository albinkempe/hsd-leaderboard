<script>
	import { onMount } from 'svelte';
	import Card from './card.svelte';

	export let name = 'turbo';
	export let steamname = 'turbo#9840';
	export let progress = 0;
	export let league = 'Diamond 4';

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
</script>

<div class="player {progress >= 40000 ? 'complete' : ''}">
	<p class="player-steamname">{steamname}</p>
	<Card title={league} progress={progress} total={40000} />
</div>

<style>
	.player {
		background-color: rgb(10, 10, 10);
		border-radius: 3px;
		padding: 2rem;
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
		border: 2px solid transparent;
		transition:
			border 0.3s ease-in-out,
			box-shadow 0.3s ease-in-out;
	}

	.player.complete {
		border: 2px solid rgb(0, 123, 255);
		box-shadow: 0 0 15px rgba(0, 123, 255, 0.5);
	}

	.player-steamname {
		font-size: 1.2rem;
		font-weight: bold;
	}

	@media (min-width: 769px) {
		.player {
			width: 20vw;
		}

		.player-steamname {
			font-size: 2rem;
			margin: 0.5rem 0 1.5rem 0;
		}
	}
</style>
