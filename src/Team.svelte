<script>
	import BoxScore from './BoxScore.svelte'
	import {teams} from './Util.svelte'
	
	export let team;
	let games;

	$: fetch(`https://data.nba.net/10s/prod/v1/2020/teams/${team}/schedule.json`)
		.then(r => r.json())
		.then(data => {
			games = data.league.standard.filter(g => g.seasonStageId > 1);
		});
</script>

{#if games}
	{#each games as game}
		<BoxScore {game} />
	{/each}
{:else}
	<p class="loading">loading...</p>
{/if}

<style>
	
</style>