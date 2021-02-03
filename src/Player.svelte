<script>
	import BoxScore from './BoxScore.svelte'
	import {teams} from './Util.svelte'
	
	export let playerId;
	let player;

	$: fetch(`https://data.nba.net/v2015/json/mobile_teams/nba/2020/players/playercard_${playerId}_02.json`)
		.then(r => r.json())
		.then(data => {
			player = data.pl;
		});
</script>

{#if player}
	<h3>{player.fn} {player.ln}</h3>
	<p>#{player.num} {player.tc} {player.tn}</p>
	<table>
		<thead>
			<td class="left">Date</td>
			<td class="left">Opp</td>
			<td class="left">Score</td>
			<td>Reb</td>
			<td>Ast</td>
			<td>TO</td>
			<td>Stl</td>
			<td>Blk</td>
			<td>Pts</td>
		</thead>
		{#each player.gls.glt.find(g => g.desc == 'Regular').gl as p}
		<tr>
			<td><a href='/#/{p.gdte.replace(/-/g,'')}'>{p.gdte}</a></td>
			<td class="left"><a href='/#/team/{teams.find(t => t.teamId == p.otid).urlName}'>{p.ota}</a></td>
			<td class="left"><a href='/#/game/{p.gdte.replace(/-/g,'')}/{p.gid}'>{p.res}</a></td>
			<td>{p.reb}</td>
			<td>{p.ast}</td>
			<td>{p.tov}</td>
			<td>{p.stl}</td>
			<td>{p.blk}</td>
			<td>{p.pts}</td>
		</tr>
		{/each}
	</table>
{:else}
	<p class="loading">loading...</p>
{/if}

<style>
	
</style>