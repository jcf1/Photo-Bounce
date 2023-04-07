<script lang="ts">
    import { onMount, onDestroy, createEventDispatcher } from "svelte";
	export let file: File;
    onMount(() => {
        console.log(file)
    });
</script>

<div class="flex relative items-center justify-center w-8 h-8">
    <img class="w-2/3 h-2/3" id="tbn" src={URL.createObjectURL(file.file)} alt=""/>
    <label class="text-sm" for="tbn">{file.file.name}</label>
</div>