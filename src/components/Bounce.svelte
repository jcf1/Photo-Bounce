<script lang="ts">
    import { onMount } from "svelte";

    export let nextBGPhoto: () => void;

    export let parentWidth: number;
    export let parentHeight: number;

    export let fgPhotos: File[];
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

    let logos: string[] = ["/images/Red_PhotoBounce.png","/images/Blue_PhotoBounce.png","/images/Orange_PhotoBounce.png","/images/Magenta_PhotoBounce.png","/images/Purple_PhotoBounce.png"]
    let logoIndex: number = 0;

    let xPos: number = 0;
    let xSpeed: number = 0;
    let xBuffer: number = 0;

    let yPos: number = 0;
    let ySpeed: number = 0;
    let yBuffer: number = 0;

    let currDir: number = 0;
    let size: number  = 0;

    let bounceStyle: string = `background-color:${fgColor};`;

    let fgIndex: number = 0;

    let interval: number;
    let moveInterval: number;

    let mounted: boolean = false;
    onMount(() => {
        mounted = true;
        fgIndex = 0;
        size = sizeMulti * (parentWidth < parentHeight ? parentWidth : parentHeight);
        currDir = startDir;
        xPos =  (startX * (parentWidth / 100)) - (size / 2);
        xSpeed = speedMulti * Math.cos(currDir* (Math.PI / 180));
        yPos =  (startY * (parentHeight / 100)) - (size / 2);
        ySpeed = speedMulti * Math.sin(currDir* (Math.PI / 180));
        calculateImageDimensions();
        moveInterval = setInterval(move, 1);
    });

    $: if(mounted) {
        if(speedMulti) {
            xSpeed = speedMulti * Math.cos(currDir* (Math.PI / 180));
            ySpeed = speedMulti * Math.sin(currDir* (Math.PI / 180));
        }
        if(sizeMulti) {
            size = sizeMulti * (parentWidth < parentHeight ? parentWidth : parentHeight);
            calculateImageDimensions();
        }
    }

    export function reset(): void {
        fgIndex = 0;
        logoIndex = 0;
        size = sizeMulti * (parentWidth < parentHeight ? parentWidth : parentHeight);
        currDir = startDir;
        xPos =  (startX * (parentWidth / 100)) - (size / 2);
        xSpeed = speedMulti * Math.cos(currDir* (Math.PI / 180));
        yPos =  (startY * (parentHeight / 100)) - (size / 2);
        ySpeed = speedMulti * Math.sin(currDir* (Math.PI / 180));
        if(fgPhotos && fgInterval > 0) {
            clearInterval(interval);
            interval = setInterval(nextFGPhoto, fgInterval * 1000);
        } else {
            clearInterval(interval);
        }
        calculateImageDimensions();
    }

    function nextFGPhoto(): void {
        if(fgPhotos && fgPhotos.length > 0) {
            fgIndex = (fgIndex + 1) % fgPhotos.length;
        } else {
            logoIndex = (logoIndex + 1) % logos.length;
        }
    }

    //Get image dimentions if transparent background is chosen to get accurate screen bounces
    function calculateImageDimensions() {
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

        if((fgImageSize === "contain") && (fgColor === "#00000000") && (xBuffer !=0 || yBuffer != 0)) {
            if(x > (parentWidth - (size - xBuffer))) {
                x = parentWidth - (size - xBuffer);
            } else if(x <= ((-1) * xBuffer)) {
                x = (-1) * xBuffer;
            }
            if(y > (parentHeight - (size - yBuffer))) {
                y = parentHeight - (size - yBuffer);
            } else if(y <= ((-1) * yBuffer)) {
                y = (-1) * yBuffer;
            }
        }

        if(x >= (parentWidth - (size - xBuffer)) && (xSpeed > 0.0)) {
            nextBGPhoto();
            if(fgInterval === 0) {
                nextFGPhoto();
            }
            if(currDir > 270) {
                currDir = (currDir - bounceAngle);
            } else if(currDir === 270) {
                currDir = Math.floor(Math.random() * 2) == 0 ? (currDir - bounceAngle) : (currDir + bounceAngle) % 360;
            } else {
                currDir = (currDir + bounceAngle) % 360;
            }

            xSpeed = speedMulti * Math.cos(currDir * (Math.PI / 180));
            ySpeed = speedMulti * Math.sin(currDir * (Math.PI / 180));
        } else if((x <= ((-1) * xBuffer)) && (xSpeed < 0.0)) {
            nextBGPhoto();
            if(fgInterval === 0) {
                nextFGPhoto();
            }
            if(currDir > 180) {
                currDir = (currDir + bounceAngle);
            } else if(currDir == 180) {
                currDir = Math.floor(Math.random() * 2) == 0 ? (currDir + bounceAngle) : (currDir - bounceAngle);
            } else {
                currDir = (currDir - bounceAngle);
            }
            xSpeed = speedMulti * Math.cos(currDir * (Math.PI / 180));
            ySpeed = speedMulti * Math.sin(currDir * (Math.PI / 180));
        }

        if(y >= (parentHeight - (size - yBuffer)) && (ySpeed > 0.0)) {
            nextBGPhoto();
            if(fgInterval === 0) {
                nextFGPhoto();
            }
            if(currDir < 90) {
                currDir = ((currDir - bounceAngle) + 360) % 360;
            } else if(currDir === 90) {
                currDir = Math.floor(Math.random() * 2) == 0 ? ((currDir - bounceAngle) + 360) % 360 : (currDir + bounceAngle) % 360;
            } else {
                currDir = (currDir + bounceAngle) % 360;
            }
            xSpeed = speedMulti * Math.cos(currDir * (Math.PI / 180));
            ySpeed = speedMulti * Math.sin(currDir * (Math.PI / 180));
        } else if((y <= ((-1) * yBuffer)) && (ySpeed < 0.0)){
            nextBGPhoto();
            if(fgInterval === 0) {
                nextFGPhoto();
            }
            
            if(currDir > 270) {
                currDir = (currDir + bounceAngle) % 360;
            } else if(currDir === 270) {
                currDir = Math.floor(Math.random() * 2) == 0 ? (currDir + bounceAngle) % 360 : (currDir - bounceAngle);
            } else {
                currDir = (currDir - bounceAngle);
            }
            xSpeed = speedMulti * Math.cos(currDir * (Math.PI / 180));
            ySpeed = speedMulti * Math.sin(currDir * (Math.PI / 180));
        }

        x = x + (xSpeed * parentWidth);
        y = y + (ySpeed * parentHeight);
        xPos = x;
        yPos = y;
        bounceStyle = `width:${size}px;height:${size}px;top:${y}px;left:${x}px;background-color:${fgColor};`
    }
</script>

<div class="absolute items-center text-center flex justify-center" bind:this={element} style={bounceStyle}>
    {#if fgPhotos && fgPhotos.length > 0}
        <img class="w-full h-full" style={`object-fit:${fgImageSize}`} src={URL.createObjectURL(fgPhotos[fgIndex])} alt="" bind:this={currImage} on:load={calculateImageDimensions}/>
    {:else}
        <img class="w-full h-full" style={`object-fit:${fgImageSize}`} src={logos[logoIndex]} alt="" bind:this={currImage} on:load={calculateImageDimensions}/>
    {/if}
</div>

