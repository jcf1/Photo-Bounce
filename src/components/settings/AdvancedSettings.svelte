<script lang="ts">
    export let startX: number = 50;
    export let startY: number = 50;
    export let startDir: number = 60;
    export let bounceAngle: number = 90;
</script>

<div class="justify-center">

    <div class="flex flex-row justify-center">
        <label class="pr-1 text-lg">
            Start bounce image at {startX}% width:
            <input class="border border-black" type="range" max="100" min="0" bind:value={startX}/>
        </label>
    </div>

    <div class="flex flex-row justify-center">
        <label class="pr-1 text-lg">
            Start bounce image at {startY}% height:
            <input class="border border-black" type="range" max="100" min="0" bind:value={startY}/>
        </label>
    </div>

    <div class="flex flex-row justify-center">
        <label class="pr-1 text-lg">
            Start Bounce direction at {startDir}&#176;:
            <input class="border border-black" type="range" max="360" min="1" bind:value={startDir}/>
        </label>
    </div>

    <div class="flex flex-row justify-center">
        <label class="pr-1 text-lg">
            Bounce angle on wall collisions is {bounceAngle}&#176;:
            <input class="border border-black" type="range" max="180" min="0" bind:value={bounceAngle}/>
        </label>
    </div>
</div>
