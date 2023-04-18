<script lang="ts">
    import { onMount, onDestroy, createEventDispatcher } from "svelte";
    import { resize } from 'svelte-resize-observer-action';
    import screenfull from "screenfull";
    import Bounce from "./Bounce.svelte";

    // Variables to make PhotoBounce fullscreen
    let component: HTMLElement;
    const dispatch = createEventDispatcher<{ change: never; error: never }>();

    let bounceComponent: Bounce;

    export let initWidth: number;
    export let initHeight: number;

    export let bgPhotos: File[];
    export let bgImageSize: String;
    export let bgColor: String;
    export let bgInterval: number;

    export let fgPhotos: File[];
    export let fgImageSize: String;
    export let fgColor: String;
    export let fgInterval: number;
    export let sizeMulti: number;
    export let speedMulti: number;

    export let startX: number;
    export let startY: number;
    export let startDir: number;
    export let bounceAngle: number;

    let width: number;
    let height: number;
    let bgIndex = 0;

    let clear: number;

    onMount(() => {
        if (screenfull.isEnabled) {
            screenfull.on("change", () => dispatch("change"));
            screenfull.on("error", () => dispatch("error"));
        }
    });

    export function reset() {
        bounceComponent.reset();
        bgIndex = 0;
        if(bgPhotos && bgInterval > 0) {
            clearInterval(clear);
            clear = setInterval(nextBGPhoto, bgInterval * 1000);
        } else {
            clearInterval(clear);
        }
    }

    function nextBGPhoto(): void {
        if(bgPhotos && bgPhotos.length > 0) {
            bgIndex = (bgIndex + 1) % bgPhotos.length;
        }
    }

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
</script>

<div style="width:0; height:0" bind:this={component}/>
<div class="flex relative items-center justify-center" style={`width:${initWidth}vw;height:${initHeight}vh;background-color:${bgColor};`} bind:clientWidth={width} bind:clientHeight={height} use:resize={reset}>
    {#if bgPhotos && bgPhotos.length > 0}
        <img class="w-full h-full" style={`object-fit:${bgImageSize};`} src={URL.createObjectURL(bgPhotos[bgIndex])} alt=""/>
    {/if}
    <Bounce
        bind:this={bounceComponent}

        nextBGPhoto={(bgPhotos && bgPhotos.length > 0 && bgInterval === 0) ? nextBGPhoto : () => {}}

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
