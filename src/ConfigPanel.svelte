<script lang="ts">
    import { enumerateTimingFunctions, type TimingFunction } from "./App.svelte";

    import { leadingZero } from "./utils";

    export let duration: number;
    export let setDuration: (duration: number) => void;

    export let decreaseThreshold: number;
    export let setDecreaseThreshold: (duration: number) => void;

    export let start: () => void;

    export let timingFunction: TimingFunction;
    export let setTimingFunction: (tf: TimingFunction) => void;
    
    let realtimeMinutes;
    let realtimeSeconds;
    $: {
        let estimatedRealtimeInSeconds = 0;
        for (let min = duration; min > 0; min--) {
            const scaledSecond = timingFunction.function(min, duration, decreaseThreshold);
            estimatedRealtimeInSeconds += 60 * scaledSecond;
        }

        realtimeMinutes = Math.floor(estimatedRealtimeInSeconds / 60);
        realtimeSeconds = estimatedRealtimeInSeconds % 60;
    }
    
</script>

<div class="full relative center-child">
    <div class="centered-col bg-stone-300 p-6 rounded-lg gap-3 w-1/2 max-w-[600px]">
        <h1 class="text-2xl mb-4">Configuration</h1>
        <label class="self-start">
            Durée :
            <input class="ml-2"
                type=number
                bind:value={duration}
                on:change={(e) => setDuration(duration)}
                min=10
                max=90
            >
            <input class="ml-2 bg-stone-100 rounded-lg appearance-none cursor-pointer h-2"
                type=range
                bind:value={duration}
                on:change={() => setDuration(duration)}
                min=10
                max=90
            >
        </label>
    
        <label class="self-start">
            Seuil de diminution :
            <input class="ml-2"
                type=number
                bind:value={decreaseThreshold}
                on:change={(e) => setDecreaseThreshold(duration)}
                min=5
                max={duration}
            >
            <input class="ml-2 bg-stone-100 rounded-lg appearance-none cursor-pointer h-2"
                type=range
                bind:value={decreaseThreshold}
                on:change={() => setDecreaseThreshold(duration)}
                min=10
                max={duration}
            >
        </label>

        <label class="self-start">
    
            Gestion du temps :
            <select
                class="ml-2"
                bind:value={timingFunction}
                on:change={() =>setTimingFunction(timingFunction)}
            >
                {#each enumerateTimingFunctions() as tf}
                    <option value={tf}>
                        {tf.name}
                    </option>
                {/each}
            </select>
        </label>
    
        <div class="flex-wrap mt-2 italic self-start">
            {timingFunction.description}
        </div>

        <div class="flex-wrap mt-2 italic self-start">
            Durée temps réel : {leadingZero(realtimeMinutes)}:{leadingZero(realtimeSeconds)}
        </div>

        <button
            class="btn mt-4"
            on:click={start}
        >
            Start
        </button>
    </div>

    <div class="w-full absolute left-0 bottom-0 text-stone-100 text-center py-2 bg-stone-800/50">
        ❤️ Réalisé avec amour par <a class="font-bold italic hover:cursor-pointer" href="https://www.plml.fr" target="_blank" rel="noreferrer">Pierre Lemmel</a> ❤️
    </div>
</div>