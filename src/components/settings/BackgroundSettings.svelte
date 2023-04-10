<script lang="ts">
    import PhotoList from '../PhotoList.svelte';

    let bgFileInput: HTMLInputElement;
    let uploads: FileList;
    export let bgPhotos: File[] = [];

    export let bgImageSize: string = "cover";
    export let bgColor: string = "#000000";

    export let bgInterval: number = 10;
    let interval: number = 10;
    let bounce: boolean = false;

    $: bgInterval = bounce ? 0 : interval;
    $: if(uploads && uploads.length > 0) {
        updatePhotos()
    }

    function updatePhotos() {
        bgPhotos = bgPhotos.concat(Array.from(uploads));
    }

    function uploadBackgroundPhotos() {
        bgFileInput.click();
    }
</script>

<input class="hidden" accept="image/png, image/jpeg, image/jpg" multiple type="file" bind:this={bgFileInput} bind:files={uploads}/>
<div class="justify-center">
    
    <div class="flex flex-row justify-center">
        <button class="bg-red-600" on:click={() => {uploadBackgroundPhotos()}}>Upload Background Images</button>
    </div>

    <div class="flex flex-row justify-center">
        {#if bgPhotos && bgPhotos.length > 0}
            <PhotoList bind:photolist={bgPhotos}/>
        {:else}
            <div>No Photos Selected</div>
        {/if}
    </div>

    <div class="flex flex-row justify-center">
        <div class="text-lg">Change Background Image:</div>
        {#if !bounce}
            <label class="pr-1 text-lg">
                Every {bgInterval} seconds (1 - 600):
                <input class="border border-black" type="number" max="600" min="1" bind:value={interval}/>
            </label>
        {/if}
        <label class="text-lg pl-1">
            <input type="checkbox" bind:checked={bounce}/>
            On Bounce
        </label>
    </div>

    <div class="flex flex-row justify-center">
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
</div>
