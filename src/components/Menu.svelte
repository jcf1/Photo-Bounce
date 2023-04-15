<script lang="ts">
    import { Tabs, TabItem } from 'flowbite-svelte';
    import BackgroundSettings from './settings/BackgroundSettings.svelte';
    import BounceSettings from './settings/BounceSettings.svelte';
    import AdvancedSettings from './settings/AdvancedSettings.svelte';

    // Background Settings
    export let bgPhotos: File[];
    export let bgImageSize: string;
    export let bgColor: string;
    export let bgInterval: number;

    // Foreground (or Bounce) Settings
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
  
<Tabs style="pill">
    <TabItem open>
        <span slot="title">Background</span>
        <BackgroundSettings
            bind:bgPhotos={bgPhotos}
            bind:bgImageSize={bgImageSize}
            bind:bgColor={bgColor}
            bind:bgInterval={bgInterval}
        />
    </TabItem>
    <TabItem>
        <span slot="title">Bounce</span>
        <BounceSettings
            bind:fgPhotos={fgPhotos}
            bind:fgImageSize={fgImageSize}
            bind:fgColor={fgColor}
            bind:fgInterval={fgInterval}
            bind:sizeMulti={sizeMulti}
            bind:speedMulti={speedMulti}
        />
    </TabItem>
    <TabItem>
        <span slot="title">Advanced</span>
        <AdvancedSettings
            bind:startX={startX}
            bind:startY={startY}
            bind:startDir={startDir}
            bind:bounceAngle={bounceAngle}
        />
    </TabItem>
</Tabs>

