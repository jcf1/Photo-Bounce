<script lang="ts">
    export let startX: number;
    export let startY: number;
    export let startDir: number;
    export let bounceAngle: number;

    let xValue = startX;
    let yValue = 100 - startY;
    let startValue = 360 - startDir;
    let bounceValue = 180 - bounceAngle;
    
    $: startX = xValue;
    $: startY = 100 - yValue;
    $: startDir = 360 - startValue;
    $: bounceAngle = 180 - bounceValue;

</script>

<div class="flex flex-col justify-center items-center pt-8">
    <div class="justify-center text-2xl w-2/3 pt-4">
        <span>Start bounce image at {xValue}% of screen width:</span>
        <input class="w-full p-0" type="range" max="100" min="0" step="1" bind:value={xValue}/>
    </div>

    <div class="justify-center text-2xl w-2/3 pt-4">
        <span>Start bounce image at {yValue}% of screen height:</span>
        <input class="w-full p-0" type="range" max="100" min="0" step="1" bind:value={yValue}/>
    </div>

    <div class="justify-center text-2xl w-2/3 pt-4">
        <span>Start bounce image at direction of {startValue}&#176;:</span>
        <input class="w-full p-0" type="range" max="359" min="0" step="1" bind:value={startValue}/>
    </div>

    <div class="justify-center text-2xl w-2/3 pt-4">
        <span>Bounce angle on wall collisions is {bounceValue}&#176;:</span>
        <input class="w-full p-0" type="range" max="150" min="30" step="1" bind:value={bounceValue}/>
    </div>
</div>
