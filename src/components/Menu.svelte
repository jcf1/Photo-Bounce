<script lang="ts">
    import { Tabs, TabItem } from 'flowbite-svelte';
    import BackgroundSettings from './settings/BackgroundSettings.svelte';
    import BounceSettings from './settings/BounceSettings.svelte';
    import AdvancedSettings from './settings/AdvancedSettings.svelte';

    // Background Settings
    export let bgPhotos: FileList;
    export let bgImageSize: string;
    export let bgColor: string;
    export let bgInterval: number;

    // Foreground (or Bounce) Settings
    export let fgPhotos: FileList;
    export let fgImageSize: string;
    export let fgColor: string;
    export let fgInterval: number;

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
        />
    </TabItem>
    <TabItem>
        <span slot="title">Advanced</span>
        <AdvancedSettings/>
    </TabItem>
</Tabs>

