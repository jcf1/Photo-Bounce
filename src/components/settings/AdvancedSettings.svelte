<script lang="ts">
    export let startX: number;
    export let startY: number;
    export let startDir: number;
    export let bounceAngle: number;

    let xValue = startX;
    let yValue = 100 - startY;
    let startValue = 360 - startDir;
    
    $: startX = xValue;
    $: startY = 100 - yValue;
    $: startDir = 360 - startValue;
</script>

<div class="justify-center">

    <div class="flex flex-row justify-center">
        <label class="pr-1 text-lg">
            Start bounce image at {xValue}% width:
            <input class="border border-black" type="range" max="100" min="0" bind:value={xValue}/>
        </label>
    </div>

    <div class="flex flex-row justify-center">
        <label class="pr-1 text-lg">
            Start bounce image at {yValue}% height:
            <input class="border border-black" type="range" max="100" min="0" bind:value={yValue}/>
        </label>
    </div>

    <div class="flex flex-row justify-center">
        <label class="pr-1 text-lg">
            Start Bounce direction at {startValue}&#176;:
            <input class="border border-black" type="range" max="359" min="0" bind:value={startValue}/>
        </label>
    </div>

    <div class="flex flex-row justify-center">
        <label class="pr-1 text-lg">
            Bounce angle on wall collisions is {bounceAngle}&#176;:
            <input class="border border-black" type="range" max="150" min="30" bind:value={bounceAngle}/>
        </label>
    </div>
</div>
