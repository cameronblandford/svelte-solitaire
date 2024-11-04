<script lang="ts">
	interface Props {
		children?: import('svelte').Snippet;
	}

	let { children }: Props = $props();
</script>

<div class="bg-red-600 w-full h-full rounded-md border border-black text-white flex items-center justify-center text-sm">
	{@render children?.()}
</div>