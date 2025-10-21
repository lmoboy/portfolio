<script lang="ts">
	import { page } from '$app/stores';
	import { onMount } from 'svelte';

	let navOpen = false;
	let scrolled = false;

	onMount(() => {
		const handleScroll = () => {
			scrolled = window.scrollY > 50;
		};
		
		window.addEventListener('scroll', handleScroll);
		return () => window.removeEventListener('scroll', handleScroll);
	});

	function scrollToSection(sectionId: string) {
		document.getElementById(sectionId)?.scrollIntoView({ behavior: 'smooth' });
		navOpen = false;
	}

	function toggleNav() {
		navOpen = !navOpen;
	}
</script>

<nav class="fixed top-0 left-0 right-0 z-50 transition-all duration-300 {scrolled ? 'bg-black/90 backdrop-blur-md border-b border-gray-800' : 'bg-transparent'}">
	<div class="container mx-auto px-6">
		<div class="flex h-16 items-center justify-between">
			<!-- Logo -->
			<button 
				on:click={() => scrollToSection('hero')}
				class="text-2xl font-bold text-white hover:text-blue-400 transition-colors"
			>
				Daniel Developer
			</button>

			<!-- Desktop Navigation -->
			<div class="hidden md:flex space-x-8">
				<button 
					on:click={() => scrollToSection('hero')}
					class="text-sm font-medium text-gray-300 hover:text-white transition-colors"
				>
					Home
				</button>
				<button 
					on:click={() => scrollToSection('about')}
					class="text-sm font-medium text-gray-300 hover:text-white transition-colors"
				>
					About
				</button>
				<button 
					on:click={() => scrollToSection('skills')}
					class="text-sm font-medium text-gray-300 hover:text-white transition-colors"
				>
					Skills
				</button>
				<button 
					on:click={() => scrollToSection('projects')}
					class="text-sm font-medium text-gray-300 hover:text-white transition-colors"
				>
					Projects
				</button>
				<button 
					on:click={() => scrollToSection('contact')}
					class="text-sm font-medium text-gray-300 hover:text-white transition-colors"
				>
					Contact
				</button>
			</div>

			<!-- Mobile Menu Button -->
			<button 
				on:click={toggleNav}
				class="md:hidden text-white hover:text-blue-400 transition-colors"
			>
				<svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					{#if navOpen}
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
					{:else}
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
					{/if}
				</svg>
			</button>
		</div>

		<!-- Mobile Navigation -->
		{#if navOpen}
			<div class="md:hidden py-4 border-t border-gray-800">
				<div class="flex flex-col space-y-4">
					<button 
						on:click={() => scrollToSection('hero')}
						class="text-sm font-medium text-gray-300 hover:text-white transition-colors text-left"
					>
						Home
					</button>
					<button 
						on:click={() => scrollToSection('about')}
						class="text-sm font-medium text-gray-300 hover:text-white transition-colors text-left"
					>
						About
					</button>
					<button 
						on:click={() => scrollToSection('skills')}
						class="text-sm font-medium text-gray-300 hover:text-white transition-colors text-left"
					>
						Skills
					</button>
					<button 
						on:click={() => scrollToSection('projects')}
						class="text-sm font-medium text-gray-300 hover:text-white transition-colors text-left"
					>
						Projects
					</button>
					<button 
						on:click={() => scrollToSection('contact')}
						class="text-sm font-medium text-gray-300 hover:text-white transition-colors text-left"
					>
						Contact
					</button>
				</div>
			</div>
		{/if}
	</div>
</nav>

