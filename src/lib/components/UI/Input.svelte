<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	// Props
	export let type: 'text' | 'email' | 'password' | 'tel' | 'url' = 'text';
	export let placeholder = '';
	export let value = '';
	export let disabled = false;
	export let required = false;
	export let className = '';
	export let label = '';
	export let id = '';
	export let name = '';
	export let rows = 1;
	export let multiline = false;

	// Event handlers
	function handleInput(event: Event) {
		const target = event.target as HTMLInputElement | HTMLTextAreaElement;
		value = target.value;
		dispatch('input', { event, value });
	}

	function handleChange(event: Event) {
		const target = event.target as HTMLInputElement | HTMLTextAreaElement;
		value = target.value;
		dispatch('change', { event, value });
	}

	function handleFocus(event: FocusEvent) {
		dispatch('focus', { event });
	}

	function handleBlur(event: FocusEvent) {
		dispatch('blur', { event });
	}

	function handleKeydown(event: KeyboardEvent) {
		dispatch('keydown', { event });
	}

	// Generate unique ID if not provided
	$: inputId = id || `input-${Math.random().toString(36).substr(2, 9)}`;
</script>

<div class="flex flex-col space-y-2 {className}">
	{#if label}
		<label for={inputId} class="text-white font-medium text-sm">
			{label}
		</label>
	{/if}
	
	{#if multiline}
		<textarea
			{id}
			{name}
			{placeholder}
			{disabled}
			{required}
			{rows}
			bind:value
			class="
				w-full 
				px-4 
				py-3 
				bg-black 
				border-3 
				border-white 
				text-white 
				placeholder-gray-400 
				resize-none
				transition-all 
				duration-300 
				ease-in-out
				focus:outline-none 
				focus:shadow-[6px_-6px_0_0_white]
				disabled:opacity-50 
				disabled:cursor-not-allowed
				{disabled ? 'opacity-50 cursor-not-allowed' : 'hover:shadow-[3px_-3px_0_0_white]'}
			"
			on:input={handleInput}
			on:change={handleChange}
			on:focus={handleFocus}
			on:blur={handleBlur}
			on:keydown={handleKeydown}
		></textarea>
	{:else}
		<input
			{id}
			{name}
			{type}
			{placeholder}
			{disabled}
			{required}
			bind:value
			class="
				w-full 
				px-4 
				py-3 
				bg-black 
				border-3 
				border-white 
				text-white 
				placeholder-gray-400 
				transition-all 
				duration-300 
				ease-in-out
				focus:outline-none 
				focus:shadow-[6px_-6px_0_0_white]
				disabled:opacity-50 
				disabled:cursor-not-allowed
				{disabled ? 'opacity-50 cursor-not-allowed' : 'hover:shadow-[3px_-3px_0_0_white]'}
			"
			on:input={handleInput}
			on:change={handleChange}
			on:focus={handleFocus}
			on:blur={handleBlur}
			on:keydown={handleKeydown}
		/>
	{/if}
</div>
