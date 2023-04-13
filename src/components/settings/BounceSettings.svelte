<script lang="ts">
    let fgFileInput: HTMLInputElement;
    export let fgPhotos: FileList;
    export let fgImageSize: string = "contain";
    export let fgColor: string = "#FFFFFF";
    export let fgInterval: number = 10;
    export let sizeMulti: number = 1.0;
    export let speedMulti: number = 0.0005;
    
    let transparent: boolean = false;
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
                <input class="border border-black" type="number" max="600" min="1" bind:value={interval}/>
                {fgInterval} seconds (1 - 600)
            </label>
        {/if}
        <label class="text-lg float-left pl-1">
            <input type="checkbox" bind:checked={bounce}/>
            On Bounce
        </label>
    </div>

    <div class="flex flex-row justify-center">
        <div class="text-lg float-left pr-2">Display Foreground Image:</div>
        <label class="float-left pr-2">
            <input class="border border-black" type=radio value={"cover"} bind:group={fgImageSize}/>
            Fill Container
        </label>
        <label class=" float-left pr-2">
            <input class="border border-black" type=radio value={"contain"} bind:group={fgImageSize}/>
            Fit To Container
        </label>
    </div>
    {#if fgImageSize == "contain"}
        <div class="flex flex-row justify-center">
            <label class="float-left">
                Background Color:
                <label>
                    <input type="checkbox" bind:checked={transparent}/>
                    Transparent
                </label>
                {#if !transparent}
                    or
                    <input type="color" bind:value={color}/>
                {/if}
            </label>
        </div>
    {/if}

    <div class="flex flex-row justify-center">
        <label class="float-left">
            Size Multiplier:
            <label>
                <input class="border border-black" type="range" max="2.0" min="0.25" step="0.01" bind:value={sizeMulti}/>
                {sizeMulti.toFixed(2)}
            </label>
        </label>
    </div>

    <div class="flex flex-row justify-center">
        <label class="float-left">
            Speed:
            <label>
                <input class="border border-black" type="range" max="0.0015" min="0.0001" step="0.0001" bind:value={speedMulti}/>
                {speedMulti.toFixed(2)}
            </label>
        </label>
    </div>

</div>
