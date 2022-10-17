<script lang="ts">
    import { onMount } from "svelte";


    export let duration: number;
    export let timingFunction: (second: number) => number;

    let displayMinutes = 60;
    let displaySeconds = 0;

    let displayString = "UNINITIALIZED";

    const leadingZero = (nb: number) => nb < 10 ? "0" + nb : nb.toString();

    onMount(() => {

        let remainingSeconds = 60 * duration;

        let timeout = null;
        
        const startTimer = () => {

            displayMinutes = Math.floor(remainingSeconds / 60);
            displaySeconds = remainingSeconds % 60;

            displayString = `${leadingZero(displayMinutes)}:${leadingZero(displaySeconds)}`.replaceAll("1", " 1");

            timeout = remainingSeconds-- > 0 ? setTimeout(startTimer, 1_000) : null;
        }

        startTimer();

        return () => {
            if (timeout) {
                clearTimeout(timeout);
            }
        }
    });
</script>

<div class="center-child font-digi text-white text-[18rem] {displayString.startsWith(" 1") ? "ml-[4rem]" : ""}">
    {displayString}
</div>
