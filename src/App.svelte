<script context="module" lang="ts">
    export interface TimingFunction {
        readonly name: string;
        readonly function: (remainingMinutes: number, duration: number, decreaseThreshold: number) => number;
        readonly description: string;
    }
    
    type functionTypes = "Homogeneous" | "Linear" | "Accelerated";
    export const timingFunctions: { [k in functionTypes]: TimingFunction } = {
        "Homogeneous": {
            name: "Homogène",
            function: (remainingMinutes, duration, decreaseThreshold) => 1,
            description: "Toutes les minutes affichées font la même durée d'une minute en temps réel"
        },
        "Linear": {
            name: "Linéaire",
            function: (remainingMinutes, duration, decreaseThreshold) => (remainingMinutes < decreaseThreshold ? 55 : 60) / 60,
            description: "A partir du seuil de diminution, toutes les minutes affichées durent 55 secondes en temps réel"
        },
        "Accelerated": {
            name: "Accélérée",
            function: (remainingMinutes, duration, decreaseThreshold) => {

                if (remainingMinutes < decreaseThreshold) {
                    const a = remainingMinutes / decreaseThreshold;
                    console.log({a, remainingMinutes, duration, decreaseThreshold})
                    return (a * 60 + (1-a) * 40) / 60;
                }
                else {
                    return 1;
                }
                
            },
            description: "A partir du seuil de diminution, toutes les minutes sont de plus en plus courte jusqu'à un minimum de 30 secondes"
        },
    };

    export const enumerateTimingFunctions = () => Object.values(timingFunctions);
</script>

<script lang="ts">
    import ConfigPanel from "./ConfigPanel.svelte";
    import Timer from "./Timer.svelte";

    let duration: number = 60;
    let decreaseThreshold: number = 30;

    let running = false;

    let timingFunction: TimingFunction = timingFunctions["Homogeneous"];
</script>


<main class="full center-child bg-stone-900">
    {#if running}
        <Timer
            {duration} {decreaseThreshold}
            {timingFunction}
            stop={() => running = false}
        />
    {:else}
        <ConfigPanel
            {duration} {decreaseThreshold} {timingFunction}
            setDuration={(val) => duration = val}
            setDecreaseThreshold={(val) => decreaseThreshold = val}
            setTimingFunction={tf => timingFunction = tf}
            start={() => running = true}
        />
    {/if}
</main>