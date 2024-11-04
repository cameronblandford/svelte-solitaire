<script lang="ts">
  import { getContext } from 'svelte';
  import Card from './Card.svelte';
  import NoCardPile from './NoCardPile.svelte';
  import { isCardInTableauPileOfIndex, type StoreProps } from './store';
  
  interface Props {
    index: number;
  }

  let { index }: Props = $props();
  
  const store = getContext<StoreProps>('store');
  const draggingSession = getContext('draggingSession');

  let cardsInThisPile = $derived($store.filter(card => isCardInTableauPileOfIndex(card, index)));
  let openedCardsLength = $derived(cardsInThisPile.filter(card => !card.isFaceDown).length)

  function handleCloseCardClick() {
    if (openedCardsLength === 0) {
      store.openCardInTableauPile(index);
    }
  }

  </script>
  
  <div onclick={handleCloseCardClick} class="relative" aria-hidden="true" style:cursor="{ openedCardsLength === 0 ? 'pointer' : 'initial' }">
    <NoCardPile />
    {#if cardsInThisPile.length > 0}
      {#each cardsInThisPile as card, index}
        <div class="absolute" style="top: {(index)*30}px">
          <Card isTopPileOpenCard={index === cardsInThisPile.length - 1} card={card} hidden={$draggingSession !== null && $draggingSession.findIndex(c => c.id === card.id) !== -1} />
        </div>
      {/each}
    {/if}
  </div>