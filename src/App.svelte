<script context="module" lang="ts">
    export type TimingFunction = (remainingMinutes: number, duration: number) => number;
</script>

<script lang="ts">
    import ConfigPanel from "./ConfigPanel.svelte";
    import Timer from "./Timer.svelte";

    let duration: number = 60;

    let running = false;

    let timingFunction: TimingFunction = () => 1;
</script>


<main class="full center-child bg-stone-900">
    {#if running}
        <Timer
            duration={duration}
            timingFunction={timingFunction}
            stop={() => running = false}
        />
    {:else}
        <ConfigPanel
            duration={duration}
            setDuration={(val) => duration = val}
            setTimingFunction={tf => timingFunction = tf}
            start={() => running = true}
        />
    {/if}
</main>