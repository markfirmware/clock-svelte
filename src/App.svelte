<script>
	import { time } from './stores.js';
	const formatter = new Intl.DateTimeFormat('en', {
		hour12: true,
		hour: 'numeric',
		minute: '2-digit',
		second: '2-digit'
	});
</script>

<h1>The time is {formatter.format($time)}</h1>
<h2>
	<ul>
		<li>A</li>
        <li>D</li>
        <li>F</li>
	</ul>
</h2>