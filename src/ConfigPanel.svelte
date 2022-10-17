<script lang="ts">
    export let duration: number;
    export let setDuration: (duration: number) => void;
    export let onStart: () => void;;
</script>

<div class="centered-col bg-stone-300 p-6 rounded gap-4">
    <label class="">
        Durée
        <input class="ml-2" type=number bind:value={duration} on:change={(e) => setDuration(duration)} min=10 max=90>
        <input class="ml-2 bg-stone-100 rounded-lg appearance-none cursor-pointer h-2" type=range bind:value={duration} on:change={() => setDuration(duration)} min=10 max=90>
    </label>
    <button
        class="btn"
        on:click={onStart}
    >
        Start
    </button>
</div>