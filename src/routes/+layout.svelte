<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	import Navigation from '$lib/components/Navigation.svelte';
	import { onMount } from 'svelte';
	import AOS from 'aos';
	import 'aos/dist/aos.css';

	let { children } = $props();

	onMount(() => {
		AOS.init({
			duration: 1000,
			easing: 'ease-in-out',
			once: true,
			offset: 100
		});
	});
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>Daniils - Portfolio</title>
	<meta name="description" content="Full-stack developer specializing in modern web technologies" />
</svelte:head>

<div class="min-h-screen bg-black">
	<Navigation />
	<main>
		{@render children?.()}
	</main>
</div>
