<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import { onNavigate } from '$app/navigation';
	import Lenis from 'lenis';
	import 'lenis/dist/lenis.css';
	import { onMount } from 'svelte';

	let { children } = $props();

	onMount(() => {
		const lenis = new Lenis({
			duration: 1.2,
			easing: (t: number) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
			smoothWheel: true
		});

		function raf(time: number) {
			lenis.raf(time);
			requestAnimationFrame(raf);
		}
		requestAnimationFrame(raf);

		return () => {
			lenis.destroy();
		};
	});

	onNavigate((navigation) => {
		if (!document.startViewTransition) return;

		const goingBack = navigation.to?.url.pathname === '/';
		document.documentElement.classList.toggle('back', goingBack);

		return new Promise((resolve) => {
			document.startViewTransition(async () => {
				resolve();
				await navigation.complete;
			});
		});
	});
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}

<style>
	@keyframes slide-up-out {
		from { transform: translateY(0); }
		to { transform: translateY(-100%); }
	}

	@keyframes slide-up-in {
		from { transform: translateY(100%); }
		to { transform: translateY(0); }
	}

	@keyframes slide-down-out {
		from { transform: translateY(0); }
		to { transform: translateY(100%); }
	}

	@keyframes slide-down-in {
		from { transform: translateY(-100%); }
		to { transform: translateY(0); }
	}

	/* Forward: old slides up, new pushes in from below */
	:global(::view-transition-old(root)) {
		animation: slide-up-out 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
	}

	:global(::view-transition-new(root)) {
		animation: slide-up-in 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
	}

	/* Back: old slides down, new pushes in from above */
	:global(html.back::view-transition-old(root)) {
		animation: slide-down-out 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
	}

	:global(html.back::view-transition-new(root)) {
		animation: slide-down-in 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
	}
</style>
