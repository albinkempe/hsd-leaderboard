<script>
	import Player from '$lib/components/player.svelte';
	import { onMount } from 'svelte';

	/**
	 * @type {any[] | null | undefined}
	 */
	let players = [];

	onMount(async () => {
		try {
			const res = await fetch(
				'https://api.the-finals-leaderboard.com/v1/leaderboard/s8worldtour/crossplay?clubTag=HSD'
			);
			if (!res.ok) throw new Error(`HTTP ${res.status}`);
			const data = await res.json();
			console.log(data.data);
			players = data.data.map((/** @type {{ name: any; steamName: any; }} */ player) => ({
				name: player.name,
				steamname: player.steamName
			}));
			console.log(players);
		} catch (err) {
			console.error('Failed to fetch rank:', err);
		}
	});
</script>

<main>
	<div class="players">
		{#each players as player}
			<Player name={player.name} steamname={player.steamname} />
		{/each}
	</div>
</main>

<style>
.players {
  display: flex;
  flex-direction: column; /* default for mobile */
  justify-content: center;
  align-items: center;
  gap: 2rem; /* default gap for mobile */
  flex-wrap: nowrap; /* optional */
  height: 100%;
  width: 100%;
}

main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  color: white;
  font-family: monospace;
  font-size: 2rem;
}

:global(body) {
  background-color: black;
}

/* Larger screens (tablet/desktop) */
@media (min-width: 769px) {
  .players {
    flex-direction: row; /* switch to row for larger screens */
    gap: 4rem; /* larger gap */
    flex-wrap: wrap;
  }
}

</style>
