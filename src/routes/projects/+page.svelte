<script lang="ts">
	import { onMount } from 'svelte';

	interface Repository {
		id: number;
		name: string;
		description: string | null;
		html_url: string;
		homepage: string | null;
		stargazers_count: number;
		forks_count: number;
		language: string | null;
		topics: string[];
		created_at: string;
		updated_at: string;
		pushed_at: string;
	}

	let repos: Repository[] = $state([]);
	let loading = $state(true);
	let error = $state('');
	let githubUsername = $state('yourusername'); // Replace with actual username

	// Function to determine if a repository is "worthy" of featuring
	function isWorthyRepo(repo: Repository): boolean {
		const now = new Date();
		const pushedDate = new Date(repo.pushed_at);
		const daysSinceUpdate = Math.floor((now.getTime() - pushedDate.getTime()) / (1000 * 60 * 60 * 24));

		// Criteria for worthy repos:
		// 1. Has at least 2 stars OR
		// 2. Has a description AND was updated in the last 180 days OR
		// 3. Has topics/tags (shows it's well-maintained) OR
		// 4. Has a homepage (likely a deployed project) OR
		// 5. Has been forked (shows community interest)

		return (
			repo.stargazers_count >= 2 ||
			(repo.description !== null && repo.description.length > 0 && daysSinceUpdate < 180) ||
			(repo.topics && repo.topics.length > 0) ||
			repo.homepage !== null ||
			repo.forks_count > 0
		);
	}

	// Function to calculate a "worthiness score" for sorting
	function calculateScore(repo: Repository): number {
		const now = new Date();
		const pushedDate = new Date(repo.pushed_at);
		const daysSinceUpdate = Math.floor((now.getTime() - pushedDate.getTime()) / (1000 * 60 * 60 * 24));

		let score = 0;
		
		// Stars contribute heavily to score
		score += repo.stargazers_count * 10;
		
		// Forks contribute to score
		score += repo.forks_count * 5;
		
		// Recent activity is valuable (inverse of days since update)
		if (daysSinceUpdate < 30) score += 20;
		else if (daysSinceUpdate < 90) score += 10;
		else if (daysSinceUpdate < 180) score += 5;
		
		// Having a description shows care
		if (repo.description) score += 5;
		
		// Topics/tags show organization
		score += (repo.topics?.length || 0) * 3;
		
		// Having a homepage (deployed project) is valuable
		if (repo.homepage) score += 15;

		return score;
	}

	onMount(async () => {
		try {
			const response = await fetch(`https://api.github.com/users/${githubUsername}/repos?per_page=100&sort=updated`);
			
			if (!response.ok) {
				throw new Error('Failed to fetch repositories');
			}

			const allRepos: Repository[] = await response.json();
			
			// Filter worthy repos and sort by score
			repos = allRepos
				.filter(repo => !repo.name.startsWith('.') && isWorthyRepo(repo)) // Filter out config repos and unworthy ones
				.sort((a, b) => calculateScore(b) - calculateScore(a)) // Sort by score (highest first)
				.slice(0, 12); // Limit to top 12 projects

			loading = false;
		} catch (err) {
			error = err instanceof Error ? err.message : 'An error occurred';
			loading = false;
		}
	});

	function formatDate(dateString: string): string {
		const date = new Date(dateString);
		return date.toLocaleDateString('en-US', { year: 'numeric', month: 'short' });
	}
</script>

<div class="py-12 text-white">
	<div class="mb-12">
		<h1 class="mb-4 border-b-4 border-white pb-4 text-5xl font-bold tracking-tight">Projects</h1>
		<p class="text-lg text-white">
			A curated selection of my most notable work from GitHub. Automatically filtered to showcase
			the projects that demonstrate my skills and contributions.
		</p>
	</div>

	{#if loading}
		<div class="flex min-h-[400px] items-center justify-center">
			<div class="text-center">
				<div class="mb-4 inline-block h-12 w-12 animate-spin rounded-full border-4 border-white border-t-transparent"></div>
				<p class="text-lg font-semibold">Loading projects...</p>
			</div>
		</div>
	{:else if error}
		<div class="rounded-lg border-2 border-white bg-gray-100 p-8 text-center">
			<p class="text-lg font-semibold text-red-600">Error: {error}</p>
			<p class="mt-2 text-gray-600">
				Please update the GitHub username in the code or check your internet connection.
			</p>
		</div>
	{:else if repos.length === 0}
		<div class="rounded-lg border-2 border-white bg-gray-100 p-8 text-center">
			<p class="text-lg font-semibold">No projects found</p>
			<p class="mt-2 text-gray-600">
				Update your GitHub username in <code class="rounded bg-black px-2 py-1 text-white"
					>src/routes/projects/+page.svelte</code
				>
			</p>
		</div>
	{:else}
		<div class="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
			{#each repos as repo (repo.id)}
				<article
					class="group flex flex-col border-2 border-white transition-all hover:-translate-y-1 hover:shadow-[8px_8px_0px_0px_rgba(0,0,0,1)]"
				>
					<div class="border-b-2 border-white bg-white p-4">
						<h2 class="text-xl font-bold text-black group-hover:underline">
							{repo.name}
						</h2>
					</div>

					<div class="flex flex-1 flex-col p-6">
						<p class="mb-4 flex-1 text-gray-700">
							{repo.description || 'No description provided'}
						</p>

						{#if repo.topics && repo.topics.length > 0}
							<div class="mb-4 flex flex-wrap gap-2">
								{#each repo.topics.slice(0, 3) as topic}
									<span class="border border-black px-2 py-1 text-xs uppercase">{topic}</span>
								{/each}
							</div>
						{/if}

						<div class="mb-4 flex items-center gap-4 text-sm text-gray-600">
							{#if repo.language}
								<span class="flex items-center gap-1">
									<span class="h-3 w-3 rounded-full bg-black"></span>
									{repo.language}
								</span>
							{/if}
							<span class="flex items-center gap-1">
								<svg class="h-4 w-4" fill="currentColor" viewBox="0 0 20 20">
									<path
										d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
									/>
								</svg>
								{repo.stargazers_count}
							</span>
							<span>Updated {formatDate(repo.pushed_at)}</span>
						</div>

						<div class="flex gap-3">
							<a
								href={repo.html_url}
								target="_blank"
								rel="noopener noreferrer"
								class="flex-1 border-2 border-black bg-black py-2 text-center font-semibold text-white transition-colors hover:bg-white hover:text-black"
							>
								View Code
							</a>
							{#if repo.homepage}
								<a
									href={repo.homepage}
									target="_blank"
									rel="noopener noreferrer"
									class="flex-1 border-2 border-black bg-white py-2 text-center font-semibold text-black transition-colors hover:bg-black hover:text-white"
								>
									Live Demo
								</a>
							{/if}
						</div>
					</div>
				</article>
			{/each}
		</div>

		<div class="mt-12 border-t-2 border-black pt-8 text-center">
			<p class="mb-4 text-gray-600">
				Want to see more? Check out my complete GitHub profile.
			</p>
			<a
				href={`https://github.com/${githubUsername}`}
				target="_blank"
				rel="noopener noreferrer"
				class="inline-block border-2 border-black bg-white px-8 py-3 font-semibold transition-colors hover:bg-black hover:text-white"
			>
				View All Repositories on GitHub
			</a>
		</div>
	{/if}
</div>

