<script lang="ts">
    import PhotoBounce from "../components/PhotoBounce.svelte";
    import Menu from "../components/Menu.svelte";

    let photobounce: PhotoBounce;

    // Background Setings
    let bgPhotos: File[] = [];
    let bgImageSize: string = "cover";
    let bgColor: string =  "#000000";
    let bgInterval: number = 10;

    // Foreground (or Bounce) Settings
    let fgPhotos: File[] = [];
    let fgImageSize: string = "contain";
    let fgColor: string = "#00000000";
    let fgInterval: number = 0;
    let sizeMulti: number = 0.4;
    let speedMulti: number = 0.001;

    // Advanced Settings
    let startX: number = 50;
    let startY: number = 50;
    let startDir: number = 300;
    let bounceAngle: number = 90;

    function clearBGPhotos(): void {
        bgPhotos = [];
        photobounce.reset();
    }

    function clearFGPhotos(): void {
        fgPhotos = [];
        photobounce.reset();
    }

    function handleKeydown(event: KeyboardEvent): void {
		if(event.code == "Space") {
            photobounce.exitFullscreen();
        }
	}
</script>

<svelte:window on:keydown={(event) => handleKeydown(event)}/>
<div class="grid gap-1 justify-center justify-items-center items-center h-screen w-screen bg-slate-600">
    <PhotoBounce 
        bind:this={photobounce}
        initWidth={40}
        initHeight={40}

        {bgPhotos}
        {bgImageSize}
        {bgColor}
        {bgInterval}

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

    <div class="flex items-center justify-center" style={`width:${60}vw;height:${20}vw;`}>
        <Menu
            {clearBGPhotos}
            bind:bgPhotos={bgPhotos}
            bind:bgImageSize={bgImageSize}
            bind:bgColor={bgColor}
            bind:bgInterval={bgInterval}

            {clearFGPhotos}
            bind:fgPhotos={fgPhotos}
            bind:fgImageSize={fgImageSize}
            bind:fgColor={fgColor}
            bind:fgInterval={fgInterval}
            bind:sizeMulti={sizeMulti}
            bind:speedMulti={speedMulti}

            bind:startX={startX}
            bind:startY={startY}
            bind:startDir={startDir}
            bind:bounceAngle={bounceAngle}
        />
    </div>

    <div class="table justify-center text-2xl text-white pt-16">
        <button class="table-cell bg-green-600 mr-16" on:click={photobounce.requestFullscreen} type=submit>Fullscreen</button>
        <button class="table-cell bg-green-600" on:click={photobounce.reset} type=submit>Restart</button>
    </div>
</div>
