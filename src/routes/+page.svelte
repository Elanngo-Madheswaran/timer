<script>
    import Timer from './Timer.svelte'; // Import the Timer component

    let time = 2; // Initial time in minutes
    let isRunning1 = false; // State to control Timer 1
    let isRunning2 = false; // State to control Timer 2

    // State to manage initial time for each timer
    let timer1Time = time;
    let timer2Time = time;

    function set_time(t) {
        time = t;
        timer1Time = t; // Update Timer 1 time
        timer2Time = t; // Update Timer 2 time

        // Reset timers when time is set
        isRunning1 = false;
        isRunning2 = false;
    }

    function toggle_timer(timer) {
        if (timer === 't1') {
            if (isRunning1) {
                // Stop Timer 1 and start Timer 2
                isRunning1 = false;
                isRunning2 = true;
            } else {
                // Start Timer 1 and stop Timer 2
                isRunning1 = true;
                isRunning2 = false;
            }
        } else if (timer === 't2') {
            if (isRunning2) {
                // Stop Timer 2 and start Timer 1
                isRunning2 = false;
                isRunning1 = true;
            } else {
                // Start Timer 2 and stop Timer 1
                isRunning2 = true;
                isRunning1 = false;
            }
        }
    }

    function pause_clocks() {
        // Pause both timers
        isRunning1 = false;
        isRunning2 = false;
    }

    // Function to handle space bar key press
    function handleKeydown(event) {
        if (event.code === 'Space') {
            event.preventDefault(); // Prevent default space bar behavior
            if (isRunning1 || isRunning2) {
                // Toggle the running timer
                if (isRunning1) {
                    toggle_timer('t1');
                } else if (isRunning2) {
                    toggle_timer('t2');
                }
            } else {
                // Start Timer 1 if both are paused
                isRunning1 = true;
                isRunning2 = false;
            }
        }
    }

    // Add keydown event listener on component mount
    import { onMount, onDestroy } from 'svelte';
    onMount(() => {
        window.addEventListener('keydown', handleKeydown);
    });

    // Clean up event listener on component destroy
    onDestroy(() => {
        window.removeEventListener('keydown', handleKeydown);
    });
</script>

<svelte:head>
    <title>Home</title>
    <meta name="description" content="Svelte chess clock timer" />
</svelte:head>

<div class="flex justify-center flex-col p-5">
    <div class="flex self-center">
        {#each [5, 10, 15] as t}
            <button class="w-24 bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5" on:click={() => set_time(t)}>{t} min</button>
        {/each}
    </div>

    <!-- Timer 1 with click to toggle -->
    <Timer initialTime={timer1Time} isActive={isRunning1} on:toggle={() => toggle_timer('t1')} />

    <!-- Timer 2 with click to toggle -->
    <Timer initialTime={timer2Time} isActive={isRunning2} on:toggle={() => toggle_timer('t2')} />

    <!-- Pause button -->
    <button on:click={pause_clocks} class="w-24 bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5">Pause Both</button>
</div>
