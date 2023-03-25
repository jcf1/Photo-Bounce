<script lang="ts">
    import PhotoBounce from "../components/PhotoBounce.svelte";

    let photobounce: PhotoBounce;

    let bgFileInput: HTMLInputElement;
    let bgPhotos: FileList;
    let bgInterval: number = 10;

    let fgFileInput: HTMLInputElement;
    let fgPhotos: FileList;
    let fgInterval: number = 10;

    function uploadBackgroundPhotos() {
        bgFileInput.click();
        bgPhotos = bgPhotos;
    }

    function uploadForegroundPhotos() {
        fgFileInput.click();
        fgPhotos = fgPhotos;
    }

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
        width={64}
        height={36}
        bgColor={"black"}
        {bgPhotos}
        {bgInterval}
        fgColor={"black"}
        {fgPhotos}
        {fgInterval}
    />
    <div class="justify-items-center">
        <input class="hidden" accept="image/png, image/jpeg, image/jpg" multiple type="file" bind:this={bgFileInput} bind:files={bgPhotos}/>
        <div class="float-left pr-4">
            <button class="bg-red-600" on:click={() => {uploadBackgroundPhotos()}}>Upload Background Images</button>
        </div>
        <div class="text-lg float-left pl-4">{bgPhotos ? bgPhotos.length : 0} Selected</div>
    </div>
    <div class="justify-items-center">
        <div class="float-left pr-1">
            <input class="border border-black" type="number" max="600" min="1" bind:value={bgInterval}/>
        </div>
        <div class="text-lg float-left pl-1">seconds</div>
    </div>
    
    <div class="justify-items-center">
        <input class="hidden" accept="image/png, image/jpeg, image/jpg" multiple type="file" bind:this={fgFileInput} bind:files={fgPhotos}/>
        <div class="float-left pr-4">
            <button class="bg-red-600" on:click={() => {uploadForegroundPhotos()}}>Upload Foreground Images</button>
        </div>
        <div class="text-lg float-left pl-4">{fgPhotos ? fgPhotos.length : 0} Selected</div>
    </div>
    <div class="justify-items-center">
        <div class="float-left pr-1">
            <input class="border border-black" type="number" max="600" min="1" bind:value={fgInterval}/>
        </div>
        <div class="text-lg float-left pl-1">seconds</div>
    </div>

    <button class="bg-red-600" on:click={()=> {photobounce.requestFullscreen();}} type=submit>Fullscreen</button>
</div>
