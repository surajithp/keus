<script>
	import { onMount } from 'svelte';
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/ScrollTrigger';
	import CanvasAnimation from '../utils/CanvasAnimation.svelte';

	import '../app.css';

	let clientWidth;
	let canvas;
	let innerHeight;
	$: console.log(innerHeight);
	onMount(() => {
		gsap.registerPlugin(ScrollTrigger);
		ScrollTrigger.defaults({
			invalidateOnRefresh: true,
			markers: true
		});

		//main banner

		gsap.to('.hub-text', {
			scrollTrigger: {
				trigger: '.hub-intro',
				scrub: 1,
				start: 'top top',
				end: 'bottom 40%',
				ease: 'power2'
			},
			y: '-30%',
			opacity: 0
		});

		// product with text
		let pwt = gsap.timeline({
			scrollTrigger: {
				trigger: '.section-zero',
				start: 'top top',
				// end: `+=${122 * 50}`,

				pin: true,
				scrub: 1
			}
		});

		ScrollTrigger.matchMedia({
			// desktop text timeline
			'(min-width: 800px)': function () {
				// setup animations and ScrollTriggers for screens 800px wide or greater (desktop) here...
				// These ScrollTriggers will be reverted/killed when the media query doesn't match anymore.
				// Timeline for fading in and fading out the text
			},
			// mobile text timeline
			'(max-width: 799px)': function () {
				// The ScrollTriggers created inside these functions are segregated and get
				// reverted/killed when the media query doesn't match anymore.
			},
			all: function () {
				pwt
					.fromTo(
						'.section-zero__heading',
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: 25,
							xPercent: 10
						},
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: 20,
							xPercent: 10
						},
						1
					)
					.fromTo(
						'.section-zero__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -25
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							xPercent: 40,
							duration: 1
						}
					)
					.to(
						'.section-zero__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -25,
							duration: 1
						},
						'+=1'
					)
					.fromTo(
						'.section-zero__title3',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -25
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							xPercent: 40,
							duration: 1
						}
					)
					.to(
						'.section-zero__title3',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -25,
							duration: 1
						},
						'+=1'
					);
			}
		});

		// Text enter and leave screen
		gsap.set('.sentinel-never-sleeps-text, .text-1', {
			y: 50,
			opacity: 0,
			duration: 1,
			ease: 'power3.out',
			overwrite: 'auto'
		});

		ScrollTrigger.create({
			trigger: '.section-one',
			start: 'top 60%',
			end: 'top 10%',

			onEnter: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: 0,
					opacity: 1,
					stagger: 0.2
				}),
			onLeave: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: -50,
					opacity: 0,
					stagger: 0.2
				}),
			onEnterBack: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: 0,
					opacity: 1,
					stagger: -0.2
				}),
			onLeaveBack: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: 50,
					opacity: 0,
					stagger: -0.2
				})
		});

		// canvas scroll pause animation for hub

		let tl = gsap.timeline({
			scrollTrigger: {
				trigger: '.section-two',
				start: 'top top',
				end: `+=${122 * 20}`,

				pin: true,
				scrub: 1
			}
		});

		let scrollImages = (canvasId, texts, imageFolder) => {
			ScrollTrigger.matchMedia({
				// desktop text timeline
				'(min-width: 800px)': function () {
					// setup animations and ScrollTriggers for screens 800px wide or greater (desktop) here...
					// These ScrollTriggers will be reverted/killed when the media query doesn't match anymore.
					// Timeline for fading in and fading out the text
				},
				// mobile text timeline
				'(max-width: 799px)': function () {
					// The ScrollTriggers created inside these functions are segregated and get
					// reverted/killed when the media query doesn't match anymore.
				},
				all: function () {
					console.clear();

					// this is the bit that does the image scrolling
					const canvasIdWithoutHash = canvasId.substring(1);
					const canvas = document.getElementById(canvasIdWithoutHash);
					const context = canvas.getContext('2d');

					canvas.width = document.body.clientWidth;
					canvas.height = innerHeight;

					//const frameCount = 199;
					//const currentFrame = (index) =>
					//`${imageFolder}/${(index + 1).toString().padStart(4, "0")}.jpg`;

					const frameCount = 122;
					const currentFrame = (index) =>
						`/assets/hub/hub_${(index + 1).toString().padStart(3, '0')}.jpg`;

					const images = [];
					const products = {
						frame: 0
					};

					for (let i = 0; i < frameCount; i++) {
						const img = new Image();
						img.src = currentFrame(i);
						images.push(img);
					}

					tl.fromTo(
						'#product-info__description1',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut'
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut'
						}
					)
						.to(
							'#product-info__description1',
							{
								autoAlpha: 0,
								duration: 1,
								ease: 'power4.easeOut'
							},
							'+=2'
						)
						.to(products, {
							frame: 67,
							snap: 'frame',
							duration: 4,
							onUpdate: render
						})
						.fromTo(
							'#product-info__description2',
							{
								autoAlpha: 0,
								ease: 'power4.easeOut'
							},
							{
								autoAlpha: 1,
								ease: 'power4.easeOut'
							}
						)
						.to(
							'#product-info__description2',
							{
								autoAlpha: 0,
								duration: 1,
								ease: 'power4.easeOut'
							},
							'+=2'
						)
						.to(products, {
							frame: frameCount - 1,
							snap: 'frame',
							duration: 2,
							onUpdate: render
						});

					images[0].onload = render;

					function render() {
						context.clearRect(0, 0, canvas.width, canvas.height);
						context.drawImage(images[products.frame], 0, 0);
					}
				}
			});
		};
		scrollImages('#product-ezgif');
		ScrollTrigger.refresh();
	});
</script>

<svelte:window bind:innerHeight />
<div class="h-screen overflow-hidden flex items-center justify-center hub-intro">
	<img src="/assets/hub-intro.jpg" class="w-full h-full object-cover" alt="" />
	<div class="hub-text absolute">
		<p class=" text-2xl md:text-4xl text-center">Meet the sentinel</p>
		<p class=" text-center">A truly advanced gatekeeper for the smartest homes.</p>
	</div>
</div>

<section class="scene section section-one h-screen relative">
	<div class="viewer viewer-one relative overflow-hidden" />
	<div class="sentinel-never-sleeps-text absolute z-10 w-full mt-16 text-center">
		<p class="text-2xl md:text-4xl">The sentinel never sleeps</p>
	</div>
	<CanvasAnimation frameCount="95" assetUrl="/assets/bscene/bscene_" bind:innerHeight />
</section>

<div class="h-screen section-zero relative" style="background-color: #F9F8F6">
  <div class="section-zero__product-image absolute top-0 left-0 w-full h-full">
    <img src="/assets/hub.jpg" class="h-3/4 md:h-full md:pt-12 absolute bottom-8 right-[-20%] md:-right-8 w-[1000px] md:w-full object-contain max-w-[110%] md:max-w-[800px]" alt="" />
  </div>
  <div class="section-zero__heading">
    <p class=" text-xl md:text-3xl pt-32">The beating heart in keus homes</p>
    <p>To deliver a superlative smart home experience.</p>
  </div>
  <p class="section-zero__title2 absolute bottom-1/3">
    Stores and backs up everything that matters
  </p>
  <div class="section-zero__title3 absolute bottom-1/4">
    <p>Auto Updates</p>
    <p>An always up to date and secure system</p>
  </div>
</div>

<div class="h-screen relative bg-dark flex flex-col text-center md:text-left md:flex-row items-center justify-evenly">
  <p class="text-white text-4xl leading-snug w-full md:w-6/12 md:max-w-[400px]">
    Seamless communication <br/> with the Keus app
  </p>
  <img src="/assets/hub-keus-app.png" class="w-full object-contain max-w-[254px] md:max-w-[346px]" alt="" />
</div>

<div class="h-screen relative md:flex md:items-center" style="background-color: #F9F8F6">
  <div class="absolute bottom-[10%] md:bottom-auto left-0 md:left-auto right-0 md:right-auto md:relative md:w-1/2">
    <p class="absolute md:max-w-[410px] md:w-full md:relative text-center md:text-right bottom-[10%] md:bottom-auto text-2xl left-1/2 md:left-auto transform -translate-x-1/2 md:translate-auto w-10/12 md:mr-0 md:ml-auto">Connects Keus smarthome to the internet</p>
  </div>

  <div class="absolute md:relative top-0 left-0 w-full md:w-1/2 h-full">
    <img src="/assets/hub-back.png" class="h-full absolute w-full object-contain max-w-[110%] md:max-w-[850px] md:right-0" alt="" />
  </div>

</div>


<section class="h-screen relative section-two product-ezgif">
	<canvas id="product-ezgif" />
	<div class="product-content">
		<div class="product-title-wrapper">
			<div class="product-title" id="product-title1">
				<h1>Hub</h1>
			</div>
		</div>
		<div class="product-info">
			<div class="product-info__description text-white" id="product-info__description1">
				<h2 class="text-4xl">Mini but max</h2>
				<p class="mt-12">Superfast and seriously powerful.</p>
			</div>
			<div class="product-info__description" id="product-info__description2">
				<h2>Multi core performace powerful and fast</h2>
			</div>
			<div class="product-info__description" id="product-info__description3">
				<h2>Magic like you’ve never heard.</h2>
			</div>
		</div>
	</div>
</section>

<div class="h-screen bg-gray-800" />

<style>
	.section {
		height: 100%;
		background: black;
		color: white;
	}
	.viewer {
		max-width: 100%;
		max-height: 100%;
		margin-left: auto;
		margin-right: auto;
		background-repeat: no-repeat;
		background-position: 0 0;
	}

	.product-content {
		position: absolute;
		top: 0;
		width: 100%;
		height: 100%;
	}

	.product-title-wrapper {
		top: 40%;
		position: absolute;
		display: flex;
		align-items: center;
		justify-content: flex-start;
		flex-direction: column;
		right: 50px !important;
		flex-direction: column;
		width: 50vw;
		z-index: 2;
	}

	.product-title {
		width: 450px;
		opacity: 0;
		right: 0;
		transition: all 0.3s ease-in-out;
		position: relative;
	}

	.product-title h1 {
		position: absolute;
		top: 0;
		right: 0;
		padding-right: 15px;
		color: white;
		font-family: Arial, Helvetica, sans-serif;
		font-size: 118px;
		font-style: oblique;
		letter-spacing: 4.9px;
		line-height: 80px;
		text-transform: uppercase;
		text-align: right;
	}

	.product-info {
		top: 44%;
		position: absolute;
		display: flex;
		align-items: center;
		justify-content: flex-start;
		flex-direction: column;
		left: 50px !important;
		flex-direction: column;
		width: 50vw;
		z-index: 2;
	}

	.product-info div.product-info__description {
		width: 450px;
		opacity: 0;
		transition: all 0.3s ease-in-out;
		position: relative;
	}

	.spacer {
		height: 700px;
	}

	.product-info div.product-info__description h2 {
		position: absolute;
		top: 0;
		left: 0;
		color: #ffffff;
		font-family: Arial, Helvetica, sans-serif;
		font-size: 28px;
		letter-spacing: 0;
		line-height: 35px;
		padding-left: 15px;
		border-left: 3px solid #d2ff02;
	}
</style>
