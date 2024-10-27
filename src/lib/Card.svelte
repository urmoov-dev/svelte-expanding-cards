<script lang="ts">
	import * as ease from "svelte/easing";
	import type { Writable } from "svelte/store";
	import { getContext } from "svelte";

    export let jsStyle:{
        height?: string,
        borderRadius?: string,
        easing?: string,
    }

    $: ({
            height,
            borderRadius = "1vh",
            easing = "quintOut",
        } = jsStyle
    )

    let writtenInfoHeight:number
    let cardHeight:number

    $: writtenInfoWindowHeight = `calc(${cutoff}px + 3vh)`
    $: writtenInfoHoverHeight = writtenInfoHeight+"px"

    let showDetails = false

    
    const containerNode:Writable<HTMLElement> = getContext('containerNode')
    const containerHeight:Writable<number> = getContext('containerHeight')
    const containerWidth:Writable<number> = getContext('containerWidth')

    let thisCardElement:HTMLElement
    let containerBoundingRect:DOMRect

    
    $: if ($containerHeight || $containerWidth) getPositionDetails()
    
    function getPositionDetails() {
        containerBoundingRect = $containerNode?.getBoundingClientRect()
    }
       
    function revealDetails(node:HTMLElement) {
        getPositionDetails()
        const thisCardClientRect = thisCardElement.getBoundingClientRect()
        const transformDiff = {
            width: 100-(100*thisCardClientRect.width)/containerBoundingRect.width,
            height: 100-(100*thisCardClientRect.height)/containerBoundingRect.height,
            top: thisCardClientRect.top - containerBoundingRect.top,
            left : thisCardClientRect.left - containerBoundingRect.left 
        }
        const contentWrapper:HTMLElement = node.querySelector(".offerDetailsWrapper")! as HTMLElement
        if (showDetails && contentWrapper) {
            contentWrapper!.style!.transition = "visibility 0s 1s, opacity 0.25s 1s"
            contentWrapper?.classList.remove("hideContent")
        }
        const opacityOverlay:HTMLElement = node.querySelector<HTMLElement>(".opacityOverlay")!
        opacityOverlay?.classList.remove("frostedGlass")
        const frostedGlass = opacityOverlay!.querySelector('.frostedGlass')! as HTMLElement
        frostedGlass!.style!.backgroundColor = "transparent"
            
        console.log(transformDiff.height, thisCardClientRect.height, containerBoundingRect.height)

        return {
            duration: 750,
            delay: 100,
            easing: (ease as any)[easing],
            css: (_t:number, u:number) => `transform: translate(${transformDiff.left*u}px, ${transformDiff.top*u}px) scale(${(100-u*transformDiff.width)/100}, ${(100-u*transformDiff.height)/100}); border-radius: calc(${borderRadius} * ${1+u*transformDiff.height/10})`
        }
    }

    let cutoff:any
    
</script>

<button class="flexColumnStart expanding-card" 
    style=" --writtenInfoHoverHeight: {writtenInfoHoverHeight}; 
            --writtenInfoWindowHeight: {writtenInfoWindowHeight};
            height: {height ?? "auto"};
          "
    bind:clientHeight={cardHeight}
    bind:this={thisCardElement}
    on:click={() => showDetails = !showDetails}
>
    <slot name="card"/>
</button>
{#if showDetails}
<div transition:revealDetails 
    class="details" 
    style=" top: {containerBoundingRect.top}px;
            left: {containerBoundingRect.left}px;
            width: {containerBoundingRect.width}px;
            height: {containerBoundingRect.height}px;"
>
    <slot name="details"/>
    <button class="close-details" on:click={() => showDetails = !showDetails}>
        <svg xmlns="http://www.w3.org/2000/svg" height="1rem" width="1rem" viewBox="0 0 384 512"><!--!Font Awesome Free 6.6.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2024 Fonticons, Inc.--><path d="M342.6 150.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L192 210.7 86.6 105.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L146.7 256 41.4 361.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L192 301.3 297.4 406.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L237.3 256 342.6 150.6z"/></svg>
    </button>
    <div class="opacityOverlay frostedGlass">
        <div class="frostedGlass"></div>
    </div>
</div>
{/if}

<style>
    .expanding-card {
        min-height: 300px;
        min-width: 200px;
        border-radius: 2vh;
        overflow: hidden;
        padding: 0;
    }

    .details {
        background-color: white;
        position: fixed;
        z-index: 50;
        transform-origin: top left;
        border: solid 2px black;
        border-radius: 1vh;
        overflow: hidden;
    }

    .close-details {
        position: absolute;
        top: 0px;
        right: 0px;
    }
</style>