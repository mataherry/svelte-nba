<script>
	import { beforeUpdate } from "svelte";
	import BoxScore from './BoxScore.svelte'
	
	export let date;
	let games;

	$: fetch(`https://data.nba.net/10s/prod/v1/${date}/scoreboard.json`)
		.then(r => r.json())
		.then(data => {
			games = data.games;
		});
</script>

{#if games}
	{games.length} Games
	{#each games as game}
		<BoxScore {game}/>
	{/each}
{:else}
	<p class="loading">loading...</p>
{/if}

<style>
	
</style>