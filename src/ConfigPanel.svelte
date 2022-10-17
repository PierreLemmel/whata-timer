<script context="module" lang="ts">
    interface TimingFunctionDropDownElement {
        readonly name: string;
        readonly timingFunction: TimingFunction;
        readonly description: string;
    }
    
    const timingFunctions: TimingFunctionDropDownElement[] = [
        {
            name: "Homogène",
            timingFunction: (remainingMinutes, duration) => 1,
            description: "Toutes les minutes affichées font la même durée d'une minute en temps réel"
        },
        {
            name: "Linéaire",
            timingFunction: (remainingMinutes, duration) => (remainingMinutes < duration / 2 ? 55 : 60) / 60,
            description: "A partir de la moitié du temps, toutes les minutes affichées durent 55 secondes en temps réel"
        },
        {
            name: "Accélérée",
            timingFunction: (remainingMinutes, duration) => {

                const halfDuration = duration / 2;

                if (remainingMinutes < halfDuration) {
                    const a = remainingMinutes / halfDuration;
                    return (a * 60 + (1-a) * 40) / 60;
                }
                else {
                    return 1;
                }
                
            },
            description: "A partir de la moitié du temps, toutes les minutes sont de plus en plus courte jusqu'à un minimum de 30 secondes"
        },
    ];
</script>

<script lang="ts">
    import type { TimingFunction } from "./App.svelte";

    export let duration: number;
    export let setDuration: (duration: number) => void;
    export let start: () => void;
    export let setTimingFunction: (tf: TimingFunction) => void;
    
    let currentTimingFunction: TimingFunctionDropDownElement = timingFunctions[0];
</script>

<div class="full relative center-child">
    <div class="centered-col bg-stone-300 p-6 rounded-lg gap-3 w-1/2 max-w-[600px]">
        <h1 class="text-2xl mb-4">Configuration</h1>
        <label class="self-start">
            Durée :
            <input class="ml-2" type=number bind:value={duration} on:change={(e) => setDuration(duration)} min=10 max=90>
            <input class="ml-2 bg-stone-100 rounded-lg appearance-none cursor-pointer h-2" type=range bind:value={duration} on:change={() => setDuration(duration)} min=10 max=90>
        </label>
    
        <label class="self-start">
    
            Gestion du temps :
            <select class="ml-2" bind:value={currentTimingFunction} on:change={() =>
                setTimingFunction(currentTimingFunction.timingFunction)}>
                {#each timingFunctions as tf}
                    <option value={tf}>
                        {tf.name}
                    </option>
                {/each}
            </select>
        </label>
    
        <div class="flex-wrap mt-2 italic self-start">
            {currentTimingFunction.description}
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