<script lang="ts">
    import { onMount, onDestroy, createEventDispatcher } from "svelte";
    import screenfull from "screenfull";
    import Bounce from "./Bounce.svelte";

    // Variables to make PhotoBounce fullscreen
    let component: HTMLElement;
    const dispatch = createEventDispatcher<{ change: never; error: never }>();

    export let initWidth: number = 25;
    export let initHeight: number = 25;

    export let bgPhotos: File[];
    export let bgImageSize: String = "cover";
    export let bgColor: String = "#000000";
    export let bgInterval: number = 10;

    export let fgPhotos: FileList;
    export let fgImageSize: String = "contain";
    export let fgColor: String = "#FFFFFF";
    export let fgInterval: number = 0;
    export let sizeMulti: number = 1.0;
    export let speedMulti: number = 0.0005;

    export let startX: number = 50;
    export let startY: number = 50;
    export let startDir: number = 315;
    export let bounceAngle: number = 90;

    let width: number;
    let height: number;
    let bgIndex = 0;

    let clear: number;
    $: if(bgPhotos && bgInterval > 0) {
        clearInterval(clear)
        clear = setInterval(nextBGPhoto, bgInterval * 1000)
    } else {
        clearInterval(clear)
    }

    onMount(() => {
      if (screenfull.isEnabled) {
        screenfull.on("change", () => dispatch("change"));
        screenfull.on("error", () => dispatch("error"));
      }
    });

    onDestroy(() => {
      if (screenfull.isEnabled) {
        screenfull.off("change", () => true);
        screenfull.off("error", () => true);
      }
    });

    export function requestFullscreen() {
        if (screenfull.isEnabled && component?.nextElementSibling) {
            screenfull.request(component.nextElementSibling);
        }
    };

    export function exitFullscreen() {
        if (screenfull.isEnabled && component?.nextElementSibling) {
            screenfull.exit();
        }
    }

    function nextBGPhoto(): void {
        bgIndex = (bgIndex + 1) % bgPhotos.length;
    }
</script>

<div style="width:0; height:0" bind:this={component}/>
<div class="flex relative items-center justify-center" style={`width:${initWidth}vw;height:${initHeight}vh;background-color:${bgColor};`} bind:clientWidth={width} bind:clientHeight={height}>
    {#if bgPhotos && bgPhotos.length > 0}
        <img class="w-full h-full" style={`object-fit:${bgImageSize};`} src={URL.createObjectURL(bgPhotos[bgIndex])} alt=""/>
    {/if}
    <Bounce
        parentWidth={width}
        parentHeight={height}

        {fgPhotos}
        {fgImageSize}
        {fgColor}
        {fgInterval}
        {sizeMulti}
        {speedMulti}

        {startX}
        {startY}
        {startDir}
        {bounceAngle}
    />
</div>
