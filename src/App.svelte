<script>
    import ConfigPanel from "./ConfigPanel.svelte";
    import Timer from "./Timer.svelte";

    let duration = 60;

    let running = false;
</script>


<main class="full center-child bg-stone-900">
    {#if running}
        <Timer
            duration={duration}
            timingFunction={nb => nb}
        />
    {:else}
        <ConfigPanel
            duration={duration}
            setDuration={(val) => duration = val}
            onStart={() => running = true}
        />
    {/if}
</main>