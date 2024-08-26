<script>
	let time = 2;
	let sec = 0;
	let interval;

	$: mins = time;

	function set_time(t){
		time = t;
		stop_timer();
	}

	function stop_timer(){
		clearInterval(interval);
		sec = 0;
	}

	function start_timer(time) {
		clearInterval(interval); // Clear any existing interval
		sec = 60; // Convert minutes to seconds
		mins -= 1;

		interval = setInterval(() => {
			if (sec > 0) {
				sec--;
			} else {
				sec = 60;
				mins--;
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
			<button class="w-24 bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5" on:click={() => set_time(t)}>{t} min</button>
		{/each}
	</div>
	<h1>{mins > 0 ? (sec > 0 ? `${mins} mins & ${sec} seconds` : `${mins} minutes`) : `${sec} seconds`}</h1>
	<button class="w-24 bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5" on:click={() => start_timer(time)}>Start</button>
	<button class="w-24 bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5" on:click={() => stop_timer()}>Stop</button>
</div>
