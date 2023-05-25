<script lang="ts">
    import PhotoList from '../PhotoList.svelte';

    export let clearFGPhotos: () => void;

    export let fgPhotos: File[];
    export let fgImageSize: string;
    export let fgTransparent: boolean;
    export let fgColor: string;
    export let fgBounce: boolean;
    export let fgInterval: number;
    export let sizeMulti: number;
    export let speedMulti: number;
    
    let fgFileInput: HTMLInputElement;
    let uploads: FileList;

    $: if(uploads && uploads.length > 0) {
        updatePhotos()
    }

    function updatePhotos() {
        fgPhotos = fgPhotos.concat(Array.from(uploads));
    }

    function shufflBounceImages() {
        if(fgPhotos != null && fgPhotos.length == 0) {
            return;
        }

        var j, x, i;
        for (i = fgPhotos.length - 1; i > 0; i--) {
            j = Math.floor(Math.random() * (i + 1));
            x = fgPhotos[i];
            fgPhotos[i] = fgPhotos[j];
            fgPhotos[j] = x;
        }
    }

    function uploadBouncePhotos() {
        fgFileInput.click();
    }
</script>

<input class="is-hidden" accept="image/png, image/jpeg, image/jpg" multiple type="file" bind:this={fgFileInput} bind:files={uploads}/>
<div class="flex flex-col justify-center items-center pt-8">

    <div class="flex row-auto justify-center align-middle w-11/12 h-40 outline" >
        {#if fgPhotos && fgPhotos.length > 0}
            <PhotoList bind:photolist={fgPhotos}/>
        {:else}
            <div class="flex mb-auto mt-auto align-middle">No Photos Selected</div>
        {/if}
    </div>

    <div class="flex justify-center pt-4">
        <button class="bg-green-600 text-white mr-8" on:click={uploadBouncePhotos}>Upload Bounce Images</button>
        <button class="bg-green-600 text-white mr-8" on:click={shufflBounceImages}>Shuffle Images</button>
        <button class="bg-green-600 text-white" on:click={clearFGPhotos}>Clear Images</button>
    </div>

    <div class="table justify-center text-2xl pt-4">
        <div class="table-cell pr-8">Change Bounce Image:</div>
        <label class="table-cell pr-8">
            <input type="checkbox" bind:checked={fgBounce}/>
            On Bounce
        </label>
        <div class={fgBounce ? "invisible table" : "table"}>
            <div class="table-cell pr-2">
                Every {fgInterval} seconds (1 - 600):
            </div>
            <input class="table-cell border" type="number" max="600" min="1" bind:value={fgInterval}/>
        </div>
    </div>

    <div class="table justify-center text-2xl pt-4">
        <div class="table-cell pr-8">Display Bounce Image:</div>
        <label class="table-cell pr-4">
            <input class="border pr-2" type=radio value={"cover"} bind:group={fgImageSize}/>
            Fill Container
        </label>
        <label class="table-cell">
            <input class="border pr-2" type=radio value={"contain"} bind:group={fgImageSize}/>
            Fit To Container
        </label>
    </div>
    {#if fgImageSize == "contain"}
        <div class="table justify-center text-2xl pt-4">
            <div class="table-cell pr-8">Background Color:</div>
            <label class="table-cell pr-4">
                <input class="border pr-2" type="checkbox" bind:checked={fgTransparent}/>
                Transparent
            </label>
            <label class={fgTransparent ? "invisible table" : "table"}>
                or
                <input class="border ml-4" type="color" bind:value={fgColor}/>
            </label>
        </div>
    {/if}

    <div class="justify-center text-2xl w-2/3 pt-4">
        <span>Size Multiplier: {sizeMulti.toFixed(2)}</span>
        <input class="w-full p-0" type="range" max="1.0" min="0.1" step="0.01" bind:value={sizeMulti}/>
    </div>

    <div class="justify-center text-2xl w-2/3 pt-4 pb-4">
        <span>Speed Multiplier: {speedMulti.toFixed(4)}</span>
        <input class="w-full p-0" type="range" max="0.01" min="0.0001" step="0.0001" bind:value={speedMulti}/>
    </div>
</div>
