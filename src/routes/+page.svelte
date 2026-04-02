<script lang="ts" module>
	let hasVisited = false;
</script>

<script lang="ts">
	import { onMount } from 'svelte';

	let theme = $state<'light' | 'dark'>('light');
	let skipAnimations = hasVisited;
	let noiseUrl = $state('');

	// Flower positions on the tree SVG (percentage of 2000x2000 viewBox)
	const flowerPositions = [
		{ x: 82, y: 20 }, { x: 16, y: 20 }, { x: 45, y: 26 },
		{ x: 27, y: 17 }, { x: 42, y: 12 }, { x: 67, y: 20 },
		{ x: 26, y: 45 }, { x: 53, y: 34 }, { x: 14, y: 31 },
		{ x: 45, y: 37 }, { x: 12, y: 43 }, { x: 74, y: 44 },
		{ x: 55, y: 20 }, { x: 85, y: 44 }, { x: 43, y: 59 },
		{ x: 38, y: 66 }, { x: 58, y: 9 },  { x: 85, y: 32 },
		{ x: 93, y: 40 }, { x: 75, y: 15 }, { x: 36, y: 25 },
		{ x: 63, y: 45 }
	];

	const lanternColors = [
		'/lantern-pastel.svg',
		'/lantern-blue.svg',
		'/lantern-green.svg',
		'/lantern-gold.svg',
		'/lantern-lavender.svg',
		'/lantern-peach.svg'
	];

	// Pick 3-6 random flower positions for lanterns, each with a different color
	function pickLanterns() {
		const count = 3 + Math.floor(Math.random() * 4); // 3 to 6
		const shuffledPositions = [...flowerPositions].sort(() => Math.random() - 0.5);
		const shuffledColors = [...lanternColors].sort(() => Math.random() - 0.5);
		return shuffledPositions.slice(0, count).map((pos, i) => ({
			...pos,
			src: shuffledColors[i % shuffledColors.length]
		}));
	}

	let lanterns = $state(pickLanterns());
	let lanternEls: HTMLDivElement[] = [];
	let mouseX = $state(0);
	let mouseY = $state(0);

	function handleMouseMove(e: MouseEvent) {
		mouseX = e.clientX;
		mouseY = e.clientY;

		for (const el of lanternEls) {
			if (!el) continue;
			const rect = el.getBoundingClientRect();
			const elCenterX = rect.left + rect.width / 2;
			const elCenterY = rect.top + rect.height / 3;
			const dx = mouseX - elCenterX;
			const dy = mouseY - elCenterY;
			const dist = Math.sqrt(dx * dx + dy * dy);
			const radius = 200;

			if (dist < radius) {
				const strength = 1 - dist / radius;
				const direction = dx < 0 ? 1 : -1;
				const angle = direction * strength * 12;
				el.style.transform = `translateX(-50%) rotate(${angle}deg) scale(${1 + strength * 0.05})`;
			} else {
				el.style.transform = 'translateX(-50%) rotate(0deg) scale(1)';
			}
		}
	}

	onMount(() => {
		hasVisited = true;
		document.documentElement.classList.add('landing-active');
		if (skipAnimations) {
			document.documentElement.classList.add('skip-animations');
		}

		// Generate noise texture via canvas
		const canvas = document.createElement('canvas');
		const ctx = canvas.getContext('2d')!;
		canvas.width = 200;
		canvas.height = 200;
		const imageData = ctx.createImageData(200, 200);
		const data = imageData.data;
		for (let i = 0; i < data.length; i += 4) {
			const v = Math.random() * 255;
			data[i] = v;
			data[i + 1] = v;
			data[i + 2] = v;
			data[i + 3] = 255;
		}
		ctx.putImageData(imageData, 0, 0);
		noiseUrl = canvas.toDataURL('image/png');

		return () => {
			document.documentElement.classList.remove('landing-active', 'skip-animations');
		};
	});
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="landing {theme}" onmousemove={handleMouseMove}>
	{#if noiseUrl}
		<div class="noise-overlay" style="background-image: url({noiseUrl});"></div>
	{/if}
	<!-- Bricks -->
	<img class="bricks-svg bricks-left" src="/bricks.svg" alt="" aria-hidden="true" />
	<img class="bricks-svg bricks-right" src="/bricks.svg" alt="" aria-hidden="true" />

	<!-- Brush strokes behind tree -->
	<img class="brush-stroke brush-1" src={theme === 'light' ? '/brush-stroke-blue.svg' : '/brush-stroke-red.svg'} alt="" aria-hidden="true" />
	<img class="brush-stroke brush-2" src={theme === 'light' ? '/brush-stroke-yellow.svg' : '/brush-stroke-blue.svg'} alt="" aria-hidden="true" />
	<img class="brush-stroke brush-3" src={theme === 'light' ? '/brush-stroke-red.svg' : '/brush-stroke-yellow.svg'} alt="" aria-hidden="true" />
	<!-- Cherry Blossom Tree with Lanterns -->
	<div class="tree-wrapper">
		<img class="cherry-tree" src="/tree.svg" alt="Cherry blossom tree" />
		{#each lanterns as pos, i}
			<div
				bind:this={lanternEls[i]}
				class="lantern-pivot"
				style="left: {pos.x}%; top: {pos.y}%;"
			>
				<img
					class="lantern"
					src={pos.src}
					alt=""
					aria-hidden="true"
					style="animation-delay: {1.2 + i * 0.3}s; --sway-duration: {3.5 + i * 0.7}s; --sway-delay: {1.2 + i * 0.3 + 1}s;"
				/>
			</div>
		{/each}
	</div>

	<!-- Content -->
	<div class="content">
		<div class="choose-label">
			<span class="label-line"></span>
			<span>Choose restaurant location</span>
			<span class="label-line"></span>
		</div>
		<a href="/uk" class="location-btn {theme === 'light' ? 'active' : ''}" onmouseenter={() => theme = 'light'}>UK</a>
		<img class="logo" src="/geisha-logo_2.png" alt="Geisha logo" />
		<a href="/thailand" class="location-btn {theme === 'dark' ? 'active' : ''}" onmouseenter={() => theme = 'dark'}>Thailand</a>
	</div>


<!-- Falling blossom petals -->
	<div class="falling-leaves">
		<svg class="leaf l1" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#1a1a1a"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#1a1a1a"/></svg>
		<svg class="leaf l2" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#1a1a1a"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#1a1a1a"/></svg>
		<svg class="leaf l3" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#1a1a1a"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#1a1a1a"/></svg>
		<svg class="leaf l4" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#1a1a1a"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#1a1a1a"/></svg>
		<svg class="leaf l5" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#1a1a1a"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#1a1a1a"/></svg>
		<svg class="leaf l6" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#1a1a1a"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#1a1a1a"/></svg>
		<svg class="leaf l7" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#1a1a1a"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#1a1a1a"/></svg>
		<svg class="leaf l8" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#1a1a1a"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#1a1a1a"/></svg>
		<svg class="leaf l9" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#1a1a1a"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#1a1a1a"/></svg>
		<svg class="leaf l10" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#1a1a1a"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#1a1a1a"/></svg>
	</div>

</div>

<!-- Grass border -->
<div
	class="grass-border {theme}"
	style="position:fixed; bottom:0; left:0; width:100%; height:80px; z-index:9999; pointer-events:none; background:url('/grass.svg') repeat-x bottom left; background-size:auto 100%; transition:filter 0.8s ease 0s;"
></div>

<style>
	:global(html.landing-active, html.landing-active body) {
		margin: 0;
		padding: 0;
		overflow: hidden;
		height: 100%;
		background: #1a1a1a;
	}

	.landing {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
		gap: 3rem;
		background: #f5ede3;
		overflow: hidden;
		position: relative;
		box-shadow: inset 0 0 0 12px white;
		transition: background 0.8s ease 0.3s, box-shadow 0.8s ease 0.3s;
	}

	.noise-overlay {
		position: absolute;
		inset: 12px;
		z-index: 0;
		pointer-events: none;
		opacity: 0.18;
		background-repeat: repeat;
		background-size: 200px 200px;
		mix-blend-mode: multiply;
		transition: opacity 0.8s ease 0.3s, mix-blend-mode 0.8s ease 0.3s;
	}

	.dark .noise-overlay {
		opacity: 0.15;
		mix-blend-mode: soft-light;
	}


	.landing.dark {
		background: #1a1a1a;
		box-shadow: inset 0 0 0 12px #111;
	}

	.bricks-svg {
		position: absolute;
		top: 12px;
		width: 500px;
		z-index: 0;
		pointer-events: none;
	}
	.bricks-left {
		left: 12px;
	}
	.bricks-right {
		right: 12px;
		transform: scaleX(-1);
	}

	.content {
		position: absolute;
		bottom: 7rem;
		left: 3rem;
		display: flex;
		align-items: center;
		gap: 2rem;
		z-index: 2;
		opacity: 0;
		transform: translateY(-30px);
		animation: fadeInDown 0.8s ease forwards;
		animation-delay: 1.8s;
	}

	.logo {
		width: 350px;
		height: auto;
		filter: invert(1);
		transition: filter 0.8s ease 0s;
	}

	.dark .logo {
		filter: invert(0);
	}

	.choose-label {
		position: absolute;
		top: -2.5rem;
		right: 0;
		margin: 0;
		display: flex;
		align-items: center;
		gap: 1rem;
		font-size: 0.85rem;
		font-weight: 400;
		letter-spacing: 0.15em;
		text-transform: uppercase;
		color: #1a1a1a;
		transition: color 0.8s ease 0.3s;
	}

	.label-line {
		display: block;
		width: 40px;
		height: 1px;
		background: #1a1a1a;
		transition: background 0.8s ease 0.3s;
	}

	.dark .choose-label {
		color: #f5ede3;
	}

	.dark .label-line {
		background: #f5ede3;
	}

	.location-btn {
		min-width: 140px;
		padding: 1.2rem 3rem;
		font-size: 1.1rem;
		font-weight: 500;
		letter-spacing: 0.15em;
		text-transform: uppercase;
		text-decoration: none;
		color: #1a1a1a;
		background: transparent;
		border: 2px solid #1a1a1a;
		border-radius: 0;
		text-align: center;
		transition: all 0.8s ease 0.6s;
		position: relative;
		overflow: hidden;
	}

	.location-btn.active {
		background: #1a1a1a;
		color: #f5ede3;
	}

	.dark .location-btn {
		color: #f5ede3;
		border-color: #f5ede3;
	}

	.dark .location-btn.active {
		background: #f5ede3;
		color: #1a1a1a;
	}

	.brush-stroke {
		position: absolute;
		right: -5%;
		bottom: -18%;
		width: 145vh;
		height: auto;
		z-index: 0;
		opacity: 0.75;
		transition: opacity 0.8s ease 0s;
		clip-path: inset(0 100% 0 0);
		animation: brushWipe 1.2s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards;
	}

	.brush-1 {
		right: -12%;
		bottom: -10%;
		animation-delay: 0.3s;
	}

	.brush-2 {
		right: -5%;
		bottom: -18%;
		animation-delay: 0.7s;
	}

	.brush-3 {
		right: 2%;
		bottom: -26%;
		animation-delay: 1.1s;
	}

	.dark .brush-stroke {
		opacity: 0.8;
	}

@keyframes brushWipe {
		0% {
			clip-path: inset(0 100% 0 0);
		}
		100% {
			clip-path: inset(0 0 0 0);
		}
	}

	@keyframes fadeIn {
		0% { opacity: 0; }
		100% { opacity: 1; }
	}

	@keyframes fadeInDown {
		0% {
			opacity: 0;
			transform: translateY(-30px);
		}
		100% {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.tree-wrapper {
		position: absolute;
		right: -5%;
		bottom: -10%;
		width: 140vh;
		z-index: 1;
	}

	.cherry-tree {
		width: 100%;
		height: auto;
		display: block;
		filter: saturate(0) brightness(0.15);
		transition: filter 0.8s ease 0s;
		opacity: 0;
		animation: fadeIn 1s ease forwards;
		animation-delay: 0.6s;
	}

	.dark .cherry-tree {
		filter: saturate(0) invert(1) sepia(0.4) saturate(0.3) brightness(0.85) hue-rotate(-10deg);
	}

	.lantern-pivot {
		position: absolute;
		width: 4%;
		transform-origin: top center;
		transform: translateX(-50%) rotate(0deg) scale(1);
		transition: transform 0.4s ease-out;
		z-index: 2;
	}

	.lantern {
		width: 100%;
		height: auto;
		display: block;
		transform-origin: top center;
		opacity: 0;
		animation:
			lanternAppear 1s ease forwards,
			lanternSway var(--sway-duration, 4s) ease-in-out var(--sway-delay, 2.2s) infinite;
		filter: drop-shadow(0 4px 12px rgba(255, 100, 50, 0.4));
		transition: filter 0.8s ease 0s;
	}

	.dark .lantern {
		filter: drop-shadow(0 4px 16px rgba(255, 150, 50, 0.6));
	}

	@keyframes lanternAppear {
		0% {
			opacity: 0;
			transform: rotate(0deg);
		}
		100% {
			opacity: 1;
			transform: rotate(0deg);
		}
	}

	@keyframes lanternSway {
		0%, 100% {
			transform: rotate(0deg);
		}
		25% {
			transform: rotate(3deg);
		}
		75% {
			transform: rotate(-3deg);
		}
	}

/* Falling leaves */
	.falling-leaves {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		pointer-events: none;
		z-index: 3;
		overflow: hidden;
		opacity: 0;
		animation: fadeIn 0.5s ease forwards;
		animation-delay: 2s;
	}

	.leaf {
		position: absolute;
		top: -30px;
		width: 18px;
		height: 18px;
		opacity: 0.6;
		animation: leafFall linear infinite;
		transition: filter 0.8s ease 0s;
	}

	.dark .leaf {
		filter: invert(1);
	}

	.l1  { left: 45%;  animation-duration: 8s;   animation-delay: 0s;    width: 16px; height: 16px; opacity: 0.5; }
	.l2  { left: 55%;  animation-duration: 10s;  animation-delay: 1.5s;  width: 20px; height: 20px; opacity: 0.7; }
	.l3  { left: 62%;  animation-duration: 7s;   animation-delay: 0.5s;  width: 15px; height: 15px; opacity: 0.55; }
	.l4  { left: 70%;  animation-duration: 9s;   animation-delay: 3s;    width: 18px; height: 18px; opacity: 0.6; }
	.l5  { left: 78%;  animation-duration: 11s;  animation-delay: 2s;    width: 14px; height: 14px; opacity: 0.5; }
	.l6  { left: 85%;  animation-duration: 8.5s; animation-delay: 4s;    width: 17px; height: 17px; opacity: 0.65; }
	.l7  { left: 92%;  animation-duration: 9.5s; animation-delay: 1s;    width: 21px; height: 21px; opacity: 0.7; }
	.l8  { left: 35%;  animation-duration: 7.5s; animation-delay: 2.5s;  width: 15px; height: 15px; opacity: 0.5; }
	.l9  { left: 50%;  animation-duration: 10.5s; animation-delay: 5s;   width: 13px; height: 13px; opacity: 0.45; }
	.l10 { left: 68%;  animation-duration: 8s;   animation-delay: 3.5s;  width: 19px; height: 19px; opacity: 0.6; }

	@keyframes leafFall {
		0% {
			transform: translateY(0) translateX(0) rotate(0deg);
			opacity: 0;
		}
		5% {
			opacity: 0.6;
		}
		25% {
			transform: translateY(25vh) translateX(-30px) rotate(90deg);
		}
		50% {
			transform: translateY(50vh) translateX(-50px) rotate(200deg);
		}
		75% {
			transform: translateY(75vh) translateX(-25px) rotate(310deg);
		}
		95% {
			opacity: 0.4;
		}
		100% {
			transform: translateY(105vh) translateX(-60px) rotate(400deg);
			opacity: 0;
		}
	}

	@media (max-width: 768px) {
		.content {
			flex-direction: column;
			gap: 2rem;
		}
		.logo {
			width: 220px;
		}
		.tree-wrapper {
			width: 70vh;
			right: -25%;
			bottom: -20%;
		}
	}

	.grass-border {
		position: fixed;
		bottom: 0;
		left: 0;
		width: 100%;
		height: 80px;
		z-index: 100;
		pointer-events: none;
		background: url('/grass.svg') repeat-x bottom left;
		background-size: auto 100%;
		opacity: 0;
		animation: fadeIn 1s ease forwards;
		animation-delay: 0.6s;
	}

	.grass-border.dark {
		filter: saturate(0) invert(1) sepia(0.4) saturate(0.3) brightness(0.85) hue-rotate(-10deg);
	}

	:global(html.skip-animations) .brush-stroke,
	:global(html.skip-animations) .cherry-tree,
	:global(html.skip-animations) .lantern,
	:global(html.skip-animations) .content,
	:global(html.skip-animations) .falling-leaves,
	:global(html.skip-animations) .grass-border {
		animation: none !important;
		opacity: 1 !important;
		clip-path: none !important;
		transform: none !important;
		transition: none !important;
	}

	:global(html.skip-animations) .lantern {
		animation: lanternSway var(--sway-duration, 4s) ease-in-out 0s infinite !important;
		opacity: 1 !important;
	}

	:global(html.skip-animations) .brush-stroke {
		opacity: 0.75 !important;
	}
	:global(html.skip-animations) .dark .brush-stroke {
		opacity: 0.8 !important;
	}
</style>
