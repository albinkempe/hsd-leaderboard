<script>
	import { onMount } from 'svelte';
	import Card from './card.svelte';

	export let name = 'turbo';
	export let steamname = 'turbo#9840';
	export let rank = 0;
	export let league = 'Diamond 4';

	onMount(async () => {
		try {
			const encodedName = encodeURIComponent(name);
			const res = await fetch(
				`https://api.the-finals-leaderboard.com/v1/leaderboard/s8/crossplay?name=${encodedName}`
			);
			if (!res.ok) throw new Error(`HTTP ${res.status}`);
			const data = await res.json();
			console.log(data.data[0].rankScore);
			rank = data.data[0].rankScore;
			league = data.data[0].league;
		} catch (err) {
			console.error('Failed to fetch rank:', err);
		}
	});
</script>

<div class="player">
	<p class="player-steamname">{steamname}</p>
	<p class="player-name">{name}</p>
	<Card title={league} progress={rank} total={40000} />
</div>

<style>
	.player-steamname {
		font-size: 2rem;
		font-weight: bold;
		margin: 0.5rem 0 1.5rem 0;
	}
	.player-name {
		font-size: 1.2rem;
		color: rgb(94, 94, 94);
		margin: 0;
	}
	.player {
		background-color: rgb(14, 14, 14);
		padding: 2rem;
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
	}
</style>
