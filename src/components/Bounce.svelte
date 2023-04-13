<script lang="ts">
    import { onMount } from "svelte";

    export let parentWidth: number;
    export let parentHeight: number;

    export let fgPhotos: FileList;
    export let fgImageSize: String;
    export let fgColor: String;
    export let fgInterval: number;
    export let sizeMulti: number;
    export let speedMulti: number;

    export let startX: number;
    export let startY: number;
    export let startDir: number;
    export let bounceAngle: number;

    let element: HTMLElement;
    let currImage: HTMLImageElement;

    let xPos: number = 0;
    let xSpeed: number = 0;
    let xBuffer: number = 0;

    let yPos: number = 0;
    let ySpeed: number = 0;
    let yBuffer: number = 0;

    let currDir: number = 0;
    let size: number  = 0;

    let bounceStyle: string = `background-color:${fgColor};`;

    let fgIndex = 0;

    let interval: number;
    let moveInterval: number;
    $: if(fgPhotos && fgInterval > 0) {
        clearInterval(interval);
        interval = setInterval(nextFGPhoto, fgInterval * 1000);
    } else {
        clearInterval(interval);
    }

    onMount(() => {
        size = 0.2 * sizeMulti * (parentWidth < parentHeight ? parentWidth : parentHeight);
        currDir = startDir;
        xPos =  (startX * (parentWidth / 100)) - (size / 2);
        xSpeed = speedMulti * Math.cos(currDir* (Math.PI / 180));
        yPos =  (startY * (parentHeight / 100)) - (size / 2);
        ySpeed = speedMulti * Math.sin(currDir* (Math.PI / 180));
        moveInterval = setInterval(move, 1);
        getImageDimensions();
        
        console.log(currDir)
        console.log(bounceAngle)
        console.log(xSpeed)
        console.log(ySpeed)
        console.log(xBuffer)
        console.log(yBuffer)
    });

    export function reset(): void {
        if(element.parentElement) {
            parentWidth = element.parentElement.clientWidth;
            parentHeight = element.parentElement.clientHeight;
            let size = sizeMulti * (parentWidth < parentHeight ? parentWidth : parentHeight);
            xPos = parentWidth / 2;
            yPos = parentHeight / 2;
        }
    }

    export function clear(): void {
        reset();
    }

    function nextFGPhoto(): void {
        if(fgPhotos) {
            fgIndex = (fgIndex + 1) % fgPhotos.length;
            getImageDimensions();
        }
    }

    //Get image dimentions if transparent background is chosen to get accurate screen bounces
    function getImageDimensions() {
        xBuffer = 0;
        yBuffer = 0;
        if((fgImageSize === "contain") && (fgColor === "#00000000")) {
            if(currImage !== undefined) {
                let imgWidth: number = currImage.naturalWidth;
                let imgHeight: number = currImage.naturalHeight;
                if(imgWidth > imgHeight) {
                    let height = (parseFloat(imgHeight.toFixed(1)) / parseFloat(imgWidth.toFixed(1))) * size;
                    yBuffer = (size - height) / 2.0;
                } else {
                    let width = (parseFloat(imgWidth.toFixed(1)) / parseFloat(imgHeight.toFixed(1))) * size;
                    xBuffer = (size - width) / 2.0;
                }
            }
        }
    }

    const move = () => {
        let x = xPos;
        let y = yPos;

        getImageDimensions();

        if(x >= (parentWidth - (size + xBuffer)) && (xSpeed > 0.0)) {
            if(fgInterval === 0) {
                nextFGPhoto();
            }
            if(currDir >= 270) {
                currDir = (currDir - bounceAngle);
            } else {
                currDir = (currDir + bounceAngle) % 360;
            }
            xSpeed = speedMulti * Math.cos(currDir * (Math.PI / 180));
            ySpeed = speedMulti * Math.sin(currDir * (Math.PI / 180));
        } else if((x <= ((-1) * xBuffer)) && (xSpeed < 0.0)) {
            if(fgInterval === 0) {
                nextFGPhoto();
            }
            if(currDir >= 180) {
                currDir = (currDir + bounceAngle);
            } else {
                currDir = (currDir - bounceAngle);
            }
            xSpeed = speedMulti * Math.cos(currDir * (Math.PI / 180));
            ySpeed = speedMulti * Math.sin(currDir * (Math.PI / 180));
        } else{
            x = x + (xSpeed * parentWidth);
        }

        if(y >= (parentHeight - (size + yBuffer)) && (ySpeed > 0.0)) {
            if(fgInterval === 0) {
                nextFGPhoto();
            }
            if(currDir <= 90) {
                currDir = ((currDir - bounceAngle) + 360) % 360;
            } else {
                currDir = (currDir + bounceAngle) % 360;
            }
            xSpeed = speedMulti * Math.cos(currDir * (Math.PI / 180));
            ySpeed = speedMulti * Math.sin(currDir * (Math.PI / 180));
        } else if((y <= ((-1) * yBuffer)) && (ySpeed < 0.0)){
            if(fgInterval === 0) {
                nextFGPhoto();
            }
            if(currDir >= 270) {
                currDir = (currDir + bounceAngle) % 360;
            } else {
                currDir = (currDir - bounceAngle);
            }
            xSpeed = speedMulti * Math.cos(currDir * (Math.PI / 180));
            ySpeed = speedMulti * Math.sin(currDir * (Math.PI / 180));
        } else{
            y = y + (ySpeed * parentHeight);
        }

        x = (x < ((-1) * xBuffer)) ? ((-1) * xBuffer) : x;
        x = (x > (parentWidth - (size + xBuffer))) ? (parentWidth - (size + xBuffer)) : x;

        y = (y > (parentHeight - (size + yBuffer))) ? (parentHeight - (size + yBuffer)) : y;
        y = (y < ((-1) * yBuffer)) ? ((-1) * yBuffer) : y;

        xPos = x;
        yPos = y;

        bounceStyle = `width:${size}px;height:${size}px;top:${y}px;left:${x}px;background-color:${fgColor};`
    }
</script>

<div class="absolute items-center text-center flex justify-center" bind:this={element} style={bounceStyle}>
    {#if fgPhotos}
        <img class="w-full h-full" style={`object-fit:${fgImageSize}`} src={URL.createObjectURL(fgPhotos[fgIndex])} alt="" bind:this={currImage}/>
    {/if}
</div>

