<script lang="ts">
    import PhotoBounce from "../components/PhotoBounce.svelte";
    import Menu from "../components/Menu.svelte";

    let photobounce: PhotoBounce;

    // Background Setings
    let bgPhotos: File[];
    let bgImageSize: string;
    let bgColor: string;
    let bgInterval: number;

    // Foreground (or Bounce) Settings
    let fgPhotos: FileList;
    let fgImageSize: string;
    let fgColor: string;
    let fgInterval: number;

    function handleKeydown(event: KeyboardEvent) {
		if(event.code == "Space") {
            photobounce.exitFullscreen();
        }
	}
</script>

<svelte:window on:keydown={(event) => handleKeydown(event)}/>
<div class="grid gap-1 justify-center justify-items-center items-center h-screen w-screen">
    <div class="text-6xl">Photo Bounce</div>
    <PhotoBounce 
        bind:this={photobounce}
        initWidth={64}
        initHeight={36}

        {bgPhotos}
        {bgImageSize}
        {bgColor}
        {bgInterval}

        {fgPhotos}
        {fgImageSize}
        {fgColor}
        {fgInterval}
    />

    <Menu
        bind:bgPhotos={bgPhotos}
        bind:bgImageSize={bgImageSize}
        bind:bgColor={bgColor}
        bind:bgInterval={bgInterval}

        bind:fgPhotos={fgPhotos}
        bind:fgImageSize={fgImageSize}
        bind:fgColor={fgColor}
        bind:fgInterval={fgInterval}
    />

    <button class="bg-red-600" on:click={()=> {photobounce.requestFullscreen();}} type=submit>Fullscreen</button>
</div>
