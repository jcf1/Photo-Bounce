<script lang="ts">
    import PhotoBounce from "../components/PhotoBounce.svelte";

    let photobounce: PhotoBounce;

    let bgFileInput: HTMLInputElement;
    let bgPhotos: FileList;
    let bgInterval: number = 10;

    function uploadBackgroundPhotos() {
        bgFileInput.click();
        bgPhotos = bgPhotos;
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
    />
    <input
        class="hidden"
        accept="image/png, image/jpeg, image/jpg"
        multiple
        type="file"
        bind:this={bgFileInput}
        bind:files={bgPhotos}
    />
    <div>
        <button on:click={() => {uploadBackgroundPhotos()}}>Upload Background Images</button>
        <div class="text-lg">{bgPhotos ? bgPhotos.length : 0} Selected</div>
    </div>
    <div>
        <input
            type="number"
            max="600"
            min="1"
            bind:value={bgInterval}
        />
        <div>sec</div>
    </div>

    <button on:click={()=> {photobounce.requestFullscreen();}} type=submit>Fullscreen</button>
</div>
