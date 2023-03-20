<script lang="ts">
    import PhotoBounce from "../components/PhotoBounce.svelte";

    let photobounce: PhotoBounce;

    let bgFileInput: HTMLInputElement;
    let bgPhotos: FileList;

    let refresh: boolean = true;

    function uploadBackgroundPhotos() {
        bgFileInput.click();
        bgPhotos = bgPhotos;
    }

    function run() {
        console.log('RUN');
        console.log(bgPhotos);
        photobounce.run();
        refresh != refresh;
    }

</script>

<div class="grid gap-1 justify-center justify-items-center items-center h-screen w-screen">
    <div class="text-6xl">Photo Bounce</div>
    <PhotoBounce 
        bind:this={photobounce}
        width={64}
        height={36}
        bgColor={"black"}
        {bgPhotos}
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
    <button on:click={()=> {run()}} type=submit>RUN</button>
</div>
