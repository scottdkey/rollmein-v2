<script lang="ts">
	import { enhance } from '$app/forms';
	import { page } from '$app/stores';
	import type { SubmitFunction } from '@sveltejs/kit';
	import '../app.postcss';
	import { theme as Theme } from '../lib/stores/theme.store.js';
	import { get } from 'svelte/store';

	import { onDestroy } from 'svelte';

	const submitUpdateTheme: SubmitFunction = ({ action }) => {
		const theme = action.searchParams.get('theme');

		if (theme) {
			document.documentElement.setAttribute('data-theme', theme);
			Theme.set(theme);
		}
	};
	const unsubscribe = Theme.subscribe((val) => {
		console.log(val);
	});
	onDestroy(() => {
		unsubscribe();
	});

	const themes = [
		'light',
		'dark',
		'cupcake',
		'bumblebee',
		'emerald',
		'corporate',
		'synthwave',
		'retro',
		'cyberpunk',
		'valentine',
		'halloween',
		'garden',
		'forest',
		'aqua',
		'lofi',
		'pastel',
		'fantasy',
		'wireframe',
		'black',
		'luxury',
		'dracula',
		'cmyk',
		'autumn',
		'business',
		'acid',
		'lemonade',
		'night',
		'coffee',
		'winter'
	];
</script>

<div class="flex-1">
	<div>
		<h1>Rollmein</h1>
		<div>
			<div class="flex-none">
				<details class="dropdown">
					<summary class="btn">{$Theme}</summary>
					<ul class="dropdown-content bg-slate-200 z-[1] rounded-box w-52">
						<form method="POST" use:enhance={submitUpdateTheme} class="dropdown-content">
							{#each themes as theme}
								<li class="w-50">
									<button
										class="btn bg-primary-500 rounded-box"
										class:active={"bg-secondary-400"}
										formaction="/?/setTheme&theme={theme}&redirectTo={$page.url.pathname}"
										>{theme}</button
									>
								</li>
							{/each}
						</form>
					</ul>
				</details>
			</div>
		</div>
		<div class='site-spacing'>
			<slot />
		</div>
	</div>
</div>

<style>
	.site-spacing {
		margin: 4px;
		margin-top: 10px;
	}
</style>
