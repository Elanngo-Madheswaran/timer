<script>
    import Timer from './Timer.svelte';
    import { onMount, onDestroy } from 'svelte';

    let time = 2;
    let isRunning1 = false;
    let isRunning2 = false;
    let ups = "";

    let timer1Time = time;
    let timer2Time = time;

    function set_time(t) {
        time = t;
        timer1Time = t;
        timer2Time = t;

        isRunning1 = false;
        isRunning2 = false;
    }

    function toggle_timer(timer) {
        if (timer === 't1') {
            if (isRunning1) {
                isRunning1 = false;
                isRunning2 = true;
            } else {
                isRunning1 = true;
                isRunning2 = false;
            }
        } else if (timer === 't2') {
            if (isRunning2) {
                isRunning2 = false;
                isRunning1 = true;
            } else {
                isRunning2 = true;
                isRunning1 = false;
            }
        }
    }

    function toggle_ups() {
        ups = (ups === "") ? "upside-down" : "";
    }

    function pause_clocks() {
        isRunning1 = false;
        isRunning2 = false;
    }

    function handleKeydown(event) {
        if (event.code === 'Space') {
            event.preventDefault();
            if (isRunning1 || isRunning2) {
                if (isRunning1) {
                    toggle_timer('t1');
                } else if (isRunning2) {
                    toggle_timer('t2');
                }
            } else {
                isRunning1 = true;
                isRunning2 = false;
            }
        }
    }

    onMount(() => {
		if (typeof window !== 'undefined') {
			window.addEventListener('keydown', handleKeydown);
		}
    });

    onDestroy(() => {
		if (typeof window !== 'undefined') {
       		window.removeEventListener('keydown', handleKeydown);
		}
    });
</script>

<svelte:head>
    <title>Home</title>
    <meta name="description" content="Svelte chess clock timer" />
</svelte:head>

<div class="flex justify-center flex-col p-5">
	
	<Timer initialTime={timer1Time} isActive={isRunning1} cla={ups} on:toggle={() => toggle_timer('t1')} />
	<hr class="bg-black h-2">
	<Timer initialTime={timer2Time} isActive={isRunning2} on:toggle={() => toggle_timer('t2')} />
			
	<button on:click={pause_clocks} class="w-24 bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5">Pause Both</button>
	<button on:click={toggle_ups} class="w-auto bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5">Upside Down Timer</button>
	<div class="flex self-center justify-center flex-wrap">
		{#each [5, 10, 15] as t}
			<button class="w-24 bg-slate-500 border-2 border-blue-900 text-white rounded self-center m-5" on:click={() => set_time(t)}>{t} min</button>
		{/each}
	</div>
</div>
