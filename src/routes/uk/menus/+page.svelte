<script lang="ts">
	import { onMount } from 'svelte';

	const menus = [
		{ name: 'Main Menu', file: '/g_fusion_main_menu_jan26_web.pdf', color: '/lantern-gold.svg' },
		{ name: 'Asia Menu', file: '/geisha_asia_menu_a3_jan26_web.pdf', color: '/lantern-peach.svg' },
		{ name: 'Drinks', file: '/geisha_fusion_drinks_mar35_web.pdf', color: '/lantern-blue.svg' },
		{ name: 'Brunch', file: '/geisha_fusion_bottomless_brunch_nov25_web.pdf', color: '/lantern-lavender.svg' },
		{ name: 'Kids Menu', file: '/fusion_kids_menu_mar26.pdf', color: '/lantern-green.svg' },
		{ name: 'Allergy Menu', file: '/geisha_allergy_menu_feb26.pdf', color: '/lantern-pastel.svg' },
	];

	// Fixed lantern positions on the tree (hand-picked to sit nicely on branches)
	const lanternPositions = [
		{ x: 26, y: 20 },
		{ x: 55, y: 14 },
		{ x: 80, y: 22 },
		{ x: 16, y: 38 },
		{ x: 45, y: 34 },
		{ x: 74, y: 40 },
	];

	let noiseUrl = $state('');
	let lanternEls: HTMLDivElement[] = [];
	let mouseX = $state(0);
	let mouseY = $state(0);
	let mounted = $state(false);

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
				el.style.transform = `translateX(-50%) rotate(${angle}deg) scale(${1 + strength * 0.08})`;
			} else {
				el.style.transform = 'translateX(-50%) rotate(0deg) scale(1)';
			}
		}
	}

	function openMenu(file: string) {
		window.open(file, '_blank');
	}

	onMount(() => {
		mounted = true;

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
	});
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="menus-page" class:mounted onmousemove={handleMouseMove}>
	{#if noiseUrl}
		<div class="noise-overlay" style="background-image: url({noiseUrl});"></div>
	{/if}

	<!-- Header nav -->
	<nav>
		<a href="/uk" class="nav-logo">
			<img src="/cropped-g_fusion_logo.png" alt="Geisha Logo" />
		</a>
		<div class="login-btn">
			<a href="/login">Login</a>
		</div>
	</nav>

	<!-- Page title - top left -->
	<div class="page-title">
		<h1>Our Menus</h1>
		<p class="subtitle">Select a lantern to view</p>
		<ul class="menu-list">
			{#each menus as menu}
				<li><a href={menu.file} target="_blank" rel="noopener">{menu.name}</a></li>
			{/each}
		</ul>
	</div>

	<!-- Back arrow - vertically centered -->
	<a href="/uk" class="back-link" aria-label="Back to home">
		<svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="15 18 9 12 15 6"></polyline></svg>
	</a>

	<!-- Brush strokes -->
	<img class="brush-stroke brush-1" src="/brush-stroke-red.svg" alt="" aria-hidden="true" />
	<img class="brush-stroke brush-2" src="/brush-stroke-blue.svg" alt="" aria-hidden="true" />
	<img class="brush-stroke brush-3" src="/brush-stroke-yellow.svg" alt="" aria-hidden="true" />

	<!-- Tree with menu lanterns -->
	<div class="tree-wrapper">
		<img class="cherry-tree" src="/tree.svg" alt="Cherry blossom tree" />
		{#each menus as menu, i}
			<div
				bind:this={lanternEls[i]}
				class="lantern-pivot"
				style="left: {lanternPositions[i].x}%; top: {lanternPositions[i].y}%;"
			>
				<button
					class="lantern-btn"
					onclick={() => openMenu(menu.file)}
					aria-label="Open {menu.name}"
				>
					<img
						class="lantern"
						src={menu.color}
						alt=""
						aria-hidden="true"
						style="animation-delay: {0.8 + i * 0.2}s; --sway-duration: {3.5 + i * 0.7}s; --sway-delay: {0.8 + i * 0.2 + 1}s;"
					/>
					<span class="lantern-label">{menu.name}</span>
				</button>
			</div>
		{/each}
	</div>

	<!-- Falling petals -->
	<div class="falling-leaves">
		<svg class="leaf l1" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#f5ede3"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#f5ede3"/></svg>
		<svg class="leaf l2" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#f5ede3"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#f5ede3"/></svg>
		<svg class="leaf l3" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#f5ede3"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#f5ede3"/></svg>
		<svg class="leaf l4" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#f5ede3"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#f5ede3"/></svg>
		<svg class="leaf l5" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#f5ede3"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#f5ede3"/></svg>
		<svg class="leaf l6" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#f5ede3"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#f5ede3"/></svg>
		<svg class="leaf l7" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#f5ede3"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#f5ede3"/></svg>
		<svg class="leaf l8" viewBox="0 0 40 40"><path d="M20 0 C26 8 30 16 28 24 C26 32 22 36 20 40 C18 36 14 32 12 24 C10 16 14 8 20 0Z" fill="#f5ede3"/><path d="M0 20 C8 14 16 10 24 12 C32 14 36 18 40 20 C36 22 32 26 24 28 C16 30 8 26 0 20Z" fill="#f5ede3"/></svg>
	</div>

	<!-- Grass -->
	<div class="grass-border"></div>
</div>

<style>
	/* ===== Base ===== */
	.menus-page {
		min-height: 100vh;
		height: 100vh;
		background: #1a1a1a;
		color: #fff;
		font-family: system-ui, -apple-system, sans-serif;
		overflow: hidden;
		position: relative;
		opacity: 0;
		transition: opacity 0.6s ease;
	}
	.menus-page.mounted { opacity: 1; }

	.noise-overlay {
		position: absolute;
		inset: 0;
		z-index: 0;
		pointer-events: none;
		opacity: 0.15;
		background-repeat: repeat;
		background-size: 200px 200px;
		mix-blend-mode: soft-light;
	}

	/* ===== Nav ===== */
	nav {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 28px 36px;
		z-index: 20;
	}
	.back-link {
		position: fixed;
		left: 36px;
		top: 50%;
		transform: translateY(-50%);
		z-index: 10;
		color: #fff;
		text-decoration: none;
		width: 44px;
		height: 44px;
		border: 1px solid rgba(255,255,255,0.3);
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: all 0.3s;
	}
	.back-link:hover { border-color: #a96700; color: #a96700; }

	.nav-logo {
		display: inline-block;
	}
	.nav-logo img { max-height: 40px; width: auto; }

	.login-btn a {
		color: #fff;
		text-decoration: none;
		font-size: 12px;
		letter-spacing: 0.15em;
		text-transform: uppercase;
		border: 1px solid #fff;
		padding: 8px 20px;
		display: inline-block;
		transition: all 0.3s;
	}
	.login-btn a:hover {
		background-color: #a96700;
		border-color: #a96700;
	}

	/* ===== Title ===== */
	.page-title {
		position: fixed;
		top: 90px;
		left: 36px;
		z-index: 5;
		opacity: 0;
		animation: fadeInTitle 0.8s ease forwards;
		animation-delay: 0.4s;
	}
	h1 {
		font-family: 'Georgia', serif;
		font-size: 36px;
		font-weight: 300;
		letter-spacing: 0.12em;
		text-transform: uppercase;
		margin: 0;
		color: #f5ede3;
	}
	.subtitle {
		margin-top: 10px;
		font-size: 13px;
		letter-spacing: 0.2em;
		text-transform: uppercase;
		color: #a96700;
	}
	.menu-list {
		list-style: none;
		padding: 0;
		margin: 20px 0 0;
	}
	.menu-list li {
		padding: 6px 0;
	}
	.menu-list a {
		color: #999;
		text-decoration: none;
		font-size: 14px;
		letter-spacing: 0.1em;
		transition: color 0.3s;
	}
	.menu-list a:hover {
		color: #a96700;
	}

	/* ===== Brush strokes ===== */
	.brush-stroke {
		position: absolute;
		right: -5%;
		bottom: -18%;
		width: 145vh;
		height: auto;
		z-index: 0;
		opacity: 0.8;
		clip-path: inset(0 100% 0 0);
		animation: brushWipe 1.2s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards;
	}
	.brush-1 { right: -12%; bottom: -10%; animation-delay: 0.1s; }
	.brush-2 { right: -5%; bottom: -18%; animation-delay: 0.4s; }
	.brush-3 { right: 2%; bottom: -26%; animation-delay: 0.7s; }

	@keyframes brushWipe {
		from { clip-path: inset(0 100% 0 0); }
		to { clip-path: inset(0 0 0 0); }
	}

	/* ===== Tree ===== */
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
		filter: saturate(0) invert(1) sepia(0.4) saturate(0.3) brightness(0.85) hue-rotate(-10deg);
		opacity: 0;
		animation: fadeIn 1s ease forwards;
		animation-delay: 0.3s;
	}

	/* ===== Lanterns ===== */
	.lantern-pivot {
		position: absolute;
		width: 5.5%;
		transform-origin: top center;
		transform: translateX(-50%) rotate(0deg) scale(1);
		transition: transform 0.4s ease-out;
		z-index: 2;
	}
	.lantern-btn {
		background: none;
		border: none;
		padding: 0;
		cursor: pointer;
		display: flex;
		flex-direction: column;
		align-items: center;
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
		filter: drop-shadow(0 4px 16px rgba(255, 150, 50, 0.6));
		transition: filter 0.3s, transform 0.3s;
	}
	.lantern-btn:hover .lantern {
		filter: drop-shadow(0 6px 28px rgba(255, 150, 50, 0.9)) brightness(1.2);
		transform: scale(1.1);
	}
	.lantern-label {
		display: block;
		margin-top: 6px;
		font-size: 11px;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: #a96700;
		white-space: nowrap;
		text-shadow: 0 1px 4px rgba(0,0,0,0.8);
		opacity: 0;
		transform: translateY(4px);
		transition: opacity 0.3s, transform 0.3s;
	}
	.lantern-btn:hover .lantern-label {
		opacity: 1;
		transform: translateY(0);
	}

	@keyframes lanternAppear {
		from { opacity: 0; }
		to { opacity: 1; }
	}
	@keyframes lanternSway {
		0%, 100% { transform: rotate(0deg); }
		25% { transform: rotate(3deg); }
		75% { transform: rotate(-3deg); }
	}

	/* ===== Falling leaves ===== */
	.falling-leaves {
		position: absolute;
		top: 0; left: 0;
		width: 100%; height: 100%;
		pointer-events: none;
		z-index: 3;
		overflow: hidden;
		opacity: 0;
		animation: fadeIn 0.5s ease forwards;
		animation-delay: 1.5s;
	}
	.leaf {
		position: absolute;
		top: -30px;
		width: 18px; height: 18px;
		opacity: 0.6;
		animation: leafFall linear infinite;
	}
	.l1 { left: 45%; animation-duration: 8s; animation-delay: 0s; width: 16px; height: 16px; opacity: 0.5; }
	.l2 { left: 55%; animation-duration: 10s; animation-delay: 1.5s; width: 20px; height: 20px; opacity: 0.7; }
	.l3 { left: 62%; animation-duration: 7s; animation-delay: 0.5s; width: 15px; height: 15px; opacity: 0.55; }
	.l4 { left: 70%; animation-duration: 9s; animation-delay: 3s; width: 18px; height: 18px; opacity: 0.6; }
	.l5 { left: 78%; animation-duration: 11s; animation-delay: 2s; width: 14px; height: 14px; opacity: 0.5; }
	.l6 { left: 85%; animation-duration: 8.5s; animation-delay: 4s; width: 17px; height: 17px; opacity: 0.65; }
	.l7 { left: 92%; animation-duration: 9.5s; animation-delay: 1s; width: 21px; height: 21px; opacity: 0.7; }
	.l8 { left: 35%; animation-duration: 7.5s; animation-delay: 2.5s; width: 15px; height: 15px; opacity: 0.5; }

	@keyframes leafFall {
		0% { transform: translateY(0) translateX(0) rotate(0deg); opacity: 0; }
		5% { opacity: 0.6; }
		25% { transform: translateY(25vh) translateX(-30px) rotate(90deg); }
		50% { transform: translateY(50vh) translateX(-50px) rotate(200deg); }
		75% { transform: translateY(75vh) translateX(-25px) rotate(310deg); }
		95% { opacity: 0.4; }
		100% { transform: translateY(105vh) translateX(-60px) rotate(400deg); opacity: 0; }
	}

	/* ===== Grass ===== */
	.grass-border {
		position: fixed;
		bottom: 0; left: 0;
		width: 100%; height: 80px;
		z-index: 10;
		pointer-events: none;
		background: url('/grass.svg') repeat-x bottom left;
		background-size: auto 100%;
		filter: saturate(0) invert(1) sepia(0.4) saturate(0.3) brightness(0.85) hue-rotate(-10deg);
		opacity: 0;
		animation: fadeIn 1s ease forwards;
		animation-delay: 0.3s;
	}

	/* ===== Utility animations ===== */
	@keyframes fadeIn {
		from { opacity: 0; }
		to { opacity: 1; }
	}
	@keyframes fadeInTitle {
		from { opacity: 0; transform: translateY(-12px); }
		to { opacity: 1; transform: translateY(0); }
	}

	/* ===== Responsive ===== */
	@media (max-width: 1024px) {
		.tree-wrapper { width: 110vh; right: -15%; }
		.lantern-pivot { width: 7%; }
	}
	@media (max-width: 768px) {
		h1 { font-size: 28px; }
		nav { padding: 20px; }
		.page-title { left: 20px; top: 76px; }
		.back-link { left: 20px; }
		.tree-wrapper { width: 85vh; right: -25%; bottom: -15%; }
		.lantern-pivot { width: 9%; }
		.lantern-label { font-size: 9px; }
	}
	@media (max-width: 480px) {
		h1 { font-size: 24px; }
		.page-title { top: 70px; }
		.tree-wrapper { width: 75vh; right: -35%; }
		.lantern-pivot { width: 11%; }
	}
</style>
