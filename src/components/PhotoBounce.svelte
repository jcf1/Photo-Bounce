<script lang="ts">
    import { onMount, onDestroy, createEventDispatcher } from "svelte";
    import screenfull from "screenfull";
    import Bounce from "./Bounce.svelte";

    // Variables to make PhotoBounce fullscreen
    let component: HTMLElement;
    const dispatch = createEventDispatcher<{ change: never; error: never }>();

    export let initWidth: number;
    export let initHeight: number;

    export let bgPhotos: FileList;
    export let bgImageSize: String;
    export let bgColor: String;
    export let bgInterval: number;

    export let fgPhotos: FileList;
    export let fgImageSize: String;
    export let fgColor: String;
    export let fgTransparent: boolean;
    export let fgInterval: number;

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
<div class="flex relative items-center justify-center" style={`width:${initWidth}vh;height:${initHeight}vh;background-color:${bgColor};`} bind:clientWidth={width} bind:clientHeight={height}>
    {#if bgPhotos}
        <img class="w-full h-full" style={`object-fit:${bgImageSize};`} src={URL.createObjectURL(bgPhotos[bgIndex])} alt=""/>
    {/if}
    <Bounce
        parentWidth={width}
        parentHeight={height}
        {fgPhotos}
        {fgImageSize}
        {fgColor}
        {fgTransparent}
        {fgInterval}
    />
</div>
