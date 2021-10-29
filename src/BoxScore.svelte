<script>
	import {teams} from './Util.svelte'
	
	export let game
	
	function isWin(team) {
		if (game.statusNum !== 3) return false
		if (game.vTeam === team)
			return (parseInt(game.vTeam.score) > parseInt(game.hTeam.score))
		else
			return (parseInt(game.hTeam.score) > parseInt(game.vTeam.score))
	}
</script>

<table>
	<thead>
		<td class="left"><small><a href='/#/{game.homeStartDate}'>{game.homeStartDate}</a></small></td>
		{#if game.statusNum > 1}
			{#if game.period}
			{#each {length: 4} as _, i}
			<td style="font-weight: {game.statusNum === 2 && game.period.current == i + 1 ? 'bold' : ''}"><small>{i+1}</small></td>
			{/each}
			{#if game.period.current > 4}
				{#each {length: (game.period.current - 4)} as _, i}
		<td style="font-weight: {game.statusNum === 2 && game.period.current - 4 == i + 1 ? 'bold' : ''}"><small>O{i + 1}</small></td>
				{/each}
			{/if}
			{/if}
		<td style="font-weight: {game.statusNum === 3 ? 'bold' : ''}"><small>{game.statusNum === 3 ? 'Final': game.statusNum === 2 ? game.clock : 'Total'}</small></td>
		{:else}
		<td><small>{game.homeStartTime}</small></td>
		{/if}
	</thead>
	<tr style="font-weight: {isWin(game.vTeam) ? 'bold' : ''}">
		<td class="left">
			{#if teams}
			<a href='/#/team/{teams.find(t => t.teamId == game.vTeam.teamId).urlName}'>{game.vTeam.triCode ? game.vTeam.triCode : teams.find(t => t.teamId == game.vTeam.teamId).tricode}</a>
			{:else}
				{game.vTeam.triCode}
			{/if}
			{#if game.vTeam.win}<small>({game.vTeam.win}-{game.vTeam.loss})</small>{/if}</td>
		{#if game.vTeam.linescore}
		{#each game.vTeam.linescore as l}
		<td>{l.score}</td>
		{/each}
		{/if}
		<td><a href="/#/game/{game.homeStartDate}/{game.gameId}">{game.vTeam.score}</a></td>
	</tr>
	<tr style="font-weight: {isWin(game.hTeam) ? 'bold' : ''}">
		<td class="left">
			{#if teams}
			<a href='/#/team/{teams.find(t => t.teamId == game.hTeam.teamId).urlName}'>{game.hTeam.triCode ? game.hTeam.triCode : teams.find(t => t.teamId == game.hTeam.teamId).tricode}</a>
			{:else}
				{game.hTeam.triCode}
			{/if}
			{#if game.hTeam.win}<small>({game.hTeam.win}-{game.hTeam.loss})</small>{/if}</td>
		{#if game.hTeam.linescore}
		{#each game.hTeam.linescore as l}
		<td>{l.score}</td>
		{/each}
		{/if}
		<td class="score"><a href="/#/game/{game.homeStartDate}/{game.gameId}">{game.hTeam.score}</a></td>
	</tr>
</table>
<br/>

<style>
	td {
		min-width: 30px;
	}
	
	.score {
		min-width: 40px;
	}
</style>