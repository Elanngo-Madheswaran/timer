<script>
	import { createEventDispatcher } from 'svelte';

	export let time = 2;
	export let isRunning = false;
	export let upsideDown = false;

	let sec = 0;
	let intervalId;
	let startTime;
	let elapsedTime = 0;

	$: mins = time;

	const dispatch = createEventDispatcher();

	function start_timer() {
		if (isRunning) return;

		isRunning = true;
		startTime = Date.now() - elapsedTime;

		intervalId = setInterval(update_timer, 1000);
		update_timer();
	}

	function update_timer() {
		const now = Date.now();
		elapsedTime = now - startTime;

		const totalSeconds = Math.floor(elapsedTime / 1000);
		mins = Math.floor((time * 60 - totalSeconds) / 60);
		sec = (time * 60 - totalSeconds) % 60;

		if (mins <= 0 && sec <= 0) {
			clearInterval(intervalId);
			isRunning = false;
			mins = 0;
			sec = 0;
			dispatch('timeUp');
		}
	}

	function stop_timer() {
		clearInterval(intervalId);
		isRunning = false;
	}

	function toggle_timer() {
		if (isRunning) {
			stop_timer();
		} else {
			start_timer();
		}
		dispatch('toggle', { isRunning });
	}

	$: if (!isRunning) {
		stop_timer();
	}
</script>

<div class="flex justify-center flex-col p-5 {upsideDown ? 'upside-down' : ''}">
	<button on:click={toggle_timer} class="cursor-pointer w-24 bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5">
		<h1>{mins > 0 ? (sec > 0 ? `${mins} mins & ${sec} seconds` : `${mins} minutes`) : `${sec} seconds`}</h1>
	</button>
</div>

<style>
	.upside-down {
		transform: rotate(180deg);
	}
</style>
