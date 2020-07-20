<svelte:head>
	<title>Clock</title>
</svelte:head>

<script>
	import { slide } from 'svelte/transition';
	import { onMount } from 'svelte';

	let civil_twilight_begin = new Date("2020-07-19T10:02:09+00:00");
	let civil_twilight_end = new Date("2020-07-20T01:39:55+00:00");

	let light_begin = civil_twilight_begin.getHours() * 60 + civil_twilight_begin.getMinutes();
	let light_end = civil_twilight_end.getHours() * 60 + civil_twilight_end.getMinutes();

	let now = new Date();
	
	$: hide = seconds == 59 && ms > 500;
	$: d = now.getDate();
	$: hours = now.getHours();
	$: minutes = now.getMinutes();
	$: seconds = now.getSeconds();
	$: ms = now.getMilliseconds();
	$: time = 
		'Correct Time: ' + (hours % 12 == 0 ? 12 : hours % 12) + ':' + (minutes < 10 ? '0' : '') + minutes;
	$: day = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'][now.getDay()] + ' ' + (hours < 12 ? 'Morning' : hours < 18 ? 'Afternoon' : hours < 21 ? 'Evening' : 'Night');
	$: date = ['January','February','March','April','May','June','July','August','September','October','November','December'][now.getMonth()] + ' ' + d + (d == 1 || d == 21 ||  d == 31 ? "st" : d == 2 || d == 22 ? "nd" : d == 3 || d == 23 ? "rd" : "th") + ' ' + now.getFullYear();
    $: until_light = light_begin - (hours * 60 + minutes);
    $: until_dark = light_end - (hours * 60 + minutes);
    $: until = near(until_dark, 'dark') + near(until_light, 'light');

	onMount(() => {
		const interval = setInterval(() => {
			now = new Date();
		}, 100);

		return () => {
			clearInterval(interval);
		};
	});

    function near(minutes, name) {
        return minutes < 1 || minutes > 59 ? '' : '(' + minutes + ' minute' + (minutes == 1 ? '' : 's') + ' until ' + name + ')';
    }
</script>

<style>
	.hide {
		opacity:0;
		transition:opacity 0.7s
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
  {#if until != ''}
    <div transition:slide={{duration:800}}>
      {until}
    </div>
  {/if}
  <div>	
	{date}
  </div>
</div>
