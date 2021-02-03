<script context="module">
	export function formatDate(d) {
		return d.toISOString().slice(0,10).replace(/-/g,"")
	}
	
	export function parseDate(d) {
		return new Date(d.substring(0,4) + '-' + d.substring(4,6) + '-' + d.substring(6,8))
	}
	
	export let teams
	fetch(`https://data.nba.net/10s/prod/v1/2020/teams.json`)
		.then(r => r.json())
		.then(data => {
			teams = data.league.standard;
		});
</script>