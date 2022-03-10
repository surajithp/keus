<script>
	import { onMount } from 'svelte';
  import {gsap}  from "gsap/dist/gsap";        
  import {ScrollTrigger} from "gsap/dist/ScrollTrigger"; 
	import CanvasAnimation from '../utils/CanvasAnimation.svelte';
	import { sectionOneAnimation } from '../utils/hub/SectionOne.svelte';

	import '../app.css';

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

		sectionOneAnimation();

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
			end: 'top -10%',

			onEnter: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: 0,
					opacity: 1,
				}),
			onLeave: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: -50,
					opacity: 0,
				}),
			onEnterBack: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: 0,
					opacity: 1,
				}),
			onLeaveBack: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: 50,
					opacity: 0,
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

		const context = canvas.getContext('2d');

		canvas.width = document.body.clientWidth;
		canvas.height = innerHeight;

		const currentFrame = (index) =>
			`/assets/bscene/bscene_${(index + 1).toString().padStart(3, '0')}.jpg`;

		const images = [];
		const frames = {
			frame: 0
		};

		for (let i = 0; i < 92; i++) {
			const img = new Image();
			img.src = currentFrame(i);
			images.push(img);
		}

		gsap.timeline()
    .to(frames, {
      frame: 0,
      snap: 'frame',
      duration: 4,
      onUpdate: render
    })
    .to(frames, {
			frame: 92 - 1,
			snap: 'frame',
			ease: 'none',
			scrollTrigger: {
				trigger: '.section-one',
				start: 'top top',
				pin: true,
				scrub: 0.5
			},
			onUpdate: render // use animation onUpdate instead of scrollTrigger's onUpdate
		});

		images[0].onload = render;

		function render() {
			var imgWidth = (canvas.height * images[frames.frame].width) / images[frames.frame].height;
			console.log(imgWidth);
			context.clearRect(0, 0, canvas.width, canvas.height);
			context.drawImage(images[frames.frame], -400, 0, imgWidth, canvas.height);
		}

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
<div class="h-screen overflow-hidden flex md:items-center justify-center hub-intro">
	<div class="w-full h-full">
		<img src="/assets/hub-intro.jpg" class="w-[120%] h-full hub-intro-image object-cover" alt="" />
	</div>
	<div class="hub-text absolute text-back max-w-[296px] md:max-w-full mx-auto mt-[20vh] md:mt-0">
		<p
			class="title-font text-42 md:text-75 leading-tight md:leading-120 text-center w-10/12 md:w-full mx-auto"
		>
			Meet the sentinel
		</p>
		<p class=" text-20 leading-8 mt-4 text-center w-10/12 md:w-full mx-auto">
			A truly advanced gatekeeper for the smartest homes.
		</p>
	</div>
</div>

<div class="h-screen section-zero relative" style="background-color: #F9F8F6">
	<div class="section-zero__product-image absolute top-0 left-0 w-full h-full">
		<img
			src="/assets/hub.png"
			class="h-3/4 md:h-full md:pt-12 absolute -bottom-8 md:bottom-8 right-[-33vw] md:right-[-20vw] xl:right-[-10vw] md:w-full object-contain max-w-[165%] md:max-w-[1120px]"
			alt=""
		/>

	</div>
	<div
		class="section-zero__heading text-back text-center md:text-left md:relative md:left-[10vw] max-w-[296px] mx-auto md:ml-0 mr-auto md:max-w-[427px]"
	>
		<p class="title-font text-36 md:text-42 md:leading-snug leading-tight pt-32 mx-auto">
			A beating heart in Keus homes
		</p>
		<p class="text-24 leading-8 mt-6 mx-auto">To deliver a superlative smart home experience.</p>
	</div>
	<p
		class="section-zero__title2 title-font text-26 leading-9 max-w-[250px] mx-auto md:max-w-none text-center md:text-right absolute bottom-[10%] md:bottom-[42%] w-full md:w-2/6 left-0 md:left-auto right-0 md:right-[75vw] lg:right-[65vw]"
	>
		Stores and backs up <br/> everything that matters
	</p>
	<div class="section-zero__title3 absolute max-w-[250px] mx-auto md:max-w-none bottom-[15%] md:bottom-[31%] text-26 leading-9 text-center md:text-right w-full md:w-2/6 left-0 md:left-auto right-0 md:right-[75vw] lg:right-[65vw]">
		<p class="title-font text-26 leading-9">Auto Updates - OTA </p>
		<p class="text-16 leading-6">An always up to date and secure system</p>
	</div>
</div>

<section class="scene section section-one h-screen relative">
	<div class="viewer viewer-one relative overflow-hidden" />
	<div class="sentinel-never-sleeps-text absolute z-10 w-full mt-16 text-center">
		<p class="title-font text-36 leading-tight">The sentinel never sleeps</p>
	</div>
	<!-- <CanvasAnimation
		frameCount="95"
		assetUrl="/assets/bscene/bscene_"
		bind:innerHeight
		parentElement={'.section-one'}
	/> -->
	<canvas class="canvas" bind:this={canvas} />
</section>

<div
	class="h-screen relative bg-dark flex flex-col text-center md:text-left md:flex-row items-center justify-evenly"
>
	<p class="text-white text-4xl leading-snug w-full md:w-6/12 md:max-w-[400px]">
		Seamless communication <br /> with the Keus app
	</p>
	<img
		src="/assets/hub-keus-app.png"
		class="w-full object-contain max-w-[254px] md:max-w-[346px]"
		alt=""
	/>
</div>

<div class="h-screen text-32 relative md:flex md:items-center" style="background-color: #F9F8F6">
	<div
		class="absolute bottom-[10%] md:bottom-auto left-0 md:left-auto right-0 md:right-auto md:relative md:w-1/2"
	>
		<p
			class="absolute md:max-w-[410px] md:w-full md:relative text-center md:text-right bottom-[10%] md:bottom-auto text-2xl left-1/2 md:left-auto transform -translate-x-1/2 md:translate-auto w-10/12 md:mr-0 md:ml-auto"
		>
			Connects Keus smarthome to the internet
		</p>
	</div>

	<div class="absolute md:relative top-0 left-0 w-full md:w-1/2 h-full">
		<img
			src="/assets/hub-back.png"
			class="h-full absolute w-full object-contain max-w-[110%] md:max-w-[850px] md:right-0"
			alt=""
		/>
	</div>
</div>

<div class="h-screen bg-dark relative w-full">
	<div class="h-full w-full relative text-white">
		<p
			class="text-4xl absolute w-full text-center md:text-left left-1/2 md:left-[20%] transform -translate-x-1/2 top-[25%] max-w-[256px] mx-auto"
		>
			Secure like Fort knox
		</p>
		<p
			class="text-2xl absolute w-full text-center md:text-right left-1/2 md:left-[20%] md:top-1/2 md:bottom-auto transform -translate-x-1/2 md:-translate-y-1/2 bottom-[30%] max-w-[319px] mx-auto"
		>
			Proprietary scurity layers to further enchance layers of military grade encryption
		</p>
	</div>
	<div
		class="absolute left-1/2 md:left-auto top-1/2 transform -translate-x-1/2 md:-translate-x-[0] md:right-[5%] -translate-y-1/2 w-full md:max-w-[60%]"
	>
		<img src="/assets/hub-chip.png" class="w-full" alt="" />
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

<div class="h-screen w-full relative bg-[#D0CDC8]">
	<div class="h-full w-full absolute top-0 bottom-0 right-0 left-0">
		<p
			class="text-2xl absolute w-full text-center md:text-right left-1/2 md:left-[20%] md:top-1/2 md:bottom-auto transform -translate-x-1/2 md:-translate-y-1/2 bottom-[15%] max-w-[319px] mx-auto"
		>
			Wall or tabletop You decide
		</p>
	</div>
	<img
		src="/assets/hub-wall-and-desk-mount.jpg"
		class="h-full w-full md:w-8/12 object-cover md:mr-0 md:ml-auto"
		alt=""
	/>
</div>
