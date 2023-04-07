<script lang="ts">
    import DragDrop from "svelte-dragdroplist";

    let labelStyle = "width:40px;height:20px"
    let imgStyle = "width:16vh;height:16vh;objectFit:contain;"

    export let photolist: File[];
    let count: number = photolist.length;
    let list: {idx: number, html: string}[] = photolist.map((f,i) => makeFileObj(f,i));

    

    function makeFileObj(f: File, i: number) {
        return {
            "idx": i,
            "html": `<label style=${labelStyle}><img style=${imgStyle} src="${URL.createObjectURL(f)}" alt=""/>${f.name}</label>`
        }
    }

    $: if(count > list.length) {
            // File was removed from list
            let remove = -1;
            for(let i = 0; i < list.length; i++) {
                if(list[i].idx != i) {
                    remove = i;
                    break
                }
            }
            if(remove != -1) {
                photolist.splice(remove, 1);
            }
            list = photolist.map((f,i) => makeFileObj(f,i));
            count = list.length;
        } else if(count < photolist.length) {
            // Files were added to the list
            list = photolist.map((f,i) => makeFileObj(f,i));
            count = photolist.length;
        } else {
            // Files were reordered
            let swap1 = -1;
            let swap2 = -1;
            for(let i = 0; i < list.length; i++) {
                swap2 = list[i].idx;
                if(swap2 != i) {
                    swap1 = i;
                    break
                }
            }
            if(swap1 != -1) {
                let temp = photolist[swap1];
                photolist[swap1] = photolist[swap2];
                photolist[swap2] = temp;
            }
            list = photolist.map((f,i) => makeFileObj(f,i));
        }
        
</script>


<DragDrop
    bind:data={list}
    removesItems={true}
/>
