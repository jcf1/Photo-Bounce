<script lang="ts">
    import PhotoList from '../PhotoList.svelte';

    export let clearBGPhotos: () => void;

    export let bgPhotos: File[];
    export let bgImageSize: string;
    export let bgColor: string;
    export let bgBounce: boolean;
    export let bgInterval: number;

    let bgFileInput: HTMLInputElement;
    let uploads: FileList;

    $: if(uploads && uploads.length > 0) {
        updatePhotos()
    }

    function updatePhotos() {
        bgPhotos = bgPhotos.concat(Array.from(uploads));
    }

    function shufflBackgroundImages() {
        if(bgPhotos != null && bgPhotos.length == 0) {
            return;
        }

        var j, x, i;
        for (i = bgPhotos.length - 1; i > 0; i--) {
            j = Math.floor(Math.random() * (i + 1));
            x = bgPhotos[i];
            bgPhotos[i] = bgPhotos[j];
            bgPhotos[j] = x;
        }
    }

    function uploadBackgroundPhotos() {
        bgFileInput.click();
    }
</script>

<input class="is-hidden" accept="image/png, image/jpeg, image/jpg" multiple type="file" bind:this={bgFileInput} bind:files={uploads}/>
<div class="flex flex-col justify-center items-center pt-8">

    <div class="flex row-auto justify-center align-middle w-11/12 h-40 outline" >
        {#if bgPhotos && bgPhotos.length > 0}
            <PhotoList bind:photolist={bgPhotos}/>
        {:else}
            <div class="flex mb-auto mt-auto align-middle">No Photos Selected</div>
        {/if}
    </div>

    <div class="flex justify-center pt-4">
        <button class="bg-green-600 text-white mr-8" on:click={uploadBackgroundPhotos}>Upload Background Images</button>
        <button class="bg-green-600 text-white mr-8" on:click={shufflBackgroundImages}>Shuffle Images</button>
        <button class="bg-green-600 text-white" on:click={clearBGPhotos}>Clear Images</button>
    </div>

    <div class="table justify-center text-2xl pt-4">
        <div class="table-cell pr-8">Change Background Image:</div>
        <label class="table-cell pr-8">
            <input type="checkbox" bind:checked={bgBounce}/>
            On Bounce
        </label>
        <div class={bgBounce ? "invisible table" : "table"}>
            <div class="table-cell pr-2">
                Every {bgInterval} seconds (1 - 600):
            </div>
            <input class="table-cell border" type="number" max="600" min="1" bind:value={bgInterval}/>
        </div>
    </div>

    <div class="table justify-center text-2xl pt-4">
        <div class="table-cell pr-8">Display Background Image:</div>
        <label class="table-cell pr-8">
            <input class="border pr-2" type=radio value={"cover"} bind:group={bgImageSize}/>
            Fill Screen
        </label>
        <label class="table-cell pr-8">
            <input class="border pr-2" type=radio value={"contain"} bind:group={bgImageSize}/>
            Fit To Screen
        </label>
        <div class={bgImageSize == "contain" ? "table" : "invisible table"}>
            <label class="table-cell">
                Background Color:
                <input class="border ml-2" type="color" bind:value={bgColor}/>
            </label>
        </div>
    </div>
</div>
