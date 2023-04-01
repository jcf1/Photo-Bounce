<script lang="ts">
    let fgFileInput: HTMLInputElement;
    export let fgPhotos: FileList;
    export let fgImageSize: string = "contain";
    export let fgColor: string = "#FFFFFF";
    export let fgInterval: number = 10;
    
    let transparent: boolean = true;
    let color: string;
    let interval: number = 10;
    let bounce: boolean = true;

    $: fgInterval = bounce ? 0 : interval;
    $: fgColor = transparent ? "#00000000" : color;

    function uploadBouncePhotos() {
        fgFileInput.click();
        fgPhotos = fgPhotos;
    }
</script>

<input class="hidden" accept="image/png, image/jpeg, image/jpg" multiple type="file" bind:this={fgFileInput} bind:files={fgPhotos}/>
<div class="justify-center">

    <label class="flex flex-row justify-center">
        <button class="bg-red-600" on:click={() => {uploadBouncePhotos()}}>Upload Foreground Images</button>
        {fgPhotos ? fgPhotos.length : 0} Selected
    </label>

    <div class="flex flex-row justify-center">
        <div class="text-lg float-left pr-2">Change Foreground Image:</div>
        {#if !bounce}
            <label class="float-left pr-1 text-lg">
                <input class="border border-black" type="range" max="600" min="1" bind:value={interval}/>
                {fgInterval} seconds
            </label>
        {/if}
        <label class="text-lg float-left pl-1">
            <input class="float-left" type="checkbox" bind:checked={bounce}/>
            On Bounce
        </label>
    </div>

    <div class="flex flex-row justify-center">
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
                    <input type="checkbox" bind:checked={transparent}/>
                    Transparent
                </label>
                or
                <input type="color" bind:value={color}/>
            </label>
        {/if}
    </div>
</div>
