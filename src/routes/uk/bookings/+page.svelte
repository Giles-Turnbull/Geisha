<script lang="ts">
	import { onMount } from 'svelte';

	let noiseUrl = $state('');
	let mounted = $state(false);
	let submitted = $state(false);

	let form = $state({
		name: '',
		email: '',
		phone: '',
		date: '',
		time: '',
		guests: '2',
		occasion: '',
		notes: '',
	});

	const timeSlots = [
		'12:00', '12:30', '13:00', '13:30', '14:00', '14:30',
		'17:00', '17:30', '18:00', '18:30', '19:00', '19:30',
		'20:00', '20:30', '21:00', '21:30',
	];

	const occasions = ['', 'Birthday', 'Anniversary', 'Date Night', 'Business Dinner', 'Family Gathering', 'Other'];

	function handleSubmit(e: Event) {
		e.preventDefault();
		submitted = true;
	}

	// Today's date as min for date picker
	const today = new Date().toISOString().split('T')[0];

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
			data[i] = v; data[i + 1] = v; data[i + 2] = v; data[i + 3] = 255;
		}
		ctx.putImageData(imageData, 0, 0);
		noiseUrl = canvas.toDataURL('image/png');
	});
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="bookings-page" class:mounted>

	{#if noiseUrl}
		<div class="noise-overlay" style="background-image: url({noiseUrl});"></div>
	{/if}

	<!-- Nav -->
	<nav>
		<a href="/uk" class="nav-logo">
			<img src="/cropped-g_fusion_logo.png" alt="Geisha Logo" />
		</a>
		<div class="login-btn">
			<a href="/login">Login</a>
		</div>
	</nav>

	<!-- Brush strokes -->
	<img class="brush-stroke brush-1" src="/brush-stroke-red.svg" alt="" aria-hidden="true" />
	<img class="brush-stroke brush-2" src="/brush-stroke-blue.svg" alt="" aria-hidden="true" />
	<img class="brush-stroke brush-3" src="/brush-stroke-yellow.svg" alt="" aria-hidden="true" />

	<!-- Booking panel -->
	<div class="booking-panel">
		{#if !submitted}
			<div class="panel-header">
				<h1>Reserve a Table</h1>
				<div class="divider">
					<span class="line"></span>
					<span class="asterisk">✳</span>
					<span class="line"></span>
				</div>
				<p class="subtitle">Newcastle Upon Tyne &nbsp;·&nbsp; Front Street, Benton</p>
			</div>

			<form onsubmit={handleSubmit} novalidate>
				<div class="form-row">
					<div class="field">
						<label for="name">Full Name</label>
						<input id="name" type="text" placeholder="Your name" bind:value={form.name} required />
					</div>
					<div class="field">
						<label for="email">Email</label>
						<input id="email" type="email" placeholder="your@email.com" bind:value={form.email} required />
					</div>
				</div>

				<div class="form-row">
					<div class="field">
						<label for="phone">Phone</label>
						<input id="phone" type="tel" placeholder="07700 000000" bind:value={form.phone} required />
					</div>
					<div class="field">
						<label for="guests">Guests</label>
						<select id="guests" bind:value={form.guests}>
							{#each Array.from({length: 20}, (_, i) => String(i + 1)) as n}
								<option value={n}>{n} {n === '1' ? 'guest' : 'guests'}</option>
							{/each}
						</select>
					</div>
				</div>

				<div class="form-row">
					<div class="field">
						<label for="date">Date</label>
						<input id="date" type="date" min={today} bind:value={form.date} required />
					</div>
					<div class="field">
						<label for="time">Time</label>
						<select id="time" bind:value={form.time} required>
							<option value="" disabled>Select a time</option>
							{#each timeSlots as slot}
								<option value={slot}>{slot}</option>
							{/each}
						</select>
					</div>
				</div>

				<div class="field full">
					<label for="occasion">Occasion <span class="optional">(optional)</span></label>
					<select id="occasion" bind:value={form.occasion}>
						{#each occasions as o}
							<option value={o}>{o || 'No special occasion'}</option>
						{/each}
					</select>
				</div>

				<div class="field full">
					<label for="notes">Special Requests <span class="optional">(optional)</span></label>
					<textarea id="notes" rows="3" placeholder="Allergies, dietary requirements, high chair needed…" bind:value={form.notes}></textarea>
				</div>

				<div class="form-footer">
					<p class="note">For parties of 8 or more, please call us on <a href="tel:01913004641">0191 300 4641</a></p>
					<button type="submit" class="submit-btn">
						<span>Confirm Booking</span>
						<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/></svg>
					</button>
				</div>
			</form>
		{:else}
			<div class="success">
				<div class="success-icon">
					<svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><circle cx="12" cy="12" r="10"/><polyline points="9 12 11 14 15 10"/></svg>
				</div>
				<h2>Booking Received</h2>
				<div class="divider">
					<span class="line"></span>
					<span class="asterisk">✳</span>
					<span class="line"></span>
				</div>
				<p>Thank you, <strong>{form.name}</strong>. We've received your reservation request for <strong>{form.guests} {form.guests === '1' ? 'guest' : 'guests'}</strong> on <strong>{form.date}</strong> at <strong>{form.time}</strong>.</p>
				<p class="confirm-note">A confirmation will be sent to <strong>{form.email}</strong> shortly.</p>
				<div class="success-actions">
					<a href="/uk" class="btn-secondary">Back to Home</a>
					<button class="btn-ghost" onclick={() => { submitted = false; form = { name: '', email: '', phone: '', date: '', time: '', guests: '2', occasion: '', notes: '' }; }}>
						New Booking
					</button>
				</div>
			</div>
		{/if}
	</div>

</div>

<style>
	/* ===== Base ===== */
	:global(body) { margin: 0; }

	.bookings-page {
		min-height: 100vh;
		background: #1a1a1a;
		color: #fff;
		font-family: system-ui, -apple-system, sans-serif;
		overflow-x: hidden;
		position: relative;
		opacity: 0;
		transition: opacity 0.6s ease;
	}
	.bookings-page.mounted { opacity: 1; }

	.noise-overlay {
		position: fixed;
		inset: 0;
		z-index: 0;
		pointer-events: none;
		opacity: 0.12;
		background-repeat: repeat;
		background-size: 200px 200px;
		mix-blend-mode: soft-light;
	}

	/* ===== Nav ===== */
	nav {
		position: fixed;
		top: 0; left: 0;
		width: 100%;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 28px 36px;
		z-index: 20;
	}
	.nav-logo img { max-height: 40px; width: auto; }

	.login-btn a {
		color: #fff;
		text-decoration: none;
		font-size: 12px;
		letter-spacing: 0.15em;
		text-transform: uppercase;
		border: 1px solid rgba(255,255,255,0.4);
		padding: 8px 20px;
		display: inline-block;
		transition: all 0.3s;
	}
	.login-btn a:hover { background-color: #a96700; border-color: #a96700; }

	/* ===== Brush strokes ===== */
	.brush-stroke {
		position: fixed;
		right: -5%;
		bottom: -18%;
		width: 145vh;
		height: auto;
		z-index: 0;
		opacity: 0.7;
		clip-path: inset(0 100% 0 0);
		animation: brushWipe 1.2s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards;
		pointer-events: none;
	}
	.brush-1 { right: -12%; bottom: -10%; animation-delay: 0.1s; }
	.brush-2 { right: -5%; bottom: -18%; animation-delay: 0.4s; }
	.brush-3 { right: 2%; bottom: -26%; animation-delay: 0.7s; }

	@keyframes brushWipe {
		from { clip-path: inset(0 100% 0 0); }
		to { clip-path: inset(0 0 0 0); }
	}

	/* ===== Booking Panel ===== */
	.booking-panel {
		position: relative;
		z-index: 5;
		max-width: 600px;
		min-height: 100vh;
		padding: 120px 60px 100px 100px;
		display: flex;
		flex-direction: column;
		justify-content: center;
	}

	/* ===== Panel Header ===== */
	.panel-header { margin-bottom: 36px; }

	h1 {
		font-family: 'Georgia', serif;
		font-size: 38px;
		font-weight: 300;
		letter-spacing: 0.1em;
		text-transform: uppercase;
		margin: 0 0 16px;
		color: #f5ede3;
		opacity: 0;
		animation: fadeInUp 0.8s ease forwards;
		animation-delay: 0.3s;
	}

	.divider {
		display: flex;
		align-items: center;
		gap: 12px;
		margin-bottom: 14px;
	}
	.divider .line {
		flex: 1;
		max-width: 60px;
		height: 1px;
		background: rgba(255,255,255,0.15);
	}
	.divider .asterisk {
		color: #a96700;
		font-size: 12px;
	}

	.subtitle {
		font-size: 12px;
		letter-spacing: 0.18em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.35);
		margin: 0;
	}

	/* ===== Form ===== */
	form {
		opacity: 0;
		animation: fadeInUp 0.8s ease forwards;
		animation-delay: 0.5s;
	}

	.form-row {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 16px;
		margin-bottom: 16px;
	}

	.field {
		display: flex;
		flex-direction: column;
		gap: 7px;
	}
	.field.full { margin-bottom: 16px; }

	label {
		font-size: 11px;
		letter-spacing: 0.16em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.45);
	}

	.optional {
		letter-spacing: 0;
		text-transform: none;
		color: rgba(255,255,255,0.25);
		font-size: 10px;
	}

	input, select, textarea {
		background: rgba(255,255,255,0.05);
		border: 1px solid rgba(255,255,255,0.1);
		border-radius: 2px;
		color: #f5ede3;
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 14px;
		padding: 11px 14px;
		outline: none;
		transition: border-color 0.25s, background 0.25s;
		width: 100%;
		box-sizing: border-box;
	}
	input::placeholder, textarea::placeholder {
		color: rgba(255,255,255,0.2);
	}
	input:focus, select:focus, textarea:focus {
		border-color: #a96700;
		background: rgba(255,255,255,0.08);
	}
	select {
		cursor: pointer;
		appearance: none;
		background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%23a96700' stroke-width='1.5' fill='none'/%3E%3C/svg%3E");
		background-repeat: no-repeat;
		background-position: right 14px center;
		padding-right: 36px;
	}
	select option {
		background: #2a2a2a;
		color: #f5ede3;
	}
	input[type="date"]::-webkit-calendar-picker-indicator {
		filter: invert(0.5) sepia(1) saturate(3) hue-rotate(10deg);
		cursor: pointer;
	}
	textarea { resize: vertical; min-height: 80px; }

	/* ===== Form Footer ===== */
	.form-footer {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 20px;
		margin-top: 24px;
		flex-wrap: wrap;
	}

	.note {
		font-size: 12px;
		color: rgba(255,255,255,0.3);
		margin: 0;
		line-height: 1.6;
	}
	.note a {
		color: #a96700;
		text-decoration: none;
	}
	.note a:hover { text-decoration: underline; }

	.submit-btn {
		display: inline-flex;
		align-items: center;
		gap: 10px;
		background: transparent;
		border: 1px solid #a96700;
		color: #a96700;
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 12px;
		letter-spacing: 0.18em;
		text-transform: uppercase;
		padding: 12px 28px;
		cursor: pointer;
		transition: all 0.3s;
		white-space: nowrap;
	}
	.submit-btn:hover {
		background: #a96700;
		color: #fff;
	}

	/* ===== Success ===== */
	.success {
		opacity: 0;
		animation: fadeInUp 0.8s ease forwards;
		animation-delay: 0.1s;
	}

	.success-icon {
		color: #a96700;
		margin-bottom: 20px;
	}

	h2 {
		font-family: 'Georgia', serif;
		font-size: 32px;
		font-weight: 300;
		letter-spacing: 0.1em;
		text-transform: uppercase;
		margin: 0 0 16px;
		color: #f5ede3;
	}

	.success p {
		color: rgba(255,255,255,0.6);
		font-size: 15px;
		line-height: 1.7;
		margin: 0 0 10px;
	}
	.success p strong { color: #f5ede3; }

	.confirm-note {
		font-size: 13px;
		color: rgba(255,255,255,0.35) !important;
		margin-top: 6px !important;
	}

	.success-actions {
		display: flex;
		gap: 16px;
		margin-top: 32px;
		flex-wrap: wrap;
	}

	.btn-secondary {
		display: inline-block;
		background: transparent;
		border: 1px solid #a96700;
		color: #a96700;
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 12px;
		letter-spacing: 0.18em;
		text-transform: uppercase;
		padding: 12px 28px;
		text-decoration: none;
		transition: all 0.3s;
	}
	.btn-secondary:hover { background: #a96700; color: #fff; }

	.btn-ghost {
		display: inline-block;
		background: transparent;
		border: 1px solid rgba(255,255,255,0.15);
		color: rgba(255,255,255,0.45);
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 12px;
		letter-spacing: 0.18em;
		text-transform: uppercase;
		padding: 12px 28px;
		cursor: pointer;
		transition: all 0.3s;
	}
	.btn-ghost:hover { border-color: rgba(255,255,255,0.4); color: rgba(255,255,255,0.8); }

	/* ===== Animations ===== */
	@keyframes fadeIn {
		from { opacity: 0; }
		to { opacity: 1; }
	}
	@keyframes fadeInUp {
		from { opacity: 0; transform: translateY(16px); }
		to { opacity: 1; transform: translateY(0); }
	}

	/* ===== Responsive ===== */
	@media (max-width: 768px) {
		nav { padding: 20px; }
		.booking-panel { padding: 100px 28px 100px 28px; max-width: 100%; }
		.form-row { grid-template-columns: 1fr; }
		h1 { font-size: 30px; }
		.form-footer { flex-direction: column; align-items: flex-start; }
	}
	@media (max-width: 480px) {
		.booking-panel { padding: 90px 20px 90px 20px; }
		h1 { font-size: 26px; }
	}
</style>
