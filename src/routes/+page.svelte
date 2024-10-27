<script lang="ts">
	import colors from 'tailwindcss/colors';
	import hexToHsl from '$lib/hextohsl';

	let value = $state('');
	let type = $state('hsl');

	const convert = (hex: string) => {
		let content = '';
		const hsl = hexToHsl(hex);
		if (!hsl) return;

		switch (type) {
			case 'hsl':
				content = `hsl(${hsl.h}, ${hsl.s}, ${hsl.l})`;
				break;
			case 'hslv':
				content = `${hsl.h} ${hsl.s} ${hsl.l}`;
				break;
			default:
				return;
		}

		navigator.clipboard.writeText(content);
	};
</script>

<div
	class="sticky top-0 flex flex-col items-center gap-4 border-b border-purple-200 bg-white p-4 shadow-md sm:flex-row sm:justify-between"
>
	<div class="flex gap-2">
		<input
			type="text"
			bind:value
			placeholder="Enter hex color with #"
			class="rounded border-purple-500 text-purple-500 outline-none focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-purple-500"
		/>
		<button class="rounded bg-purple-500 px-2 py-1 text-white" onclick={() => convert(value)}
			>Convert</button
		>
	</div>
	<div class="flex gap-2">
		<label class="flex items-center gap-2">
			<input type="radio" name="type" value="hsl" bind:group={type} checked={type === 'hsl'} /> HSL
		</label>

		<label class="flex items-center gap-2">
			<input type="radio" name="type" value="hslv" bind:group={type} checked={type === 'hslv'} /> HSL
			as CSS Variable
		</label>
	</div>
</div>
<section>
	{#each Object.entries(colors) as [color, value]}
		{#if typeof value === 'object'}
			{#each Object.entries(value) as [shade, shadeValue]}
				{@const textColor = (parseInt(shade) ?? 0) > 500 ? 'white' : 'black'}
				<!-- svelte-ignore a11y_invalid_attribute -->
				<button
					class="cursor-pointer p-4 outline-4 outline-purple-500"
					style:background-color={shadeValue as string}
					style:color={textColor}
					onclick={() => convert(shadeValue as string)}
				>
					{color}-{shade} <br />
					{shadeValue}
				</button>
			{/each}
		{/if}
	{/each}
</section>

<style>
	section {
		display: grid;
		@apply text-xs;
		grid-gap: 1px;
		grid-template-columns: repeat(auto-fill, minmax(80px, 1fr));
	}
</style>
