<script lang="ts">
    // @ts-ignore
    import { Tabs,Tab } from 'svelte-chota';
    import 'chota';
    import BackgroundSettings from './settings/BackgroundSettings.svelte';
    import BounceSettings from './settings/BounceSettings.svelte';
    import AdvancedSettings from './settings/AdvancedSettings.svelte';

    let active: string = "background";

    // Background Settings
    export let clearBGPhotos: () => void;
    export let bgPhotos: File[];
    export let bgImageSize: string;
    export let bgColor: string;
    export let bgInterval: number;

    // Foreground (or Bounce) Settings
    export let clearFGPhotos: () => void;
    export let fgPhotos: File[];
    export let fgImageSize: string;
    export let fgColor: string;
    export let fgInterval: number;
    export let sizeMulti: number;
    export let speedMulti: number;

    // Advanced Settings
    export let startX: number;
    export let startY: number;
    export let startDir: number;
    export let bounceAngle: number;
</script>

<div class="w-full h-full bg-slate-200">
    <Tabs full bind:active={active}>
        <Tab tabid="background">Background</Tab>
        <Tab tabid="bounce">Bounce</Tab>
        <Tab tabid="advanced">Advanced</Tab>
    </Tabs>
    <div class="w-full h-full overflow-y-auto bg-slate-400">
        {#if active === "background"}
            <BackgroundSettings
                {clearBGPhotos}
                bind:bgPhotos={bgPhotos}
                bind:bgImageSize={bgImageSize}
                bind:bgColor={bgColor}
                bind:bgInterval={bgInterval}
            />
        {:else if active === "bounce"}
            <BounceSettings
                {clearFGPhotos}
                bind:fgPhotos={fgPhotos}
                bind:fgImageSize={fgImageSize}
                bind:fgColor={fgColor}
                bind:fgInterval={fgInterval}
                bind:sizeMulti={sizeMulti}
                bind:speedMulti={speedMulti}
            />
        {:else}
            <AdvancedSettings
                bind:startX={startX}
                bind:startY={startY}
                bind:startDir={startDir}
                bind:bounceAngle={bounceAngle}
            />
        {/if}
    </div>
</div>

<style global>
  span.active {
    background-color: white;
  }
</style>

