<script lang="ts">
    import PhotoBounce from "../components/PhotoBounce.svelte";

    let photobounce: PhotoBounce;

    let bgFileInput: HTMLInputElement;
    let bgPhotos: FileList;
    let bgImageSize: string = "cover";
    let bgColor: string = "#000000";

    let bgInterval: number = 10;
    let bgBounce: boolean = false;

    let fgFileInput: HTMLInputElement;
    let fgPhotos: FileList;

    let fgImageSize: string = "contain";
    let fgColor: string = "#FFFFFF";
    let fgTransparent: boolean = true;

    let fgInterval: number = 10;
    let fgBounce: boolean = true;

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
        initWidth={64}
        initHeight={36}

        {bgPhotos}
        {bgImageSize}
        {bgColor}
        bgInterval={bgBounce ? 0 : bgInterval}

        {fgPhotos}
        {fgImageSize}
        fgColor = {fgTransparent ? "#00000000" : fgColor}
        {fgTransparent}
        fgInterval = {fgBounce ? 0 : fgInterval}
    />

    <input class="hidden" accept="image/png, image/jpeg, image/jpg" multiple type="file" bind:this={bgFileInput} bind:files={bgPhotos}/>
    <label class="float-left text-lg ">
        <button class="bg-red-600" on:click={() => {uploadBackgroundPhotos()}}>Upload Background Images</button>
        {bgPhotos ? bgPhotos.length : 0} Selected
    </label>
    <div class="justify-items-center">
        <div class="text-lg float-left pr-2">Change Background Image:</div>
        {#if !bgBounce}
            <label class="float-left pr-1 text-lg">
                <input class="border border-black" type="range" max="600" min="1" bind:value={bgInterval}/>
                {bgInterval} seconds
            </label>
        {/if}
        <label class="text-lg float-left pl-1">
            <input class="float-left" type="checkbox" bind:checked={bgBounce}/>
            On Bounce
        </label>
    </div>
    <div class="justify-items-center">
        <div class="text-lg float-left pr-2">Display Background Image:</div>
        <label class="float-left pr-2">
            <input class="border border-black" type=radio value={"cover"} bind:group={bgImageSize}/>
            Fill Screen
        </label>
        <label class=" float-left pr-2">
            <input class="border border-black" type=radio value={"contain"} bind:group={bgImageSize}/>
            Fit To Screen
        </label>
        {#if bgImageSize == "contain"}
            <label class="float-left">
                Background Color:
                <input type="color" bind:value={bgColor}/>
            </label>
        {/if}
    </div>
    

    <input class="hidden" accept="image/png, image/jpeg, image/jpg" multiple type="file" bind:this={fgFileInput} bind:files={fgPhotos}/>
    <label class="float-left text-lg ">
        <button class="bg-red-600" on:click={() => {uploadForegroundPhotos()}}>Upload Foreground Images</button>
        {fgPhotos ? fgPhotos.length : 0} Selected
    </label>
    <div class="justify-items-center">
        <div class="text-lg float-left pr-2">Change Foreground Image:</div>
        {#if !fgBounce}
            <label class="float-left pr-1 text-lg">
                <input class="border border-black" type="range" max="600" min="1" bind:value={fgInterval}/>
                {fgInterval} seconds
            </label>
        {/if}
        <label class="text-lg float-left pl-1">
            <input class="float-left" type="checkbox" bind:checked={fgBounce}/>
            On Bounce
        </label>
    </div>
    <div class="justify-items-center">
        <div class="text-lg float-left pr-2">Display Foreground Image:</div>
        <label class="float-left pr-2">
            <input class="border border-black" type=radio value={"cover"} bind:group={fgImageSize}/>
            Fill Screen
        </label>
        <label class=" float-left pr-2">
            <input class="border border-black" type=radio value={"contain"} bind:group={fgImageSize}/>
            Fit To Screen
        </label>
        {#if fgImageSize == "contain"}
            <label class="float-left">
                Background Color:
                <label>
                    <input type="checkbox" bind:checked={fgTransparent}/>
                    Transparent
                </label>
                or
                <input type="color" bind:value={fgColor}/>
            </label>
        {/if}
    </div>

    <button class="bg-red-600" on:click={()=> {photobounce.requestFullscreen();}} type=submit>Fullscreen</button>
</div>
