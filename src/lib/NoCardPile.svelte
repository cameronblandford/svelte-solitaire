<script lang="ts">
import { getContext } from "svelte";

	interface Props {
		bg?: string;
		children?: import('svelte').Snippet;
	}

	let { bg = '', children }: Props = $props();

const classNames = [
	'position',
	'border',
	'border-1',
	'border-black',
	'rounded-md',
	'flex',
	'flex-col',
	'items-center',
	'justify-center',
	'pointer-events-none',
	bg,
	bg === '' ? 'text-black' : 'text-white',
	bg === '' ? 'text-2xl' : 'text-sm',
];

const cardWidth = getContext('cardWidth');
const cardHeight = getContext('cardHeight');
</script>

<div
	style:width="{cardWidth}px"
	style:height="{cardHeight}px"
	class={classNames.join(' ')}>
	{@render children?.()}
</div>