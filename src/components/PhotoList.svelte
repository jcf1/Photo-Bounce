<script lang="ts">
    import { SortableList } from '@jhubbardsf/svelte-sortablejs';
    import type { SortableEvent } from 'sortablejs';

    export let photolist: File[];

    function handleOnSort(e: SortableEvent) {
        if(e && e.oldDraggableIndex != undefined && e.newDraggableIndex != undefined) {
            let oldIdx: number = e.oldDraggableIndex;
            let newIdx: number = e.newDraggableIndex;
            let selected: File = photolist[oldIdx];
            if(oldIdx < newIdx) {
                for(let i = oldIdx; i < newIdx; i++) {
                    photolist[i] = photolist[i + 1];
                }
            } else{
                for(let i = oldIdx; i > newIdx; i--) {
                    photolist[i] = photolist[i - 1];
                }
            }
            photolist[newIdx] = selected;
        }
    }   
</script>

<div class=" overflow-x-scroll">
    <SortableList
        class={'flex'}
        swapThreshold={0.7}
        animation={150}
        direction={'horizontal'}
        onSort={handleOnSort}
    >
    {#each photolist as f (f.name) }
        <img class=" pr-1 h-20 w-20 object-contain" src={URL.createObjectURL(f)} alt=""/>
    {/each}
    </SortableList>
</div>