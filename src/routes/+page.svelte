<script>
	let time = 0;
	let sec = 0;
	let interval;

	$: mins = time;

	function start_timer(time) {
		clearInterval(interval); // Clear any existing interval
		sec = time * 60; // Convert minutes to seconds

		interval = setInterval(() => {
			if (sec > 0) {
				sec--;
			} else {
				clearInterval(interval);
			}
		}, 1000);
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte timer" />
</svelte:head>

<div class="flex justify-center flex-col p-5">
	<div class="flex self-center">
		{#each [5, 10, 15] as t}
			<button class="w-24 bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5" on:click={() => time = t}>{t} min</button>
		{/each}
	</div>
	<h1>{mins > 0 ? (sec > 0 ? `${mins} mins & ${sec} seconds` : `${mins} minutes`) : `${sec} seconds`}</h1>
	<button class="w-24 bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5" on:click={() => start_timer(time)}>Start</button>
</div>
