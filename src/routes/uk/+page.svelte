<script lang="ts">
	import { onMount } from 'svelte';

	let noiseUrl = $state('');
	let body: HTMLElement;
	let nav: HTMLElement;
	let navToggle: HTMLElement;
	let navSpanMiddle: HTMLElement;
	let navNavigationBar: HTMLElement;
	let headerText: HTMLElement;
	let headerSection: HTMLElement;
	let aboutSection: HTMLElement;
	let recipeSection: HTMLElement;
	let menuSection: HTMLElement;
	let fixedImageSection: HTMLElement;
	let footerSection: HTMLElement;
	let dotOne: HTMLElement;
	let dotTwo: HTMLElement;
	let dotThree: HTMLElement;
	let dotsElements: HTMLElement[];
	let logoImage: HTMLImageElement;
	let svgDown: HTMLElement;
	let svgUp: HTMLElement;
	let menuImgs: NodeListOf<HTMLImageElement>;
	let boxModel: HTMLElement;
	let menuImageContainer: HTMLElement;
	let boxModelArrow: HTMLElement;
	let boxModelImage: HTMLImageElement;

	onMount(() => {
		// Generate noise texture
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

		body = document.body;
		nav = document.querySelector('.uk-page header nav')!;
		navToggle = document.querySelector('.uk-page header nav .toggle')!;
		navSpanMiddle = document.querySelector('.uk-page header nav .toggle .middle')!;
		navNavigationBar = document.querySelector('.uk-page header nav .navigation-bar')!;
		headerText = document.querySelector('.uk-page header .text')!;
		headerSection = document.querySelector('.uk-page header')!;
		aboutSection = document.querySelector('.uk-page .about-us')!;
		recipeSection = document.querySelector('.uk-page .recipes')!;
		menuSection = document.querySelector('.uk-page .menu')!;
		fixedImageSection = document.querySelector('.uk-page .fixed-image')!;
		footerSection = document.querySelector('.uk-page footer')!;
		dotOne = document.querySelector('.uk-page .dots .one')!;
		dotTwo = document.querySelector('.uk-page .dots .two')!;
		dotThree = document.querySelector('.uk-page .dots .three')!;
		dotsElements = Array.from(document.querySelectorAll('.uk-page .dots > div'));
		logoImage = document.querySelector('.uk-page header nav .logo img')! as HTMLImageElement;
		svgDown = document.querySelector('.uk-page header .arrow-down')!;
		svgUp = document.querySelector('.uk-page .copyright .arrow-up')!;
		menuImgs = document.querySelectorAll('.uk-page .menu .menu-image-container img');
		boxModel = document.querySelector('.uk-page .menu .box-model')!;
		menuImageContainer = document.querySelector('.uk-page .menu-image-container')!;
		boxModelArrow = document.querySelector('.uk-page .menu .box-model .arrow')!;
		boxModelImage = document.querySelector('.uk-page .menu .box-model img')! as HTMLImageElement;

		// Prevent links with # from navigating
		document.querySelectorAll('.uk-page a[href="#"]').forEach(link =>
			link.addEventListener('click', (e) => e.preventDefault())
		);

		// Toggle hamburger menu
		navToggle.addEventListener('click', () => {
			navToggle.classList.toggle('active');
			navSpanMiddle.classList.toggle('hide');
			navNavigationBar.classList.toggle('show');
		});

		// Active nav li
		document.querySelectorAll('.uk-page header nav .navigation-bar li').forEach(li =>
			li.addEventListener('click', () => {
				const arr = Array.from(li.parentElement!.children);
				arr.forEach(el => el.classList.remove('active'));
				li.classList.add('active');
			})
		);

		// Scroll to top
		svgUp.addEventListener('click', () => {
			window.scroll({ top: 0, behavior: 'smooth' });
		});

		// Scroll handler
		window.onscroll = function () {
			// Sticky nav & logo color change
			if (window.pageYOffset > headerSection.offsetHeight - 75) {
				nav.classList.add('active');
				logoImage.src = '/cropped-g_fusion_logo.png';
			} else {
				nav.classList.remove('active');
				logoImage.src = '/cropped-g_fusion_logo.png';
			}

			// Header text fade
			if (window.pageYOffset > 0) {
				headerText.style.opacity = String(-window.pageYOffset / 300 + 1);
			}

			// Dots color
			if (window.pageYOffset < headerSection.offsetHeight * 0.5) {
				dotsElements.forEach(dot => dot.classList.remove('black'));
				dotTwo.classList.remove('active');
				dotOne.classList.add('active');
			} else if (window.pageYOffset > headerSection.offsetHeight * 0.5 && window.pageYOffset < recipeSection.offsetTop * 0.72) {
				dotsElements.forEach(dot => dot.classList.add('black'));
			} else if (window.pageYOffset > recipeSection.offsetTop * 0.75 && window.pageYOffset < menuSection.offsetTop * 0.81) {
				dotsElements.forEach(dot => dot.classList.remove('black'));
				dotOne.classList.remove('active');
				dotThree.classList.remove('active');
				dotTwo.classList.add('active');
			} else if (window.pageYOffset > menuSection.offsetTop * 0.81 && window.pageYOffset < fixedImageSection.offsetTop * 0.86) {
				dotsElements.forEach(dot => dot.classList.add('black'));
				dotThree.classList.remove('active');
				dotTwo.classList.add('active');
			} else if (window.pageYOffset > fixedImageSection.offsetTop * 0.86 && window.pageYOffset < footerSection.offsetTop * 0.72) {
				dotsElements.forEach(dot => dot.classList.remove('black'));
				dotTwo.classList.remove('active');
				dotThree.classList.add('active');
			} else if (window.pageYOffset > footerSection.offsetTop * 0.72 && window.pageYOffset < footerSection.offsetTop * 0.901) {
				dotsElements.forEach(dot => dot.classList.add('black'));
			} else if (window.pageYOffset > footerSection.offsetTop * 0.901) {
				dotsElements.forEach(dot => dot.classList.remove('black'));
			}
		};

		// Arrow down scroll
		svgDown.addEventListener('click', () => {
			window.scroll({ top: aboutSection.offsetTop - 30, behavior: 'smooth' });
		});

		// Dots scroll
		dotsElements.forEach(dot =>
			dot.addEventListener('click', function (this: HTMLElement) {
				const target = document.querySelector(this.dataset.x!);
				if (target) {
					window.scrollTo({ top: (target as HTMLElement).offsetTop - 100, behavior: 'smooth' });
				}
			})
		);

		// Menu image lightbox
		menuImgs.forEach(img =>
			img.addEventListener('click', function (this: HTMLImageElement) {
				const arr = Array.from(this.parentElement!.parentElement!.children);
				arr.forEach(div => div.classList.remove('active'));
				this.parentElement!.classList.add('active');
				boxModel.classList.add('active');
				boxModelImage.src = this.src;
				boxModelImage.classList.add('active');
				body.classList.add('hide-scroll');
			})
		);

		function boxModelFun(e: Event) {
			const ev = e as KeyboardEvent & MouseEvent;
			const target = ev.target as HTMLElement;

			if (ev.code === 'Escape' || (target.tagName === 'DIV' && !target.classList.contains('arrow')) || target.classList.contains('close')) {
				boxModel.classList.remove('active');
				body.classList.remove('hide-scroll');
			}

			if (boxModel.classList.contains('active')) {
				if (ev.code === 'ArrowRight' || ev.code === 'ArrowLeft' || target.classList.contains('arrow-right') || target.classList.contains('arrow-left')) {
					const arr = Array.from(menuImageContainer.children);
					const active = arr.find(div => div.classList.contains('active'))!;

					if (target.classList.contains('arrow-right') || ev.code === 'ArrowRight') {
						if (active.nextElementSibling === null) {
							active.parentElement!.firstElementChild!.classList.add('active');
							boxModelImage.src = (active.parentElement!.firstElementChild!.firstElementChild as HTMLImageElement).src;
						} else {
							active.nextElementSibling.classList.add('active');
							boxModelImage.src = (active.nextElementSibling.firstElementChild as HTMLImageElement).src;
						}
					} else if (target.classList.contains('arrow-left') || ev.code === 'ArrowLeft') {
						if (active.previousElementSibling === null) {
							active.parentElement!.lastElementChild!.classList.add('active');
							boxModelImage.src = (active.parentElement!.lastElementChild!.lastElementChild as HTMLImageElement).src;
						} else {
							active.previousElementSibling.classList.add('active');
							boxModelImage.src = (active.previousElementSibling.firstElementChild as HTMLImageElement).src;
						}
					}
					active.classList.remove('active');
				}
			}
		}

		window.addEventListener('keydown', boxModelFun);
		window.addEventListener('click', boxModelFun);
		boxModelArrow.addEventListener('click', boxModelFun);

		return () => {
			window.onscroll = null;
			window.removeEventListener('keydown', boxModelFun);
			window.removeEventListener('click', boxModelFun);
		};
	});
</script>

<svelte:head>
	<link href="https://fonts.googleapis.com/css2?family=Cabin&family=Herr+Von+Muellerhoff&family=Source+Sans+Pro&display=swap" rel="stylesheet">
	<link href="https://fonts.cdnfonts.com/css/electroharmonix" rel="stylesheet">
	<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.0/css/all.css" integrity="sha384-lKuwvrZot6UHsBSfcMvOkWwlCMgc0TaWr+30HWe3a4ltaBwTZhyTEggF5tJv8tbt" crossorigin="anonymous">
</svelte:head>

<div class="uk-page">
	{#if noiseUrl}
		<div class="noise-overlay" style="background-image: url({noiseUrl});"></div>
	{/if}

	<!--Dots-->
	<div class="dots">
		<div class="active one" data-x="header"></div>
		<div class="two" data-x=".recipes"></div>
		<div class="three" data-x=".fixed-image"></div>
	</div>

	<!--Header-->
	<header>
		<nav>
			<div class="login-btn">
				<a href="/login">Login</a>
			</div>
			<div class="logo">
				<a href="/uk"><img src="/cropped-g_fusion_logo.png" alt="Geisha Logo"></a>
			</div>
			<div class="toggle">
				<span class="first"></span>
				<span class="middle"></span>
				<span class="last"></span>
			</div>
			<div class="navigation-bar">
				<ul>
					<li><a href="/uk/menus">Menus<span class="underline"></span></a></li>
					<li><a href="/uk/bookings">Bookings<span class="underline"></span></a></li>
					<li><a href="#">Takeaway<span class="underline"></span></a></li>
					<li><a href="#">Vouchers<span class="underline"></span></a></li>
					<li><a href="#">Contact<span class="underline"></span></a></li>
				</ul>
			</div>
		</nav>
		<div class="text">
			<img class="hero-logo" src="/cropped-g_fusion_logo.png" alt="Geisha Fusion">
			<div class="hero-divider">
				<span class="line"></span>
				<i class="fas fa-asterisk"></i>
				<span class="line"></span>
			</div>
			<span>United Kingdom</span>
			<div class="button"><button>Explore</button></div>
		</div>
		<svg class="svg-down" width="192" height="61" version="1.1" xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" viewBox="0 0 160.7 61.5" enable-background="new 0 0 160.7 61.5" xml:space="preserve"><path fill="currentColor" d="M80.3,61.5c0,0,22.1-2.7,43.1-5.4s41-5.4,36.6-5.4c-21.7,0-34.1-12.7-44.9-25.4S95.3,0,80.3,0c-15,0-24.1,12.7-34.9,25.4S22.3,50.8,0.6,50.8c-4.3,0-6.5,0,3.5,1.3S36.2,56.1,80.3,61.5z"></path></svg>
		<div class="arrow-down"></div>
	</header>

	<!--About Us-->
	<div class="about-us">
		<div class="text">
			<h2>Welcome to</h2>
			<h3>Geisha Fusion</h3>
			<div><i class="fas fa-asterisk"></i></div>
			<p>Embark on a delightful culinary journey at Geisha Fusion! Our family-friendly restaurant presents a vibrant array of dishes from across the globe.</p>
			<p>Our diverse menu blends classic favourites with modern twists, inspired by a variety of international cuisines. Whether you're in the mood for an American-style burger, mouthwatering curry, or crispy aromatic duck, our chefs are ready to serve up the best flavours to delight your palate.</p>
			<div><a class="a-CTA" href="#">About Us</a></div>
		</div>
		<div class="image-container">
			<div class="image image1">
				<img src="/unnamed (1).webp" alt="Food Photo">
			</div>
			<div class="image image2">
				<img src="/unnamed (2).webp" alt="Food Photo">
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
		<div class="box-model">
			<i class="fas fa-times fa-2x close"></i>
			<div class="arrow">
				<div class="arrow arrow-right"></div>
				<div class="arrow arrow-left"></div>
			</div>
			<div class="box-image-container">
				<div class="box-image">
					<img src="" alt="Food Photo">
				</div>
			</div>
		</div>
		<div class="menu-image-container">
			<div class="image active">
				<img src="/food-1.webp" alt="Food Photo">
			</div>
			<div class="image">
				<img src="/food-2.webp" alt="Food Photo">
			</div>
		</div>
		<div class="text">
			<h2>Explore Our</h2>
			<h3>Menu</h3>
			<div><i class="fas fa-asterisk"></i></div>
			<p>From American-style burgers to mouthwatering curries and crispy aromatic duck — our menu is packed with global flavours to suit every taste. View our full menu and find your next favourite dish.</p>
			<div><a class="a-CTA" href="/uk/menus">View The Full Menu</a></div>
		</div>
	</div>

	<!--Fixed Image-->
	<div class="fixed-image">
		<div class="text">
			<h2>A Little Bit of</h2>
			<h3>Everywhere</h3>
		</div>
	</div>

	<!--Reservation-->
	<div class="reservation">
		<div class="text">
			<h2>Book Your</h2>
			<h3>Table</h3>
			<div><i class="fas fa-asterisk"></i></div>
			<p>Good food, great company, and a warm, welcoming atmosphere. Join us for a memorable dining experience the whole family will love. Tables fill up fast — book yours now!</p>
			<div><a class="a-CTA" href="#">Make a Reservation</a></div>
		</div>
		<div class="image-container">
			<div class="image image1">
				<img src="/unnamed (1).webp" alt="Restaurant">
			</div>
			<div class="image image2">
				<img src="/unnamed (2).webp" alt="Restaurant">
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
				<p>Front Street, Benton</p>
				<p>Newcastle Upon Tyne</p>
				<p>NE12 8AE</p>
			</div>
			<div class="footer-col">
				<h3>Contact Us</h3>
				<hr>
				<p>0191 300 4641</p>
				<p>fusion@geisharestaurants.co.uk</p>
			</div>
			<div class="footer-col">
				<h3>Opening Times</h3>
				<hr>
				<p>Mon – 12pm - 10:00pm</p>
				<p>Tues – 12pm - 10:00pm</p>
				<p>Wed – 12pm - 10:00pm</p>
				<p>Thur – 12pm - 10:00pm</p>
				<p>Fri – 12pm - 11:00pm</p>
				<p>Sat – 12pm - 11:00pm</p>
				<p>Sun – 12pm - 7:30pm</p>
			</div>
			<div class="footer-col">
				<h3>Essentials</h3>
				<hr>
				<a href="#">Takeaway</a>
				<a href="#">Book A Table</a>
				<a href="#">Parties and Functions</a>
				<a href="#">Gift Voucher</a>
				<a href="#">Contact Us</a>
				<a href="#">Privacy & Cookie Policy</a>
			</div>
		</div>
	</footer>

	<!--Copyright-->
	<div class="copyright">
		<svg class="svg-up" width="192" height="61" version="1.1" xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" viewBox="0 0 160.7 61.5" enable-background="new 0 0 160.7 61.5" xml:space="preserve"><path fill="#ece4d9" d="M80.3,61.5c0,0,22.1-2.7,43.1-5.4s41-5.4,36.6-5.4c-21.7,0-34.1-12.7-44.9-25.4S95.3,0,80.3,0c-15,0-24.1,12.7-34.9,25.4S22.3,50.8,0.6,50.8c-4.3,0-6.5,0,3.5,1.3S36.2,56.1,80.3,61.5z"></path></svg>
		<i class="fas fa-angle-double-up arrow-up"></i>
		<ul class="info">
			<li>&copy; Geisha Fusion 2026</li>
			<li>United Kingdom</li>
		</ul>
		<ul class="CTA">
			<li><a href="#">PERMISSIONS AND COPYRIGHT</a></li>
			<li><a href="#">CONTACT THE TEAM</a></li>
		</ul>
	</div>

</div>

<style>
	/* Global */
	:global(body) {
		margin: 0;
	}
	.uk-page {
		padding: 12px;
		background: #f5ede3;
		position: relative;
	}

	.noise-overlay {
		position: absolute;
		top: 12px;
		left: 12px;
		right: 12px;
		bottom: 12px;
		z-index: 0;
		pointer-events: none;
		opacity: 0.18;
		background-repeat: repeat;
		background-size: 200px 200px;
		mix-blend-mode: multiply;
	}
	.uk-page :global(*) {
		box-sizing: border-box;
	}
.uk-page :global(a) {
		text-decoration: none;
	}
	.uk-page :global(ul) {
		margin: 0;
		padding: 0;
		list-style: none;
	}
	.uk-page :global(img) {
		max-width: 100%;
	}
	.uk-page :global(h2) {
		font-family: 'Electroharmonix', sans-serif;
		font-size: 100px;
		font-weight: normal;
		margin: 0 0 -55px;
	}
	.uk-page :global(h1),
	.uk-page :global(h3) {
		font-family: system-ui, -apple-system, sans-serif;
		font-size: 47px;
		font-weight: bold;
		letter-spacing: 9.4px;
		margin: 0 0 15px;
	}
	.uk-page :global(p) {
		font-family: system-ui, -apple-system, sans-serif;
		color: #515151;
		line-height: 27px;
	}
	.uk-page :global(.a-CTA) {
		border-bottom: 2px solid #a96700;
		padding-bottom: 4px;
		letter-spacing: 1.5px;
		font-size: 18px;
	}

	/* Mutual */
	.uk-page :global(p),
	.uk-page :global(.a-CTA),
	.uk-page :global(input),
	.uk-page :global(header .navigation-bar a),
	.uk-page :global(.copyright li) {
		font-family: system-ui, -apple-system, sans-serif;
	}
	.uk-page :global(.fa-asterisk),
	.uk-page :global(.a-CTA),
	.uk-page :global(h2),
	.uk-page :global(header .navigation-bar a:hover),
	.uk-page :global(header nav.active .navigation-bar a:hover),
	.uk-page :global(footer .social-media .links a:hover),
	.uk-page :global(.copyright .info li a),
	.uk-page :global(.copyright .CTA li a:hover) {
		color: #a96700;
	}
	.uk-page :global(header nav .navigation-bar .underline) {
		background-color: #a96700;
	}
	.uk-page :global(header .navigation-bar ul li),
	.uk-page :global(header .text),
	.uk-page :global(.about-us .text),
	.uk-page :global(.reservation .text),
	.uk-page :global(.menu .box-model),
	.uk-page :global(.menu .text),
	.uk-page :global(.fixed-image .text),
	.uk-page :global(footer),
	.uk-page :global(.copyright) {
		text-align: center;
	}
	.uk-page :global(header nav),
	.uk-page :global(header .navigation-bar ul),
	.uk-page :global(.about-us),
	.uk-page :global(.reservation),
	.uk-page :global(.about-us .image-container),
	.uk-page :global(.reservation .image-container),
	.uk-page :global(.menu),
	.uk-page :global(.menu .menu-image-container),
	.uk-page :global(footer .contact-container),
	.uk-page :global(.copyright ul) {
		display: flex;
	}
	.uk-page :global(header nav .toggle),
	.uk-page :global(header nav .toggle span),
	.uk-page :global(header .navigation-bar),
	.uk-page :global(header .navigation-bar ul),
	.uk-page :global(.menu .box-model .close),
	.uk-page :global(footer .social-media .links a),
	.uk-page :global(.copyright .CTA li a) {
		transition: .3s;
	}
	.uk-page :global(header),
	.uk-page :global(header nav .toggle span),
	.uk-page :global(header .navigation-bar a),
	.uk-page :global(header .text),
	.uk-page :global(header .text .arrow .left),
	.uk-page :global(header .text .arrow .right),
	.uk-page :global(.recipes),
	.uk-page :global(.menu .box-image-container),
	.uk-page :global(.fixed-image .text),
	.uk-page :global(.copyright),
	.uk-page :global(.copyright .arrow-up) {
		position: relative;
	}
	.uk-page :global(header nav .toggle),
	.uk-page :global(header .navigation-bar .underline),
	.uk-page :global(header .text .arrow .left:after),
	.uk-page :global(header .text .arrow .right:after),
	.uk-page :global(header .svg-down),
	.uk-page :global(header .arrow-down),
	.uk-page :global(.recipes .text),
	.uk-page :global(.menu .box-model .close),
	.uk-page :global(.menu .box-model .arrow .arrow-right),
	.uk-page :global(.menu .box-model .arrow .arrow-left),
	.uk-page :global(.menu .box-image-container .box-image),
	.uk-page :global(.copyright .svg-up) {
		position: absolute;
	}
	.uk-page :global(.recipes .text),
	.uk-page :global(.fixed-image .text),
	.uk-page :global(.menu .box-image-container),
	.uk-page :global(.menu .box-image-container .box-image) {
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}
	.uk-page :global(header nav),
	.uk-page :global(header .navigation-bar a:hover .underline),
	.uk-page :global(header .navigation-bar li.active .underline),
	.uk-page :global(.menu .box-model),
	.uk-page :global(.menu .box-image-container),
	.uk-page :global(.menu .box-image-container .box-image) {
		width: 100%;
	}
	.uk-page :global(button),
	.uk-page :global(.dots > div),
	.uk-page :global(header nav .toggle),
	.uk-page :global(header .arrow-down),
	.uk-page :global(.menu .box-model .close),
	.uk-page :global(.menu .box-model .arrow .arrow-right),
	.uk-page :global(.menu .box-model .arrow .arrow-left),
	.uk-page :global(.menu .menu-image-container .image img),
	.uk-page :global(footer .newsletter i),
	.uk-page :global(.copyright .arrow-up) {
		cursor: pointer;
	}
	.uk-page :global(.dots .active),
	.uk-page :global(header nav.active .toggle.active span),
	.uk-page :global(header nav .toggle span),
	.uk-page :global(header .navigation-bar .underline) {
		background-color: #2c2c2c;
	}
	.uk-page :global(header nav.active) {
		background-color: #f5ede3;
	}
	.uk-page :global(header .text .arrow .left),
	.uk-page :global(header .text .arrow .right) {
		background-color: #fff;
	}
	.uk-page :global(h1),
	.uk-page :global(h3),
	.uk-page :global(header .text span),
	.uk-page :global(.menu .box-model .close:hover) {
		color: #fff;
	}
	.uk-page :global(header .navigation-bar a) {
		color: #2c2c2c;
	}
	.uk-page :global(header nav),
	.uk-page :global(header .navigation-bar.show),
	.uk-page :global(header .navigation-bar a:hover .underline),
	.uk-page :global(header .navigation-bar li.active .underline),
	.uk-page :global(header .text .arrow .left:after),
	.uk-page :global(.menu .box-model),
	.uk-page :global(.copyright .arrow-up) {
		left: 0;
	}
	.uk-page :global(header .text .arrow .left:after),
	.uk-page :global(header .text .arrow .right:after),
	.uk-page :global(header .text span),
	.uk-page :global(footer .social-media .links a),
	.uk-page :global(footer .newsletter i),
	.uk-page :global(.copyright .arrow-up) {
		display: inline-block;
	}

	/* Dots */
	.uk-page :global(.dots) {
		position: fixed;
		top: 50%;
		right: 30px;
		transform: translate(0, -50%);
		z-index: 5;
	}
	.uk-page :global(.dots > div) {
		width: 12px;
		height: 12px;
		border-radius: 50%;
		z-index: 20;
		margin-bottom: 10px;
		border: 2px solid #fff;
	}
	.uk-page :global(.dots .black) {
		border-color: #000;
	}
	.uk-page :global(.dots .active.black) {
		background-color: #000;
	}

	/* Loader */
	.uk-page :global(.loader-wrap),
	.uk-page :global(.loader) {
		position: absolute;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;
		margin: auto;
	}
	.uk-page :global(.loader-wrap) {
		background-color: #a96700;
		z-index: 30;
	}
	.uk-page :global(.loader-wrap.remove) {
		display: none;
	}
	.uk-page :global(.loader) {
		margin: auto;
		height: 40px;
		width: 80px;
	}
	.uk-page :global(.loader .loader-item) {
		position: relative;
		float: left;
		height: 40px;
		width: 4px;
		margin: 0 2px;
		background-color: #fff;
	}
	.uk-page :global(.loader .loader-item:nth-child(1)) { animation: loader-item-1 2s linear infinite; }
	.uk-page :global(.loader .loader-item:nth-child(2)) { animation: loader-item-2 2s linear infinite; }
	.uk-page :global(.loader .loader-item:nth-child(3)) { animation: loader-item-3 2s linear infinite; }
	.uk-page :global(.loader .loader-item:nth-child(4)) { animation: loader-item-4 2s linear infinite; }
	.uk-page :global(.loader .loader-item:nth-child(5)) { animation: loader-item-5 2s linear infinite; }
	.uk-page :global(.loader .loader-item:nth-child(6)) { animation: loader-item-6 2s linear infinite; }
	.uk-page :global(.loader .loader-item:nth-child(7)) { animation: loader-item-7 2s linear infinite; }
	.uk-page :global(.loader .loader-item:nth-child(8)) { animation: loader-item-8 2s linear infinite; }
	.uk-page :global(.loader .loader-item:nth-child(9)) { animation: loader-item-9 2s linear infinite; }
	.uk-page :global(.loader .loader-item:nth-child(10)) { animation: loader-item-10 2s linear infinite; }
	.uk-page :global(.loader:after) {
		content: 'Loading...';
		font-size: 16px;
		font-family: Arial, serif;
		color: #fff;
		text-align: center;
		position: absolute;
		left: 0;
		right: 0;
		bottom: -32px;
		margin: auto;
	}

	@keyframes loader-item-1 { 1% { transform: scaleY(1); } 11% { transform: scaleY(1.4); } 21% { transform: scaleY(1); } 100% { transform: scaleY(1); } }
	@keyframes loader-item-2 { 7% { transform: scaleY(1); } 17% { transform: scaleY(1.4); } 27% { transform: scaleY(1); } 100% { transform: scaleY(1); } }
	@keyframes loader-item-3 { 13% { transform: scaleY(1); } 23% { transform: scaleY(1.4); } 33% { transform: scaleY(1); } 100% { transform: scaleY(1); } }
	@keyframes loader-item-4 { 19% { transform: scaleY(1); } 29% { transform: scaleY(1.4); } 39% { transform: scaleY(1); } 100% { transform: scaleY(1); } }
	@keyframes loader-item-5 { 25% { transform: scaleY(1); } 35% { transform: scaleY(1.4); } 45% { transform: scaleY(1); } 100% { transform: scaleY(1); } }
	@keyframes loader-item-6 { 31% { transform: scaleY(1); } 41% { transform: scaleY(1.4); } 51% { transform: scaleY(1); } 100% { transform: scaleY(1); } }
	@keyframes loader-item-7 { 37% { transform: scaleY(1); } 47% { transform: scaleY(1.4); } 57% { transform: scaleY(1); } 100% { transform: scaleY(1); } }
	@keyframes loader-item-8 { 43% { transform: scaleY(1); } 53% { transform: scaleY(1.4); } 63% { transform: scaleY(1); } 100% { transform: scaleY(1); } }
	@keyframes loader-item-9 { 49% { transform: scaleY(1); } 59% { transform: scaleY(1.4); } 69% { transform: scaleY(1); } 100% { transform: scaleY(1); } }
	@keyframes loader-item-10 { 55% { transform: scaleY(1); } 65% { transform: scaleY(1.4); } 75% { transform: scaleY(1); } 100% { transform: scaleY(1); } }

	/* Header */
	.uk-page :global(header) {
		height: calc(100vh - 22px);
		background: url('/geisha_italia_booking_banner.jpg') fixed bottom / cover;
		padding: 20px 70px;
	}
	.uk-page :global(header nav) {
		position: fixed;
		top: 0;
		padding: 36px 140px 20px 36px;
		z-index: 20;
		background-color: #f5ede3;
	}
	.uk-page :global(header nav .toggle) {
		display: none;
		top: 26px;
		left: 20px;
		background-color: transparent;
		border: none;
		padding: 0;
		z-index: 21;
	}
	.uk-page :global(header nav .toggle:focus) { outline: none; }
	.uk-page :global(header nav.active .toggle span) { background-color: #000; }
	.uk-page :global(header nav .toggle.active) { left: 32%; }
	.uk-page :global(header nav .toggle.active .first) { top: 16px; transform: rotate(-45deg); }
	.uk-page :global(header nav .toggle.active .last) { top: 0; transform: rotate(45deg); }
	.uk-page :global(header nav .toggle .hide) { transition: 0s; visibility: hidden; }
	.uk-page :global(header nav .toggle span) { display: block; width: 30px; height: 2px; border-radius: 40px; }
	.uk-page :global(header nav .toggle span:not(:last-of-type)) { margin-bottom: 6px; }
	.uk-page :global(header nav .login-btn) { position: absolute; right: 36px; top: 36px; z-index: 21; }
	.uk-page :global(header nav .login-btn a) { color: #2c2c2c; text-decoration: none; font-family: system-ui, -apple-system, sans-serif; font-size: 12px; letter-spacing: 0.15em; text-transform: uppercase; transition: all .3s; border: 1px solid #2c2c2c; padding: 8px 20px; display: inline-block; }
	.uk-page :global(header nav .login-btn a:hover) { background-color: #a96700; border-color: #a96700; color: #fff; }
	.uk-page :global(header nav.active .login-btn a) { color: #000; border-color: #000; }
	.uk-page :global(header nav.active .login-btn a:hover) { background-color: #a96700; border-color: #a96700; color: #fff; }
	.uk-page :global(header nav .logo) { flex-basis: 56%; padding-left: 50px; }
	.uk-page :global(header nav .logo img) { max-height: 50px; width: auto; filter: invert(1); }
	.uk-page :global(header .navigation-bar.show) { width: 40%; }
	.uk-page :global(header .navigation-bar.show a) { color: #ccc !important; }
	.uk-page :global(header .navigation-bar.show a:hover) { color: #fff !important; }
	.uk-page :global(header .navigation-bar ul li) { flex: 1; padding: 0 10px; }
	.uk-page :global(header .navigation-bar a) { text-decoration: none; transition: all .5s; font-size: 13px; font-family: system-ui, -apple-system, sans-serif; letter-spacing: 0.15em; text-transform: uppercase; white-space: nowrap; }
	.uk-page :global(header nav.active .navigation-bar a) { color: #000; }
	.uk-page :global(header .navigation-bar .underline) { height: 2px; width: 0; left: 50%; bottom: -4px; transition: all .3s; display: block; }
	.uk-page :global(header .text) {
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.uk-page :global(header .text .hero-logo) { max-width: 400px; margin-bottom: 10px; }
	.uk-page :global(header .text .hero-divider) {
		display: flex;
		align-items: center;
		gap: 15px;
		margin-bottom: 15px;
	}
	.uk-page :global(header .text .hero-divider .line) {
		display: block;
		width: 80px;
		height: 1px;
		background: rgba(255, 255, 255, 0.6);
	}
	.uk-page :global(header .text .hero-divider .fa-asterisk) {
		font-size: 10px;
		color: #a96700;
	}
	.uk-page :global(header .text .arrow .left),
	.uk-page :global(header .text .arrow .right) { height: 3px; width: 100px; }
	.uk-page :global(header .text .arrow .left) { left: -4%; }
	.uk-page :global(header .text .arrow .right) { right: -4%; }
	.uk-page :global(header .text .arrow .left:after),
	.uk-page :global(header .text .arrow .right:after) { content: ''; border: 10px solid transparent; border-left-color: #fff; top: -8px; }
	.uk-page :global(header .text .arrow .right:after) { right: 0; border-color: transparent #fff transparent transparent; }
	.uk-page :global(header .text .arrow .fa-asterisk) { vertical-align: super; }
	.uk-page :global(header .text span) { font-family: system-ui, -apple-system, sans-serif; font-size: 15px; margin-bottom: 12px; letter-spacing: 0.15em; text-transform: uppercase; }
	.uk-page :global(header .text .button button) {
		border: 1px solid #fff; padding: 10px 28px; letter-spacing: 0.15em; font-size: 12px;
		font-family: system-ui, -apple-system, sans-serif; text-transform: uppercase;
		background: transparent; color: #fff; cursor: pointer; transition: all 0.3s;
	}
	.uk-page :global(header .text .button button:hover) {
		background-color: #a96700; border-color: #a96700; color: #fff;
	}
	.uk-page :global(header .text .button button:focus) { outline: none; }
	.uk-page :global(header .svg-down) { bottom: 0; left: 50%; z-index: 5; margin-left: -96px; margin-bottom: -12px; color: #f5ede3; }
	.uk-page :global(header .arrow-down) {
		width: 70px; height: 50px; bottom: -10px; left: 50%; transform: translate(-50%, 0); z-index: 10;
		background: url('https://res.cloudinary.com/abdel-rahman-ali/image/upload/v1535988515/arrow-down.png') no-repeat center;
	}

	/* About Us */
	.uk-page :global(.about-us),
	.uk-page :global(.reservation) { padding: 60px; }
	.uk-page :global(.about-us .text),
	.uk-page :global(.reservation .text) { flex: 50%; padding: 40px 52px 0 0; }
	.uk-page :global(.about-us .text h3),
	.uk-page :global(.reservation .text h3) { color: #000; }
	.uk-page :global(.about-us .text .fa-asterisk),
	.uk-page :global(.reservation .text .fa-asterisk) { color: #9a9998; }
	.uk-page :global(.about-us .image-container),
	.uk-page :global(.reservation .image-container) { flex: 50%; }
	.uk-page :global(.about-us .image),
	.uk-page :global(.reservation .image) { margin-left: 20px; }

	/* Recipes */
	.uk-page :global(.recipes) {
		position: relative;
	}
	.uk-page :global(.recipes .recipes-image-container) {
		display: flex;
		gap: 20px;
	}
	.uk-page :global(.recipes .recipes-image-container .image) {
		flex: 1;
		height: 350px;
		background-attachment: fixed;
		background-position: center;
		background-size: cover;
	}

	/* Menu */
	.uk-page :global(.menu) { padding: 60px; }
	.uk-page :global(.menu .box-model) {
		display: none; position: fixed; height: 100%; top: 0; z-index: 20; background-color: rgba(0,0,0,.7);
	}
	.uk-page :global(.menu .box-model.active) { display: block; }
	.uk-page :global(.menu .box-model .close) { color: #ccc; right: 25px; top: 10px; z-index: 20; }
	.uk-page :global(.menu .box-model .arrow .arrow-right),
	.uk-page :global(.menu .box-model .arrow .arrow-left) {
		width: 40px; height: 40px; right: 20px; top: 50%;
		border-right: 2px solid #fff; border-top: 2px solid #fff; transform: rotate(45deg); z-index: 20;
	}
	.uk-page :global(.menu .box-model .arrow .arrow-left) { left: 20px; transform: rotate(-135deg); }
	.uk-page :global(.menu .box-image-container) { height: 100%; }
	.uk-page :global(.menu .box-image-container .box-image img.active) { animation: scale .5s; }
	@keyframes scale { from { transform: scale(0,0); } to { transform: scale(1,1); } }
	.uk-page :global(.menu .menu-image-container) { flex-wrap: wrap; flex: 60%; }
	.uk-page :global(.menu .menu-image-container .image) { margin: 0 20px 20px 0; flex: calc(50% - 40px); }
	.uk-page :global(.menu .text) { flex: 55%; padding: 40px 0 0 62px; }
	.uk-page :global(.menu .text h3) { color: #000; }
	.uk-page :global(.menu .text .fa-asterisk) { color: #9a9998; }

	/* Fixed Image */
	.uk-page :global(.fixed-image) {
		background: url('/geisha_italia_booking_banner.jpg') fixed center;
		background-size: cover;
		height: 400px;
	}

	/* Footer */
	.uk-page :global(footer) { background-color: #f5ede3; padding: 60px 40px; position: relative; z-index: 1; }
	.uk-page :global(footer .footer-grid) { display: grid; grid-template-columns: repeat(4, 1fr); gap: 40px; max-width: 1200px; margin: auto; }
	.uk-page :global(footer .footer-col h3) { font-family: system-ui, -apple-system, sans-serif; font-size: 16px; letter-spacing: 1.5px; text-transform: uppercase; color: #2c2c2c; margin: 0 0 12px; font-weight: bold; }
	.uk-page :global(footer .footer-col hr) { border: none; border-top: 2px solid #a96700; margin: 0 0 18px; }
	.uk-page :global(footer .footer-col p) { color: #515151; font-size: 14px; line-height: 1.9; margin: 0; }
	.uk-page :global(footer .footer-col a) { color: #515151; font-size: 14px; line-height: 2.2; text-decoration: none; display: block; transition: color 0.2s; }
	.uk-page :global(footer .footer-col a:hover) { color: #a96700; }

	/* Copyright */
	.uk-page :global(.copyright) { padding: 15px 40px 30px; background-color: #ece4d9; position: relative; z-index: 1; }
	.uk-page :global(.copyright .svg-up) { top: 0; left: 50%; margin-left: -96px; margin-top: -50px; }
	.uk-page :global(.copyright .arrow-up) { width: 40px; height: 30px; top: -45px; color: #2c2c2c; line-height: 1.9; }
	.uk-page :global(.copyright ul) { justify-content: center; }
	.uk-page :global(.copyright li) { color: #717171; font-size: 14px; }
	.uk-page :global(.copyright li:not(:last-of-type):after) { content: '\2022'; margin: 10px; }
	.uk-page :global(.copyright .CTA) { margin-top: 25px; }
	.uk-page :global(.copyright .CTA li a) { color: #717171; }

	/* Responsive */
	@media (max-width: 1200px) {
		.uk-page :global(.menu),
		.uk-page :global(.about-us),
		.uk-page :global(.reservation) { padding: 60px 40px; }
		.uk-page :global(.about-us .text),
		.uk-page :global(.reservation .text) { padding: 0 32px 0 0; }
		.uk-page :global(.about-us .image-container),
		.uk-page :global(.reservation .image-container) { align-items: center; }
		.uk-page :global(.menu .text) { padding: 0 0 0 32px; }
	}
	@media (max-width: 992px) {
		.uk-page { padding: 0; }
		.noise-overlay { top: 0; left: 0; right: 0; bottom: 0; }
		.uk-page :global(.dots) { display: none; }
		.uk-page :global(header) { height: calc(100vh - 10px); }
		.uk-page :global(header nav .logo) { padding-left: 0; }
		.uk-page :global(.about-us) { display: block; padding-top: 50px; }
		.uk-page :global(.about-us .text),
		.uk-page :global(.reservation .text) { padding: 0 0 40px; }
		.uk-page :global(.about-us .image-container .image1),
		.uk-page :global(.reservation .image-container .image1) { margin-left: 0; }
		.uk-page :global(.menu) { display: block; padding: 60px 20px 60px 40px; }
		.uk-page :global(.menu .text) { padding: 20px 20px 0 0; }
		.uk-page :global(.reservation) { display: block; padding: 30px 20px 60px; }
	}
	@media (max-width: 768px) {
		.uk-page :global(header nav) { display: block; text-align: center; left: 0; top: 0; width: 100%; padding: 25px 36px 20px; }
		.uk-page :global(header nav .toggle) { display: block; }
		.uk-page :global(header nav .login-btn) { right: 20px; top: 22px; }
		.uk-page :global(header .navigation-bar) {
			position: fixed; top: 0; left: -174px; height: 100%; background-color: #262526; overflow: hidden; z-index: 20; padding: 40px;
		}
		.uk-page :global(header .navigation-bar ul) { display: block; padding-top: 40px; }
		.uk-page :global(header .navigation-bar ul li) { text-align: left; padding: 14px 0; }
		.uk-page :global(footer .footer-grid) { grid-template-columns: repeat(2, 1fr); }
		.uk-page :global(.copyright ul) { display: block; margin-top: -20px; }
		.uk-page :global(.copyright li) { margin-bottom: 5px; }
		.uk-page :global(.copyright li:not(:last-of-type):after) { content: ''; }
	}
	@media (max-width: 576px) {
		.uk-page :global(h3),
		.uk-page :global(h1) { font-size: 40px; }
		.uk-page :global(h2) { font-size: 90px; }
		.uk-page :global(header) { padding: 0; }
		.uk-page :global(header nav .logo) { padding-left: 0; }
		.uk-page :global(header nav .toggle.active) { left: 36%; }
		.uk-page :global(header .navigation-bar.show) { padding: 20px; width: 48%; }
		.uk-page :global(.about-us),
		.uk-page :global(.reservation) { padding: 60px 20px; }
		.uk-page :global(.menu) { padding: 60px 0 60px 20px; }
		.uk-page :global(footer .footer-grid) { grid-template-columns: 1fr; }
		.uk-page :global(footer) { padding: 40px 20px; }
	}

</style>
