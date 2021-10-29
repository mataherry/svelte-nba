<script>
	import BoxScore from './BoxScore.svelte'
	
	export let gameId
	
	let game
	
	$: fetch(`https://data.nba.net/10s/prod/v1/${gameId}_boxscore.json`)
		.then(r => r.json())
		.then(data => {
			game = data;
		});
</script>

{#if game}
	<BoxScore game={game.basicGameData}/>
	<table>
		<thead><td colspan=8 class="center white"><b>{game.basicGameData.vTeam.triCode}</b></td></thead>
		<thead class="small">
			<td class="left">Name</td>
			<td>Pos</td>
			<td>Reb</td>
			<td>Ast</td>
			<td>TO</td>
			<td>Stl</td>
			<td>Blk</td>
			<td>Pts</td>
		</thead>
		{#each game.stats.activePlayers.filter(a => a.teamId == game.basicGameData.vTeam.teamId) as p}
			<tr>
				<td class="left"><a href='/#/player/{p.personId}'>{p.firstName} {p.lastName}</a></td>
				<td class="left small">{p.pos}</td>
				<td>{p.totReb}</td>
				<td>{p.assists}</td>
				<td>{p.turnovers}</td>
				<td>{p.steals}</td>
				<td>{p.blocks}</td>
				<td>{p.points}</td>
			</tr>
		{/each}
		<tr><td colspan=8 class="white">&nbsp</td></tr>
		<thead><td colspan=8 class="center"><b>{game.basicGameData.hTeam.triCode}</b></td></thead>
		<thead class="small">
			<td class="left">Name</td>
			<td>Pos</td>
			<td>Reb</td>
			<td>Ast</td>
			<td>TO</td>
			<td>Stl</td>
			<td>Blk</td>
			<td>Pts</td>
		</thead>
		{#each game.stats.activePlayers.filter(a => a.teamId == game.basicGameData.hTeam.teamId) as p}
			<tr>
				<td class="left"><a href='/#/player/{p.personId}'>{p.firstName} {p.lastName}</a></td>
				<td class="left small">{p.pos}</td>
				<td>{p.totReb}</td>
				<td>{p.assists}</td>
				<td>{p.turnovers}</td>
				<td>{p.steals}</td>
				<td>{p.blocks}</td>
				<td>{p.points}</td>
			</tr>
		{/each}
	</table>
{:else}
	<p class="loading">loading...</p>
{/if}

<style>

</style>