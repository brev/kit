<script>
	import { page } from '$app/stores';
	import Icon from '@sveltejs/site-kit/components/Icon.svelte';
	import * as hovers from '$lib/docs/client/hovers.js';
	import OnThisPage from './OnThisPage.svelte';

	/** @type {import('./$types').PageData} */
	export let data;

	$: pages = data.sections.flatMap((section) => section.pages);
	$: index = pages.findIndex(({ path }) => path === $page.url.pathname);
	$: prev = pages[index - 1];
	$: next = pages[index + 1];

	hovers.setup();
</script>

<svelte:head>
	<title>{data.page.title} • Docs • SvelteKit</title>

	<meta name="twitter:title" content="SvelteKit docs" />
	<meta name="twitter:description" content="{data.page.title} • SvelteKit documentation" />
	<meta name="Description" content="{data.page.title} • SvelteKit documentation" />
</svelte:head>

<div class="text content">
	<h1>{data.page.title}</h1>

	<a class="edit" href="https://github.com/sveltejs/kit/edit/master/documentation/{data.page.file}">
		<Icon size={50} name="edit" /> Edit this page on GitHub
	</a>

	<section>
		{@html data.page.content}
	</section>

	<div class="controls">
		<div>
			<span class:faded={!prev}>previous</span>
			{#if prev}
				<a href={prev.path}>{prev.title}</a>
			{/if}
		</div>

		<div>
			<span class:faded={!next}>next</span>
			{#if next}
				<a href={next.path}>{next.title}</a>
			{/if}
		</div>
	</div>
</div>

<OnThisPage details={data.page} />

<style>
	.content {
		width: 100%;
		margin: 0;
	}

	.edit {
		position: relative;
		font-size: 1.4rem;
		line-height: 1;
		z-index: 2;
	}

	.edit :global(.icon) {
		position: relative;
		top: -0.1rem;
		left: 0.3rem;
		width: 1.4rem;
		height: 1.4rem;
		margin-right: 0.5rem;
	}

	.controls {
		max-width: calc(var(--sk-line-max-width) + 1rem);
		border-top: 1px solid var(--sk-back-4);
		padding: 1rem 0 0 0;
		display: grid;
		grid-template-columns: 1fr 1fr;
		margin: 6rem 0 0 0;
	}

	.controls > :first-child {
		text-align: left;
	}

	.controls > :last-child {
		text-align: right;
	}

	.controls span {
		display: block;
		font-size: 1.2rem;
		text-transform: uppercase;
		font-weight: 600;
		color: var(--sk-text-3);
	}

	.controls span.faded {
		opacity: 0.4;
	}
</style>
