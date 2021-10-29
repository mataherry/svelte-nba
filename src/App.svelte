<script>
	import {onMount} from 'svelte'
	import List from './List.svelte'
	import Game from './Game.svelte'
	import Team from './Team.svelte'
	import Player from './Player.svelte'
	import Standing from './Standing.svelte'
	import {formatDate,parseDate} from './Util.svelte'
	
	let date = formatDate(new Date())
	let gameId
	let team
	let playerId
	let stand
  	addDate(-1)
	
	async function hashchange() {
		// the poor man's router!
		const path = window.location.hash.slice(1);
		
		gameId = null
		team = null
		playerId = null
		stand = false

		if (path.startsWith('/stand)) {
			stand = true
		} else if (path.startsWith('/game')) {
			gameId = path.slice(6)
			window.scrollTo(0,0)
		} else if (path.startsWith('/team')) {
			team = path.slice(6)
			window.scrollTo(0,0)
		} else if (path.startsWith('/player')) {
			playerId = path.slice(8)
			window.scrollTo(0,0)
		} else {
			if (path.length > 1)
				date = path.slice(1)
			else {
				date = formatDate(new Date())
				addDate(-1)
			}
		}
	}

	onMount(hashchange);
	
	function addDate(i) {
		var dt = parseDate(date)
		dt.setDate(dt.getDate() + i)
		date = formatDate(dt)
	}
</script>

<svelte:window on:hashchange={hashchange}/>
<svelte:head>
    
</svelte:head>
<main>
	{#if gameId || team || playerId || stand}
		<button on:click="{() => history.back()}">Back</button>
	{/if}
	{#if gameId}
		<Game {gameId} />
	{:else if team}
		<Team {team} />
	{:else if playerId}
		<Player {playerId} />
	{:else if stand}
		<Standing />
	{:else}
		<h3>NBA Scores | <a href="/#/stand">Standing</a></h3>
		<button on:click="{() => addDate(-1)}">-</button> {date} <button on:click="{() => addDate(1)}">+</button>
		<List {date} />
	{/if}
</main>

<style>
	:global(tr:nth-child(odd), thead) {
		 background-color: #ccc;
	}
	
	:global(.left) {
		text-align: left;
	}
	
	:global(.center) {
		text-align: center;
	}
	
	:global(td) {
		text-align: right;
		padding: 0.1rem;
	}

	:global(.white) {
		background-color: white;
	}
  
  	:global(.small) {
		font-size: smaller;
	}
</style>