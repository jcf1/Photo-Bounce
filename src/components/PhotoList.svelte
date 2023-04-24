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

<div class="overflow-x-scroll justify-center items-center w-full h-full">
    <SortableList
        class={'flex w-full h-full'}
        swapThreshold={0.7}
        animation={150}
        direction={'horizontal'}
        onSort={handleOnSort}
    >
    {#each photolist as f (f.name) }
        <img class="inline-block align-middle object-contain pr-2 h-full w-40 mb-auto mt-auto" src={URL.createObjectURL(f)} alt=""/>
    {/each}
    </SortableList>
</div>