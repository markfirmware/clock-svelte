<svelte:head>
	<title>Clock</title>
</svelte:head>

<script>
	import { onMount } from 'svelte';

	let now = new Date();
	
	$: hide = ms > 800;
	$: d = now.getDate();
	$: hours = now.getHours();
	$: minutes = now.getMinutes();
	$: seconds = now.getSeconds();
	$: ms = now.getMilliseconds();
	$: time = 
		'Correct Time: ' + (hours % 12 == 0 ? 12 : hours % 12) + ':' + (minutes < 10 ? '0' : '') + minutes + ':' + (seconds < 10 ? '0' : '') + seconds;
	$: day = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'][now.getDay()] + ' ' + (hours < 6 ? 'Night' : hours < 12 ? 'Morning' : hours < 18 ? 'Afternoon' : hours < 21 ? 'Evening' : 'Night');
	$: date = ['January','February','March','April','May','June','July','August','September','October','November','December'][now.getMonth()] + ' ' + d + (d == 1 || d == 21 ||  d == 31 ? "st" : d == 2 || d == 22 ? "nd" : d == 3 || d == 23 ? "rd" : "th") + ' ' + now.getFullYear();

	onMount(() => {
		const interval = setInterval(() => {
			now = new Date();
		}, 100);

		return () => {
			clearInterval(interval);
		};
	});
</script>

<style>
	.hide {
			opacity:0;
			transition:opacity 0.3s
	}
	.page {
		background-color:black;
		color:yellow;
		font-family:monospace;
		font-size:40px;
		line-height:98%
	}
</style>

<div class=page>
  <div class:hide>
		{time}
  </div>
  <div>	
		{day}
  </div>
  <div>	
		{date}
  </div>
</div>
