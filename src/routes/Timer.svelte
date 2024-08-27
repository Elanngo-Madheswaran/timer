<script>
    import { createEventDispatcher } from 'svelte';

    export let initialTime = 2;
    export let isActive = false;
    export let cla = "";

    let mins = initialTime;
    let secs = 0;
    let interval;

    const dispatch = createEventDispatcher();

    function start_timer() {
        interval = setInterval(() => {
            if (secs === 0) {
                if (mins === 0) {
                    stop_timer();
                } else {
                    mins--;
                    secs = 59;
                }
            } else {
                secs--;
            }
        }, 1000);
    }

    function stop_timer() {
        clearInterval(interval);
    }

    $: if (isActive) {
        start_timer();
    } else {
        stop_timer();
    }

    function handleClick() {
        dispatch('toggle');
    }

    $: {
        mins = initialTime;
        secs = 0;
    }


</script>

<button class="{cla} w-auto text-5xl cursor-pointer m-5 border-0" on:click={handleClick} aria-pressed={isActive} aria-label="Timer" >
    {mins > 0 ? `${mins} mins & ${secs} seconds` : `${secs} seconds`}
</button>

<style>
	.upside-down{
		transform: rotate(180deg);
	}
</style>