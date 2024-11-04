<script lang="ts">
import { getContext } from "svelte";
  import BackCard from "./BackCard.svelte";
import Card from "./Card.svelte";
import NoCardPile from "./NoCardPile.svelte";
import { isCardInPile, type CardType } from "./store";

const store = getContext('store');

let cards = $derived($store.filter((card: CardType) => isCardInPile(card, { type: "stock" })));

function handleClick() {
  store.pushToWastePileFromStockPile();
}

</script>

<div class="relative cursor-pointer" onclick={handleClick} aria-hidden="true">
  {#if cards.length > 0}
    <BackCard>{cards.length}</BackCard>
  {/if}
  <div class="absolute top-0">
    <NoCardPile />
  </div>
</div>