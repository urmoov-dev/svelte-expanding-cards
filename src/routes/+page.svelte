<script lang="ts">
	import Card from "$lib/Card.svelte";
    import ExpandedCardTarget from "$lib/ExpandedCardTarget.svelte";
	import { onMount } from "svelte";

    let cards:Array<any> = Array(23)

    
</script>

<div>
    <ExpandedCardTarget>
        {#each cards as card, i}
            <Card jsStyle={{easing:"expoOut"}}>
                <div slot="details" class="card-details">
                    Here goes all the necessary details for the card
                    <h1>{i}</h1>
                </div>
            </Card>
        {/each}
    </ExpandedCardTarget>
</div>

<style>
    :global(body) {
        margin: 0;
    }

    :global(.expanded-target) {
        display: flex;
        flex-wrap: wrap;
        justify-content: flex-start;
        align-content: stretch;
        gap: 1vh;
        padding: 1vh;
        box-sizing: content-box;
        margin: 1vh;
    }
    :global(.expanding-card) {
        flex: 1;
    }

    div {
        height: 100%;
    }

    .card-details {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
</style>