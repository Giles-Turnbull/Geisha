<svelte:head>
	<link href="https://fonts.googleapis.com/css2?family=Cabin&family=Herr+Von+Muellerhoff&family=Source+Sans+Pro&display=swap" rel="stylesheet">
	<link href="https://fonts.cdnfonts.com/css/electroharmonix" rel="stylesheet">
	<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.0/css/all.css" integrity="sha384-lKuwvrZot6UHsBSfcMvOkWwlCMgc0TaWr+30HWe3a4ltaBwTZhyTEggF5tJv8tbt" crossorigin="anonymous">
</svelte:head>

<div class="thailand-page">

	<!-- Coming Soon Overlay -->
	<div class="coming-soon-overlay">
		<div class="coming-soon-content">
			<img src="/cropped-g_fusion_logo.png" alt="Geisha Logo" class="coming-soon-logo">
			<div class="coming-soon-divider">
				<span class="line"></span>
				<i class="fas fa-asterisk"></i>
				<span class="line"></span>
			</div>
			<span class="location">Thailand</span>
			<h2>Coming Soon</h2>
			<p>We're bringing Geisha Fusion to Thailand. Stay tuned.</p>
			<a href="/" class="back-link">Back to Home</a>
		</div>
	</div>

	<!-- Greyed-out UK page structure beneath -->
	<div class="page-content" aria-hidden="true">

		<!--Header-->
		<header>
			<nav>
				<div class="login-btn">
					<span class="fake-link">Login</span>
				</div>
				<div class="logo">
					<span class="fake-link"><img src="/cropped-g_fusion_logo.png" alt=""></span>
				</div>
				<div class="navigation-bar">
					<ul>
						<li><span class="fake-link">Menus<span class="underline"></span></span></li>
						<li><span class="fake-link">Bookings<span class="underline"></span></span></li>
						<li><span class="fake-link">Takeaway<span class="underline"></span></span></li>
						<li><span class="fake-link">Vouchers<span class="underline"></span></span></li>
						<li><span class="fake-link">Contact<span class="underline"></span></span></li>
					</ul>
				</div>
			</nav>
			<div class="text">
				<img class="hero-logo" src="/cropped-g_fusion_logo.png" alt="">
				<div class="hero-divider">
					<span class="line"></span>
					<i class="fas fa-asterisk"></i>
					<span class="line"></span>
				</div>
				<span>Thailand</span>
				<div class="button"><button>Explore</button></div>
			</div>
		</header>

		<!--About Us-->
		<div class="about-us">
			<div class="text">
				<h2>Welcome to</h2>
				<h3>Geisha Fusion</h3>
				<div><i class="fas fa-asterisk"></i></div>
				<p>Embark on a delightful culinary journey at Geisha Fusion! Our family-friendly restaurant presents a vibrant array of dishes from across the globe.</p>
				<p>Our diverse menu blends classic favourites with modern twists, inspired by a variety of international cuisines.</p>
				<div><span class="a-CTA">About Us</span></div>
			</div>
			<div class="image-container">
				<div class="image image1">
					<img src="/unnamed (1).webp" alt="">
				</div>
				<div class="image image2">
					<img src="/unnamed (2).webp" alt="">
				</div>
			</div>
		</div>

		<!--Recipes-->
		<div class="recipes">
			<div class="recipes-image-container">
				<div class="image" style="background-image: url('/food-1.webp')"></div>
				<div class="image" style="background-image: url('/food-2.webp')"></div>
				<div class="image" style="background-image: url('/food-3.webp')"></div>
			</div>
			<div class="text">
				<h2>Global</h2>
				<h3>Flavours</h3>
			</div>
		</div>

		<!--Menu-->
		<div class="menu">
			<div class="menu-image-container">
				<div class="image">
					<img src="/food-1.webp" alt="">
				</div>
				<div class="image">
					<img src="/food-2.webp" alt="">
				</div>
			</div>
			<div class="text">
				<h2>Discover</h2>
				<h3>Menu</h3>
				<div><i class="fas fa-asterisk"></i></div>
				<p>For those with pure food indulgence in mind, explore our ever-changing internationally and seasonally inspired dishes.</p>
				<div><span class="a-CTA">View The Full Menu</span></div>
			</div>
		</div>

		<!--Fixed Image-->
		<div class="fixed-image">
			<div class="text">
				<h2>The Perfect</h2>
				<h3>Blend</h3>
			</div>
		</div>

		<!--Reservation-->
		<div class="reservation">
			<div class="text">
				<h2>Culinary</h2>
				<h3>Delight</h3>
				<div><i class="fas fa-asterisk"></i></div>
				<p>We promise an intimate and relaxed dining experience that offers something different to local and foreign patrons.</p>
				<div><span class="a-CTA">Make a Reservation</span></div>
			</div>
			<div class="image-container">
				<div class="image image1">
					<img src="/unnamed (1).webp" alt="">
				</div>
				<div class="image image2">
					<img src="/unnamed (2).webp" alt="">
				</div>
			</div>
		</div>

		<!--Footer-->
		<footer>
			<div class="footer-grid">
				<div class="footer-col">
					<h3>Visit</h3>
					<hr>
					<p>Geisha Fusion</p>
					<p>Thailand</p>
				</div>
				<div class="footer-col">
					<h3>Contact Us</h3>
					<hr>
					<p>Coming Soon</p>
				</div>
				<div class="footer-col">
					<h3>Opening Times</h3>
					<hr>
					<p>Coming Soon</p>
				</div>
				<div class="footer-col">
					<h3>Essentials</h3>
					<hr>
					<span class="fake-link">Takeaway</span>
					<span class="fake-link">Book A Table</span>
					<span class="fake-link">Gift Voucher</span>
					<span class="fake-link">Contact Us</span>
				</div>
			</div>
		</footer>

		<!--Copyright-->
		<div class="copyright">
			<ul class="info">
				<li>&copy; Geisha Fusion 2026</li>
				<li>Thailand</li>
			</ul>
		</div>

	</div>
</div>

<style>
	:global(body) {
		margin: 0;
		overflow: hidden;
	}

	.thailand-page {
		position: relative;
		width: 100%;
	}

	/* ── Coming Soon Overlay ── */
	.coming-soon-overlay {
		position: fixed;
		inset: 0;
		z-index: 100;
		display: flex;
		align-items: center;
		justify-content: center;
		background: rgba(10, 10, 10, 0.72);
		backdrop-filter: blur(3px);
	}

	.coming-soon-content {
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
		padding: 60px 40px;
		width: 100%;
		max-width: 600px;
		overflow: hidden;
	}

	.coming-soon-logo {
		max-width: 180px;
		filter: invert(1) brightness(0.7);
		margin-bottom: 20px;
	}

	.coming-soon-divider {
		display: flex;
		align-items: center;
		gap: 15px;
		margin-bottom: 16px;
		color: #7a5a3a;
	}

	.coming-soon-divider .line {
		display: block;
		width: 60px;
		height: 1px;
		background: rgba(255, 255, 255, 0.3);
	}

	.coming-soon-divider .fa-asterisk {
		font-size: 10px;
		color: #7a5a3a;
	}

	.coming-soon-content .location {
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 13px;
		letter-spacing: 0.2em;
		text-transform: uppercase;
		color: rgba(255, 255, 255, 0.5);
		margin-bottom: 20px;
	}

	.coming-soon-content h2 {
		font-family: 'Electroharmonix', sans-serif;
		font-size: clamp(40px, 10vw, 110px);
		font-weight: normal;
		color: rgba(255, 255, 255, 0.15);
		margin: 0 0 -40px;
		line-height: 1;
		width: 100%;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.coming-soon-content p {
		font-family: system-ui, -apple-system, sans-serif;
		color: rgba(255, 255, 255, 0.4);
		font-size: 15px;
		line-height: 1.7;
		margin: 50px 0 30px;
		max-width: 320px;
	}

	.back-link {
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 12px;
		letter-spacing: 0.15em;
		text-transform: uppercase;
		color: rgba(255, 255, 255, 0.5);
		text-decoration: none;
		border: 1px solid rgba(255, 255, 255, 0.2);
		padding: 10px 28px;
		transition: all 0.3s;
	}

	.back-link:hover {
		border-color: rgba(255, 255, 255, 0.5);
		color: rgba(255, 255, 255, 0.85);
	}

	/* ── Page content (greyed-out backdrop) ── */
	.page-content {
		filter: grayscale(1) brightness(0.35);
		pointer-events: none;
		user-select: none;
	}

	.page-content * {
		box-sizing: border-box;
	}

	.page-content .fake-link {
		text-decoration: none;
	}

	.page-content ul {
		margin: 0;
		padding: 0;
		list-style: none;
	}

	.page-content img {
		max-width: 100%;
	}

	.page-content h2 {
		font-family: 'Electroharmonix', sans-serif;
		font-size: 100px;
		font-weight: normal;
		margin: 0 0 -55px;
		color: #a96700;
	}

	.page-content h3 {
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 47px;
		font-weight: bold;
		letter-spacing: 9.4px;
		margin: 0 0 15px;
		color: #fff;
	}

	.page-content p {
		font-family: system-ui, -apple-system, sans-serif;
		color: #515151;
		line-height: 27px;
	}

	.page-content .a-CTA {
		border-bottom: 2px solid #a96700;
		padding-bottom: 4px;
		letter-spacing: 1.5px;
		font-size: 18px;
		color: #a96700;
	}

	/* Header */
	header {
		height: 100vh;
		background: url('/geisha_italia_booking_banner.jpg') fixed bottom / cover;
		padding: 20px 70px;
		position: relative;
	}

	header nav {
		display: flex;
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		padding: 36px 140px 20px 36px;
		z-index: 20;
		background-color: #f5ede3;
	}

	header nav .login-btn {
		position: absolute;
		right: 36px;
		top: 36px;
	}

	header nav .login-btn .fake-link {
		color: #2c2c2c;
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 12px;
		letter-spacing: 0.15em;
		text-transform: uppercase;
		border: 1px solid #2c2c2c;
		padding: 8px 20px;
		display: inline-block;
	}

	header nav .logo {
		flex-basis: 56%;
		padding-left: 50px;
	}

	header nav .logo img {
		max-height: 50px;
		width: auto;
		filter: invert(1);
	}

	header .navigation-bar ul {
		display: flex;
		margin: 0;
		padding: 0;
		list-style: none;
	}

	header .navigation-bar ul li {
		flex: 1;
		padding: 0 10px;
		text-align: center;
	}

	header .navigation-bar .fake-link {
		text-decoration: none;
		font-size: 13px;
		font-family: system-ui, -apple-system, sans-serif;
		letter-spacing: 0.15em;
		text-transform: uppercase;
		color: #2c2c2c;
	}

	header .text {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
	}

	header .text .hero-logo {
		max-width: 400px;
		margin-bottom: 10px;
	}

	header .text .hero-divider {
		display: flex;
		align-items: center;
		gap: 15px;
		margin-bottom: 15px;
	}

	header .text .hero-divider .line {
		display: block;
		width: 80px;
		height: 1px;
		background: rgba(255, 255, 255, 0.6);
	}

	header .text .hero-divider .fa-asterisk {
		font-size: 10px;
		color: #a96700;
	}

	header .text span {
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 15px;
		margin-bottom: 12px;
		letter-spacing: 0.15em;
		text-transform: uppercase;
		color: #fff;
	}

	header .text .button button {
		border: 1px solid #fff;
		padding: 10px 28px;
		letter-spacing: 0.15em;
		font-size: 12px;
		font-family: system-ui, -apple-system, sans-serif;
		text-transform: uppercase;
		background: transparent;
		color: #fff;
	}

	/* About Us */
	.about-us,
	.reservation {
		display: flex;
		padding: 60px;
		background: #f5ede3;
	}

	.about-us .text,
	.reservation .text {
		flex: 50%;
		padding: 40px 52px 0 0;
		text-align: center;
	}

	.about-us .text h3,
	.reservation .text h3 {
		color: #000;
	}

	.about-us .image-container,
	.reservation .image-container {
		display: flex;
		flex: 50%;
	}

	.about-us .image,
	.reservation .image {
		margin-left: 20px;
	}

	/* Recipes */
	.recipes {
		position: relative;
		background: #f5ede3;
	}

	.recipes .recipes-image-container {
		display: flex;
		gap: 20px;
	}

	.recipes .recipes-image-container .image {
		flex: 1;
		height: 350px;
		background-attachment: fixed;
		background-position: center;
		background-size: cover;
	}

	.recipes .text {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		text-align: center;
	}

	/* Menu */
	.menu {
		display: flex;
		padding: 60px;
		background: #f5ede3;
	}

	.menu .menu-image-container {
		display: flex;
		flex-wrap: wrap;
		flex: 60%;
	}

	.menu .menu-image-container .image {
		margin: 0 20px 20px 0;
		flex: calc(50% - 40px);
	}

	.menu .text {
		flex: 55%;
		padding: 40px 0 0 62px;
		text-align: center;
	}

	.menu .text h3 {
		color: #000;
	}

	/* Fixed Image */
	.fixed-image {
		background: url('/geisha_italia_booking_banner.jpg') fixed center;
		background-size: cover;
		height: 400px;
		position: relative;
	}

	.fixed-image .text {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		text-align: center;
	}

	/* Footer */
	footer {
		background-color: #f5ede3;
		padding: 60px 40px;
		text-align: center;
	}

	footer .footer-grid {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 40px;
		max-width: 1200px;
		margin: auto;
	}

	footer .footer-col h3 {
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 16px;
		letter-spacing: 1.5px;
		text-transform: uppercase;
		color: #2c2c2c;
		margin: 0 0 12px;
		font-weight: bold;
	}

	footer .footer-col hr {
		border: none;
		border-top: 2px solid #a96700;
		margin: 0 0 18px;
	}

	footer .footer-col p {
		color: #515151;
		font-size: 14px;
		line-height: 1.9;
		margin: 0;
	}

	footer .footer-col .fake-link {
		color: #515151;
		font-size: 14px;
		line-height: 2.2;
		text-decoration: none;
		display: block;
	}

	/* Copyright */
	.copyright {
		padding: 15px 40px 30px;
		background-color: #ece4d9;
		text-align: center;
	}

	.copyright ul {
		display: flex;
		justify-content: center;
		list-style: none;
		margin: 0;
		padding: 0;
	}

	.copyright li {
		color: #717171;
		font-size: 14px;
	}

	.copyright li:not(:last-of-type)::after {
		content: '\2022';
		margin: 10px;
	}

	/* Responsive */
	@media (max-width: 768px) {
		header nav {
			display: block;
			text-align: center;
			padding: 25px 36px 20px;
		}

		.about-us,
		.reservation,
		.menu {
			display: block;
			padding: 60px 20px;
		}

		footer .footer-grid {
			grid-template-columns: repeat(2, 1fr);
		}
	}

	@media (max-width: 576px) {
		.page-content h3 {
			font-size: 40px;
		}

		.page-content h2 {
			font-size: 90px;
		}

		footer .footer-grid {
			grid-template-columns: 1fr;
		}
	}
</style>
