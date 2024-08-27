<script>
    import { createEventDispatcher } from 'svelte'; // Import createEventDispatcher

    export let initialTime = 2; // Time in minutes passed as a prop
    export let isActive = false; // Control whether the timer should run
	export let cla="";

    let mins = initialTime;
    let secs = 0;
    let interval;

    const dispatch = createEventDispatcher(); // Create event dispatcher

    // Function to start the timer
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

    // Function to stop the timer
    function stop_timer() {
        clearInterval(interval);
    }

    // Watch for changes in `isActive` prop to start/stop the timer
    $: if (isActive) {
        start_timer();
    } else {
        stop_timer();
    }

    // Function to handle click event to toggle timer
    function handleClick() {
        dispatch('toggle'); // Dispatch a custom event to parent
    }

    // Watch for changes in `initialTime` to reset the timer
    $: {
        mins = initialTime;
        secs = 0;
    }
</script>

<button on:click={handleClick}>
    <button class="cursor-pointer text-5xl m-5 {cla}">
        {mins > 0 ? `${mins} mins & ${secs} seconds` : `${secs} seconds`}
    </button>
</button>

<style>
	.upside-down{
		transform: rotate(180deg);
	}
</style>