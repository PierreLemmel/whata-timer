<script context="module" lang="ts">
    export type TimingFunction = (remainingMinutes: number, duration: number, decreaseThreshold: number) => number;
</script>

<script lang="ts">
    import ConfigPanel from "./ConfigPanel.svelte";
    import Timer from "./Timer.svelte";

    let duration: number = 60;
    let decreaseThreshold: number = 30;

    let running = false;

    let timingFunction: TimingFunction = () => 1;
</script>


<main class="full center-child bg-stone-900">
    {#if running}
        <Timer
            {duration}
            {decreaseThreshold}
            {timingFunction}
            stop={() => running = false}
        />
    {:else}
        <ConfigPanel
            {duration} {decreaseThreshold}
            setDuration={(val) => duration = val}
            setDecreaseThreshold={(val) => decreaseThreshold = val}
            setTimingFunction={tf => timingFunction = tf}
            start={() => running = true}
        />
    {/if}
</main>