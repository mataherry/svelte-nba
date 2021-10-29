<script>	
  import {teams} from './Util.svelte'
  
	let confs;

	$: fetch(`https://data.nba.net/prod/v1/current/standings_conference.json`)
		.then(r => r.json())
		.then(data => {
			confs = data.league.standard.conference
		});
</script>

{#if confs}
  <div style="display:table; width:100%">
    <table>
      <thead><td colspan="3" class="center"><b>EAST</b></td></thead>
      <thead><td>No</td><td class="center">Team</td><td>W-L</td></thead>
      {#each confs.east as team, i}
      <tr>
        <td>{i+1}</td>
        <td class="left"><a href='/#/team/{teams.find(t => t.teamId == team.teamId).urlName}'>{team.teamSitesOnly.teamTricode}</a></td>
        <td>{team.win}-{team.loss}</td>
      </tr>
      {/each}
    </table>
    <table>
      <thead><td colspan="3" class="center"><b>WEST</b></td></thead>
      <thead><td>No</td><td class="center">Team</td><td>W-L</td></thead>
      {#each confs.west as team, i}
      <tr>
        <td>{i+1}</td>
        <td class="left"><a href='/#/team/{teams.find(t => t.teamId == team.teamId).urlName}'>{team.teamSitesOnly.teamTricode}</a></td>
        <td>{team.win}-{team.loss}</td>
      </tr>
      {/each}
    </table>
  </div>
{:else}
	<p class="loading">Loading ...</p>
{/if}

<style>
  table {
    width: 45%;
    float: left;
    margin: 5px;
  }
</style>