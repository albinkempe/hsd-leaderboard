<script lang="ts">
	import Player from '$lib/components/player.svelte';
	import { onMount } from 'svelte';

	interface PlayerData {
		name: string;
		steamname: string;
	}

	let players: PlayerData[] = [];

	onMount(async () => {
		try {
			const res = await fetch(
				'https://api.the-finals-leaderboard.com/v1/leaderboard/s8worldtour/crossplay?clubTag=HSD'
			);
			if (!res.ok) throw new Error(`HTTP ${res.status}`);

			const data = await res.json();

			players = data.data.map(
				(player: { name: string; steamName: string }): PlayerData => ({
					name: player.name,
					steamname: player.steamName
				})
			);
		} catch (err) {
			console.error('Failed to fetch rank:', err);
		}
	});

	$: sortedPlayers = [...players].sort((a, b) => a.steamname.localeCompare(b.steamname));
</script>

<main>
	<h1>HSD Esports</h1>
	<h2>Players</h2>
	<div class="players">
		{#each sortedPlayers as player}
			<Player name={player.name} steamname={player.steamname} />
		{/each}
	</div>
</main>

<style>
	h1 {
		font-size: 2rem;
		margin: 1.5rem 0 0.5rem 0;
	}

	h2 {
		font-size: 1.4rem;
		margin: 1rem 0 0.5rem 0;
	}

	.players {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: stretch;
		gap: 2rem;
		flex-wrap: nowrap;
		margin-top: 1rem;
		margin-bottom: 1rem;
		width: 90vw;
	}

	main {
		width: 100vw;
		display: flex;
		flex-direction: column;
		justify-content: start;
		align-items: center;
		font-family: monospace;
		color: white;
		font-size: 2rem;
	}

	:global(body) {
		background-color: black;
		overflow-x: hidden;
		padding: 0;
		margin: 0;
	}

	@media (min-width: 769px) {
		h1 {
			font-size: 2rem;
			margin: 22rem 0 3rem 0;
		}

		.players {
			flex-direction: row;
			gap: 4rem;
			flex-wrap: wrap;
			justify-content: center;
			align-items: center;
		}

		main {
			justify-content: start;
			height: 100vh;
		}
	}
</style>
