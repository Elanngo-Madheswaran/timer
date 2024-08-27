<script>
    let time = 2; // Initial time in minutes
    let mins1 = time, mins2 = time;
    let sec1 = 0, sec2 = 0;
    let interval1, interval2;
    let isRunning1 = false, isRunning2 = false;

    // Initialize mins1 and mins2 to the set time
    $: mins1 = time;
    $: mins2 = time;

    function set_time(t) {
        time = t;
        mins1 = t;
        mins2 = t;
        sec1 = 0;
        sec2 = 0;
        stop_timer("t1");
        stop_timer("t2");
    }

    function stop_timer(timer) {
        if (timer === "t1") {
            clearInterval(interval1);
            isRunning1 = false;
        } else if (timer === "t2") {
            clearInterval(interval2);
            isRunning2 = false;
        }
    }

    function start_timer(timer) {
        if (timer === "t1" && !isRunning1) {
            isRunning1 = true;
            interval1 = setInterval(() => {
                if (sec1 === 0) {
                    if (mins1 === 0) {
                        stop_timer("t1");
                    } else {
                        mins1--;
                        sec1 = 59;
                    }
                } else {
                    sec1--;
                }
            }, 1000);
        } else if (timer === "t2" && !isRunning2) {
            isRunning2 = true;
            interval2 = setInterval(() => {
                if (sec2 === 0) {
                    if (mins2 === 0) {
                        stop_timer("t2");
                    } else {
                        mins2--;
                        sec2 = 59;
                    }
                } else {
                    sec2--;
                }
            }, 1000);
        }
    }

    function toggle_timer(timer) {
        if (timer === "t1") {
            if (isRunning1) {
                // Timer 1 is running, stop it and start Timer 2
                stop_timer("t1");
                start_timer("t2");
            } else {
                // Timer 1 is not running, start it and stop Timer 2
                stop_timer("t2");
                start_timer("t1");
            }
        } else if (timer === "t2") {
            if (isRunning2) {
                // Timer 2 is running, stop it and start Timer 1
                stop_timer("t2");
                start_timer("t1");
            } else {
                // Timer 2 is not running, start it and stop Timer 1
                stop_timer("t1");
                start_timer("t2");
            }
        }
    }
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
    <button on:click={() => toggle_timer('t1')} class="cursor-pointer text-5xl m-5">
        {mins1 > 0 ? `${mins1} mins & ${sec1} seconds` : `${sec1} seconds`}
    </button>
    <button on:click={() => toggle_timer('t2')} class="cursor-pointer text-5xl m-5">
        {mins2 > 0 ? `${mins2} mins & ${sec2} seconds` : `${sec2} seconds`}
    </button>
</div>
