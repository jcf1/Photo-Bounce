<script lang="ts">
    
    export let width: Number = 64;
    export let height: Number = 36;
    export let bgColor: String = "black"
    export let bgPhotos: FileList;


    let bgIndex = 0;

    $: if(bgPhotos) {
        console.log('RENDER')
        bgPhotos = bgPhotos
    }

    function getBGPhoto(): string {
        console.log(bgPhotos)
        if(bgPhotos) {
            return URL.createObjectURL(bgPhotos[0]);
        }
        return "data:,";
    }

    function getStyle(): string {
        return "height:"+height+"vh; width:"+width+"vh; background-color:"+bgColor+";"
    }

    export function run() {
        console.log('PB')
        console.log(bgPhotos)
    }
</script>

<div class="flex items-center justify-center" style={getStyle()}>
    {#if bgPhotos}
        <img class="w-full h-full" src={URL.createObjectURL(bgPhotos[bgIndex])} alt=""/>
    {/if}
</div>
