<script lang="ts">
    import type { TimingFunction } from "./App.svelte";
    import { leadingZero } from "./utils";

    export let duration: number;
    export let decreaseThreshold: number;
    export let timingFunction: TimingFunction;
    export let stop: () => void;

    let displayMinutes = 60;
    let displaySeconds = 0;

    
    const formatTime = (minutes: number, seconds: number) => `${leadingZero(minutes)}:${leadingZero(seconds)}`.replaceAll("1", " 1");
    let displayString = formatTime(displayMinutes, displaySeconds);

    let timeout: NodeJS.Timeout | null = null;
    let playing = false;

    const start = () => {

        stopTimer();
        let remainingSeconds = 60 * duration;

        const startTimer = () => {

            playing = true;
            const remainingMinutes = Math.floor(remainingSeconds / 60);
            displayMinutes = remainingMinutes;
            displaySeconds = remainingSeconds % 60;

            displayString = formatTime(displayMinutes, displaySeconds);

            if (remainingSeconds-- > 0) {

                const nextScaledSecondDuration = timingFunction.function(remainingMinutes, duration, decreaseThreshold);
                timeout = setTimeout(startTimer, 1_000 * nextScaledSecondDuration);
            }
            else {
                timeout = null;
            }
        }

        startTimer();

    }

    const stopTimer = () => {
        if (timeout !== null) {
            clearTimeout(timeout);
            timeout = null;
            playing = false;
        }
    }
</script>

<svelte:window
    on:keydown={e => {
        if (e.key === 'Escape') {
            stop();
        }
    }}
/>

<div class="full relative">
    <div class="full flex flex-col items-center justify-center font-digi text-white">
        <div class="text-[18rem] {displayString.startsWith(" 1") ? "ml-[4rem]" : ""}">{displayString}</div>
        <button class="opacity-0 bg-stone-700 text-white hover:opacity-100 transition-all duration-300 hover:cursor-pointer min-w-[50%] p-6 text-center flex flex-col items-center justify-center gap-2"
            on:click={() => playing ? stopTimer() : start()}>
            {#if playing}
                Arreter
            {:else}
                Demarrer
            {/if}
        </button>
    </div>

    <button
        class="w-28 h-28 absolute top-0 right-0 center-child text-8xl opacity-0 bg-stone-700 text-white hover:opacity-100 transition-all duration-300 hover:cursor-pointer font-thin"
        on:click={() => stop()}
    >
        X
    </button>
</div>
