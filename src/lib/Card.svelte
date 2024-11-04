<script lang="ts">
  import { getContext } from 'svelte';
  import BackCard from './BackCard.svelte';
import type { CardType, StoreProps } from './store';

	interface Props {
		card: CardType;
		hidden?: boolean;
		isTopPileOpenCard?: boolean;
	}

	let { card, hidden = false, isTopPileOpenCard = true }: Props = $props();

let isRed = $derived(card.suit === '♦️' || card.suit === '♥️');

let classNames = $derived([
	'relative',
	'text-3xl',
	card.isFaceDown ? 'pointer-events-none': 'pointer-events-auto',
	card.isFaceDown ? 'cursor-auto': 'cursor-pointer',
]);

const store = getContext<StoreProps>('store');
const isCardStartedDragging: any = getContext('isCardStartedDragging');
const draggingCardPosition: any = getContext('draggingCardPosition');
const pointerEvent: any = getContext('pointerEvent');

function handleClick() {
	if (!isDragging) {
		// handle click here
	} else {
		isDragging = false;
	}
}

function handleDoubleClick() {
	console.log("double click");
	store.handleCardDoubleClick(card);
}

function handlePointerDown(e: PointerEvent) {
	e.preventDefault();
	e.stopPropagation();
	pointerEvent.set(e);
	if (!card.isFaceDown) isCardStartedDragging.set(card);
}

const cardWidth = getContext('cardWidth');
const cardHeight = getContext('cardHeight');

let opacity = $derived(hidden ? '0' : '1');

let contentClass = $derived(`
	w-full
	h-full
	relative
	z-0
	flex
	${isTopPileOpenCard ? 'flex-col' : ''}
	${isTopPileOpenCard ? 'items-center' : ''}
	${isTopPileOpenCard ? 'text-3xl' : 'text-lg'}
	${isTopPileOpenCard ? 'space-y-0' : 'space-x-2'}
	${isRed ? 'text-red-600' : 'text-black'}
	justify-center
	pointer-events-none
	font-mono
	bg-white
	rounded-md
	border
	border-black
`)
</script>

<div
	onpointerdown={handlePointerDown}
	ondblclick={handleDoubleClick}
	class={classNames.join(' ')}
	style:opacity
	style:width="{cardWidth}px"
	style:height="{cardHeight}px"
>
	<div class="absolute inset-0 z-10"></div>
	{#if !card.isFaceDown}
		<div class={contentClass}>
			<span class="pointer-events-none select-none">{card.rank}</span>
			<span class="pointer-events-none select-none">{card.suit}</span>
		</div>
	{:else}
		<BackCard />
	{/if}
</div>
