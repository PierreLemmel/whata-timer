<script lang="ts">
    import { onMount } from "svelte";
    import type { TimingFunction } from "./App.svelte";

    export let duration: number;
    export let timingFunction: TimingFunction;
    export let stop: () => void;

    let displayMinutes = 60;
    let displaySeconds = 0;

    let displayString = "UNINITIALIZED";

    const leadingZero = (nb: number) => nb < 10 ? "0" + nb : nb.toString();

    onMount(() => {

        let remainingSeconds = 60 * duration;

        let timeout = null;
        
        const startTimer = () => {

            const remainingMinutes = Math.floor(remainingSeconds / 60);
            displayMinutes = remainingMinutes;
            displaySeconds = remainingSeconds % 60;

            displayString = `${leadingZero(displayMinutes)}:${leadingZero(displaySeconds)}`.replaceAll("1", " 1");

            if (remainingSeconds-- > 0) {

                const nextDisplaySecondDuration = timingFunction(remainingMinutes, duration);
                timeout = setTimeout(startTimer, 1_000 * nextDisplaySecondDuration);
            }
            else {
                timeout = null;
            }
        }

        startTimer();

        return () => {
            if (timeout) {
                clearTimeout(timeout);
            }
        }
    });
</script>

<svelte:window
    on:keydown={e => {
        if (e.key === 'Escape') {
            stop();
        }
    }}
/>

<div class="full relative">
    <div class="full center-child font-digi text-white text-[18rem] {displayString.startsWith(" 1") ? "ml-[4rem]" : ""}">
        {displayString}
    </div>

    <button
        class="w-28 h-28 absolute top-0 right-0 center-child text-8xl opacity-0 bg-stone-700 text-white hover:opacity-100 transition-all duration-300 hover:cursor-pointer font-thin"
        on:click={() => stop()}
    >
        X
    </button>
</div>
