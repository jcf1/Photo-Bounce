<script lang="ts">
    import { onMount, onDestroy } from "svelte";

    export let parentWidth: number;
    export let parentHeight: number;

    export let sizeMulti: number = 0.25;

    interface Position {
        x: number,
        y: number
    }

    let element: HTMLElement;

    let size: number = 10;
    let xSpeed: number = 0.001;
    let ySpeed: number = 0.001;
    let pos: Position = {
        x: 0,
        y: 0
    };

    let bounceStyle: string = '';

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

    function move() {
        let xBuffer: number = 0;
        let yBuffer: number = 0;
        let x = pos.x;
        let y = pos.y;
        size = sizeMulti * (parentWidth < parentHeight ? parentWidth : parentHeight);
        
        if((x >= (parentWidth - (size + xBuffer)) && (xSpeed > 0.0)) || ((x <= ((-1) * xBuffer)) && (xSpeed < 0.0))) {
            x = x + ((xSpeed * parentWidth) * (-1));
            xSpeed *= (-1);
        } else {
            x = x + (xSpeed * parentWidth)
        }

        if((y >= (parentHeight - (size + yBuffer)) && (ySpeed > 0.0)) || ((y <= ((-1) * yBuffer)) && (ySpeed < 0.0))) {
            y = y + ((ySpeed * parentHeight) * (-1));
            ySpeed *= (-1);
        } else {
            y = y + (ySpeed * parentHeight)
        }

        x = (x < ((-1) * xBuffer)) ? ((-1) * xBuffer) : x;
        x = (x > (parentWidth - (size + xBuffer))) ? (parentWidth - (size + xBuffer)) : x;

        y = (y > (parentHeight - (size + yBuffer))) ? (parentHeight - (size+ yBuffer)) : y;
        y = (y < ((-1) * yBuffer)) ? ((-1) * yBuffer) : y;

        pos = {
            x: x,
            y: y
        };

        bounceStyle = `width:${size}px;height:${size}px;top:${y}px;left:${x}px`
    }
</script>

<div class="absolute items-center text-center flex justify-center bg-red-500"  bind:this={element} style={bounceStyle}>
</div>

