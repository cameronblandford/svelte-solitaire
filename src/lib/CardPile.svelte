<script lang="ts">
	import { run } from 'svelte/legacy';

import { getContext } from 'svelte';
import type { Writable } from 'svelte/store';

import Card from './Card.svelte';
import NoCardPile from './NoCardPile.svelte';
import { isCardInPile, type CardPile, type CardType, type StoreProps } from './store';

	interface Props {
		pile: CardPile;
	}

	let { pile }: Props = $props();

const store = getContext<StoreProps>('store');
let cards = $derived($store.filter(card => isCardInPile(card, pile)));
run(() => {
	if ((pile.type === "stock" && pile.status === "close")) cards.reverse();
});

const isCascadable = pile.type === 'tableau';

function handleClick() {
	if (pile.type === "stock" && pile.status === "close") {
		store.onClosedStockPileClicked();
	}
}

const isCardStartedDragging: any = getContext('isCardStartedDragging');
const hoveredPile: any = getContext('hoveredPile');

function handlePointerEnter() {
	if ($isCardStartedDragging !== null) {
		hoveredPile.set(pile);
	}		
}

</script>

<div class="relative" onclick={handleClick} onpointerenter={handlePointerEnter} aria-hidden="true">
	{#if cards.length > 0}
		{#each cards as card, index}
			<div class="absolute" style={ isCascadable ? `top: ${index*40}px` : ""} >
				<Card card={card} />
			</div>
		{/each}
	{:else}
	<NoCardPile />
	{/if}
</div>