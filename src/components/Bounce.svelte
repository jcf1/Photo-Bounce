<script lang="ts">
    import { onMount, onDestroy } from "svelte";

    export let parentWidth: number;
    export let parentHeight: number;

    export let fgPhotos: FileList;
    export let fgImageSize: String;
    export let fgColor: String;
    export let fgTransparent: boolean;
    export let fgInterval: number;


    export let sizeMulti: number = 0.25;

    interface Position {
        x: number,
        y: number
    }

    let element: HTMLElement;
    let currImage: HTMLImageElement;

    let size: number = 10;
    let xSpeed: number = 0.0003;
    let ySpeed: number = 0.0003;
    let pos: Position = {
        x: 0,
        y: 0
    };

    let bounceStyle: string = `background-color:${fgColor};`;

    let fgIndex = 0;

    let interval: number;
    $: if(fgPhotos && fgInterval > 0) {
        clearInterval(interval)
        interval = setInterval(nextFGPhoto, fgInterval * 1000)
    } else {
        clearInterval(interval)
    }

    onMount(() => {
        size = sizeMulti * (parentWidth > parentHeight ? parentWidth : parentHeight);
        pos = {
            x: parentWidth / 2,
            y: parentHeight / 2
        }
        setInterval(move, 1);
    });

    export function reset(): void {
        if(element.parentElement) {
            parentWidth = element.parentElement.clientWidth;
            parentHeight = element.parentElement.clientHeight;
            size = sizeMulti * (parentWidth < parentHeight ? parentWidth : parentHeight);
            pos = {
                x: parentWidth / 2,
                y: parentHeight / 2
            }
        }
    }

    export function clear(): void {
        reset();
    }

    function nextFGPhoto(): void {
        if(fgPhotos) {
            fgIndex = (fgIndex + 1) % fgPhotos.length;
        }
    }

    function move() {
        let x = pos.x;
        let y = pos.y;
        size = sizeMulti * (parentWidth < parentHeight ? parentWidth : parentHeight);
        let width = size;
        let height = size;
        let xBuffer = 0;
        let yBuffer = 0;

        //Get image dimentions if transparent background is chosen to get accurate screen bounces
        if((fgImageSize === "contain") && fgTransparent) {
            if(currImage !== undefined) {
                let imgWidth: number = currImage.naturalWidth;
                let imgHeight: number = currImage.naturalHeight;
                if(imgWidth > imgHeight) {
                    height = (parseFloat(imgHeight.toFixed(1)) / parseFloat(imgWidth.toFixed(1))) * size;
                    yBuffer = (size - height) / 2.0;
                } else {
                    width = (parseFloat(imgWidth.toFixed(1)) / parseFloat(imgHeight.toFixed(1))) * size;
                    xBuffer = (size - width) / 2.0;
                }
            }
        }
        
        if((x >= (parentWidth - (width + xBuffer)) && (xSpeed > 0.0)) || ((x <= ((-1) * xBuffer)) && (xSpeed < 0.0))) {
            if(fgInterval === 0) {
                nextFGPhoto();
            }
            x = x + ((xSpeed * parentWidth) * (-1));
            xSpeed *= (-1);
        } else {
            x = x + (xSpeed * parentWidth)
        }

        if((y >= (parentHeight - (height + yBuffer)) && (ySpeed > 0.0)) || ((y <= ((-1) * yBuffer)) && (ySpeed < 0.0))) {
            if(fgInterval === 0) {
                nextFGPhoto();
            }
            y = y + ((ySpeed * parentHeight) * (-1));
            ySpeed *= (-1);
        } else {
            y = y + (ySpeed * parentHeight)
        }

        x = (x < ((-1) * xBuffer)) ? ((-1) * xBuffer) : x;
        x = (x > (parentWidth - (width + xBuffer))) ? (parentWidth - (width + xBuffer)) : x;

        y = (y > (parentHeight - (height + yBuffer))) ? (parentHeight - (height + yBuffer)) : y;
        y = (y < ((-1) * yBuffer)) ? ((-1) * yBuffer) : y;

        pos = {
            x: x,
            y: y
        };

        bounceStyle = `width:${size}px;height:${size}px;top:${y}px;left:${x}px;background-color:${fgColor};`
    }
</script>

<div class="absolute items-center text-center flex justify-center" bind:this={element} style={bounceStyle}>
    {#if fgPhotos}
        <img class="w-full h-full" style={`object-fit:${fgImageSize}`} src={URL.createObjectURL(fgPhotos[fgIndex])} alt="" bind:this={currImage}/>
    {/if}
</div>

