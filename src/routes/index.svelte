<script>
	import { onMount } from 'svelte';
	// import { gsap } from 'gsap';
	// import { ScrollTrigger } from 'gsap/ScrollTrigger.js';
	// import Scrollbar from 'smooth-scrollbar';
	import CanvasAnimation from '../utils/CanvasAnimation.svelte';
	import { sectionOneAnimation } from '../utils/hub/SectionOne.svelte';
	import { sectionThreeAnimation } from '../utils/hub/SectionThree.svelte';
	import { sectionFourAnimation } from '../utils/hub/SectionFour.svelte';
	import { sectionFiveAnimation } from '../utils/hub/SectionFive.svelte';
	import { sectionSevenAnimation } from '../utils/hub/SectionSeven.svelte';
	import { sectionEightAnimation } from '../utils/hub/SectionEight.svelte';

	import '../app.css';

	let canvas;
	let innerHeight;

	onMount(() => {
		innerHeight = window.screen.height;
		gsap.registerPlugin(ScrollTrigger);

		// Setup
		const scroller = document.querySelector('#content');

		const bodyScrollBar = Scrollbar.init(scroller, {
			damping: 0.1,
			delegateTo: document,
			alwaysShowTracks: true
		});

		ScrollTrigger.scrollerProxy('#content', {
			scrollTop(value) {
				if (arguments.length) {
					bodyScrollBar.scrollTop = value;
				}
				return bodyScrollBar.scrollTop;
			}
		});

		bodyScrollBar.addListener(ScrollTrigger.update);

		ScrollTrigger.defaults({
			invalidateOnRefresh: true,
			markers: false,
			scroller: scroller
		});

		// Only necessary to correct marker position - not needed in production
		if (document.querySelector('.gsap-marker-scroller-start')) {
			const markers = gsap.utils.toArray('[class *= "gsap-marker"]');

			bodyScrollBar.addListener(({ offset }) => {
				gsap.set(markers, { marginTop: -offset.y });
			});
		}

		// 		function resize() {
		//   offset_value = ($('.viewer').height() * 598)/(338); //get desired width of sprite according to defined height
		//   centering = offset_value/4;

		//   $('.viewer').css('max-width', offset_value);
		//   $('.viewer2').css('max-width', offset_value);
		// }

		// ScrollTrigger.addEventListener("refreshInit", resize);
		// resize();

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
			trigger: '.section-two',
			start: 'top 60%',
			end: 'top -10%',

			onEnter: () =>
				gsap.fromTo(
					'.sentinel-never-sleeps-text',
					{
						y: 200,
						opacity: 0
					},
					{
						y: 130,
						opacity: 1
					}
				),
			onLeave: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: 110,
					opacity: 0
				}),
			onEnterBack: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: 130,
					opacity: 1
				}),
			onLeaveBack: () =>
				gsap.to('.sentinel-never-sleeps-text', {
					y: 200,
					opacity: 0
				})
		});

		// canvas scroll pause animation for hub

		let tl = gsap.timeline({
			scrollTrigger: {
				trigger: '.section-two',
				start: 'top',
				// end: `+=${122 * 20}`,

				pin: true,
				scrub: 1
			}
		});

		let sectionTwoAnimation = () => {
			ScrollTrigger.matchMedia({
				all: function () {
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
						// let paths = [];
						// for (const size of sizes) {
						// 	paths.push(`/cdn-cgi/image/width=${size},quality=75${currentFrame(i)} ${size}w`);
						// }
						const img = new Image();
						img.src = currentFrame(i);
						img.srcset = imgSrcSet(currentFrame(i));
						images.push(img);
					}

					tl.to(
						frames,
						{
							frame: 0,
							snap: 'frame',
							duration: 4,
							onUpdate: render
						},
						'+=2'
					).to(frames, {
						frame: 92 - 1,
						snap: 'frame',
						ease: 'none',
						scrollTrigger: {
							trigger: '.section-two',
							start: 'top top',
							pin: true,
							scrub: 0.5
						},
						onUpdate: render // use animation onUpdate instead of scrollTrigger's onUpdate
					});

					images[0].onload = render;

					function render() {
						var imgWidth =
							(canvas.height * images[frames.frame].width) / images[frames.frame].height;
						context.clearRect(0, 0, canvas.width, canvas.height);
						if (document.body.clientWidth <= 768) {
							context.drawImage(images[frames.frame], -430, 0, imgWidth, canvas.height);
						} else {
							context.drawImage(images[frames.frame], 0, 0, imgWidth, canvas.height);
						}
					}
				}
			});
		};
		sectionTwoAnimation();
		sectionThreeAnimation();
		sectionFourAnimation();
		sectionFiveAnimation();

		let sixTimeline = gsap.timeline({
			scrollTrigger: {
				trigger: '.section-six',
				start: 'top top',
				end: `+=${122 * 30}`,

				pin: true,
				scrub: 1
			}
		});

		let sectionSixAnimation = (canvasId, texts, imageFolder) => {
			ScrollTrigger.matchMedia({
				// desktop text timeline
				'(min-width: 768px)': function () {
					// setup animations and ScrollTriggers for screens 800px wide or greater (desktop) here...
					// These ScrollTriggers will be reverted/killed when the media query doesn't match anymore.
					// Timeline for fading in and fading out the text
					// this is the bit that does the image scrolling
					const canvasIdWithoutHash = canvasId.substring(1);
					const hubCanvas = document.getElementById(canvasIdWithoutHash);
					const context = hubCanvas.getContext('2d');

					hubCanvas.width = document.body.clientWidth;
					hubCanvas.height = innerHeight;
					let currentFrame;

					const frameCount = 122;

					currentFrame = (index) =>
						`/assets/hub/hub_${(index + 1).toString().padStart(3, '0')}.jpg`;

					const images = [];
					const products = {
						frame: 0
					};

					for (let i = 0; i < frameCount; i++) {
						// let paths = [];
						// for (const size of sizes) {
						// 	paths.push(`/cdn-cgi/image/width=${size},quality=75${currentFrame(i)} ${size}w`);
						// }
						const img = new Image();
						img.src = currentFrame(i);
						img.srcset = imgSrcSet(currentFrame(i));
						images.push(img);
					}

					sixTimeline
						.to(
							'#product-info__description1',
							{
								autoAlpha: 0,
								duration: 1,
								ease: 'power4.easeOut'
							},
							'+=1'
						)
						.to(products, {
							frame: 22,
							snap: 'frame',
							duration: 4,
							onUpdate: render
						})
						.to('#product-info__description3', {}, '+=2')
						.to(products, {
							frame: 43,
							snap: 'frame',
							duration: 4,
							onUpdate: render
						})
						.to('#product-info__description2', {}, '+=2')
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
								ease: 'power4.easeOut',
								duration: 2
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
							duration: 8,
							onUpdate: render
						})
						.fromTo(
							'#product-info__description3',
							{
								autoAlpha: 0,
								ease: 'power4.easeOut',
								y: 25
							},
							{
								autoAlpha: 1,
								ease: 'power4.easeOut',
								y: 0
							},
							'+=1'
						)
						.fromTo(
							'#product-info__description3',
							{
								autoAlpha: 1,
								ease: 'power4.easeOut',
								y: 0
							},
							{
								autoAlpha: 1,
								ease: 'power4.easeOut',
								y: 0
							},
							'+=4'
						);

					images[0].onload = render;

					function render() {
						let imgWidth =
							(hubCanvas.height * images[products.frame].width) / images[products.frame].height;
						context.clearRect(0, 0, hubCanvas.width, hubCanvas.height);
						context.drawImage(images[products.frame], 0, 0, imgWidth, hubCanvas.height);
					}
				},
				// mobile text timeline
				'(max-width: 767px)': function () {
					// The ScrollTriggers created inside these functions are segregated and get
					// reverted/killed when the media query doesn't match anymore.
					// this is the bit that does the image scrolling
					const canvasIdWithoutHash = canvasId.substring(1);
					const hubCanvas = document.getElementById(canvasIdWithoutHash);
					const context = hubCanvas.getContext('2d');

					hubCanvas.width = document.body.clientWidth;
					hubCanvas.height = innerHeight;
					let currentFrame;

					const frameCount = 122;
					currentFrame = (index) =>
						`/assets/hub_mobile/hub_${(index + 1).toString().padStart(3, '0')}.jpg`;

					const images = [];
					const products = {
						frame: 0
					};

					for (let i = 0; i < frameCount; i++) {
						// let paths = [];
						// for (const size of sizes) {
						// 	paths.push(`/cdn-cgi/image/width=${size},quality=75${currentFrame(i)} ${size}w`);
						// }
						const img = new Image();
						img.src = currentFrame(i);
						img.srcset = imgSrcSet(currentFrame(i));
						images.push(img);
					}

					sixTimeline
						.fromTo(
							'#product-info__description1',
							{
								autoAlpha: 0,
								ease: 'power4.easeOut',
								y: 0
							},
							{
								autoAlpha: 1,
								ease: 'power4.easeOut',
								y: -25
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
							frame: 22,
							snap: 'frame',
							duration: 4,
							onUpdate: render
						})
						.to('#product-info__description3', {}, '+=2')
						.to(products, {
							frame: 43,
							snap: 'frame',
							duration: 4,
							onUpdate: render
						})
						.to('#product-info__description2', {}, '+=2')
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
							duration: 8,
							onUpdate: render
						})
						.fromTo(
							'#product-info__description3',
							{
								autoAlpha: 0,
								ease: 'power4.easeOut',
								y: 100
							},
							{
								autoAlpha: 1,
								ease: 'power4.easeOut',
								y: 0
							}
						)
						.to('#product-info__description3', {}, '+=4');

					images[0].onload = render;

					function render() {
						var imgHeight =
							(hubCanvas.width * images[products.frame].height) / images[products.frame].width;
						context.clearRect(0, 0, hubCanvas.width, hubCanvas.height);
						context.drawImage(
							images[products.frame],
							0,
							(hubCanvas.height - imgHeight) / 2,
							hubCanvas.width,
							imgHeight
						);
					}
				},
				all: function () {
					// console.clear();
				}
			});
		};

		sectionSixAnimation('#product-ezgif');

		sectionSevenAnimation();
		sectionEightAnimation();

		ScrollTrigger.refresh();

		window.addEventListener('resize', () => {
			innerHeight = window.screen.height;
		});
	});

	const imgSrcSet = (imagePath) => {
		let srcset = '';
		let sizes = [400, 600, 900, 1200, 1600, 2000];
		let paths = [];

		for (const size of sizes) {
			paths.push(`/cdn-cgi/image/width=${size},quality=75,format=auto${imagePath} ${size}w`);
		}
		if (import.meta.env.VITE_ENV === 'production') {
			srcset = `${paths.join(', ')}`;
		}
		return srcset;
	};
</script>

<div class="h-screen overflow-hidden flex md:items-center justify-center hub-intro">
	<div class="w-full h-full">
		<img
			src="/assets/hub-intro.jpg"
			srcset={imgSrcSet('/assets/hub-intro.jpg')}
			class="w-[120%] h-full hub-intro-image object-cover"
			alt=""
		/>
	</div>
	<div
		class="hub-text absolute text-back max-w-[296px] md:max-w-full mx-auto mt-[17vh] md:mt-0 md:top-[34vh]"
	>
		<p
			class="title-font text-42 md:text-72 leading-tight md:leading-120 text-center w-10/12 md:w-full mx-auto"
		>
			Meet the sentinel
		</p>
		<p
			class=" text-20 md:text-20 md:leading-7 leading-8 mt-4 text-center w-10/12 md:w-full mx-auto"
		>
			A truly advanced gatekeeper for the smartest homes.
		</p>
	</div>
</div>

<div class=" section-one overflow-hidden" style="background-color: #F9F8F6">
	<div class="max-w-screen-xl w-full h-screen relative mx-auto md:flex md:items-center">
		<div class="flex-auto md:w-3/5 flex flex-col content-between h-full md:h-[640px] md:ml-[10%]">
			<div
				class="section-one__heading flex-auto text-back text-center md:text-left md:relative max-w-[296px] mx-auto md:ml-0 md:max-w-[427px]"
			>
				<p
					class="title-font text-[5.2vh] md:text-[4.2vh] md:leading-snug leading-tight pt-12 mx-auto"
				>
					A beating heart in Keus homes
				</p>
				<p class="text-[2.6vh] md:leading-8 mt-6 mx-auto max-w-[272px] md:max-w-[none]  ">
					To deliver a superlative smart home experience.
				</p>
			</div>
			<div class="text-center">
				<p
					class="section-one__title2 absolute bottom-0  w-full md:relative md:bottom-auto title-font md:text-[2.6vh] text-[4.2vh] leading-9  md:max-w-none text-center md:text-right mb-16"
				>
					Stores and backs up <br /> everything that matters
				</p>
				<div
					class="section-one__title3 absolute bottom-0 w-full  md:relative md:bottom-auto   md:max-w-none  leading-9 text-center md:text-right mb-16"
				>
					<p class="title-font md:text-[2.6vh] text-[4.2vh] leading-9">Auto Updates - OTA</p>
					<p class="md:text-[1.6vh] text-[2.6vh] leading-6 font-normal">
						An always up to date and secure system
					</p>
				</div>
			</div>
		</div>
		<div
			class="section-one__product-image flex-auto w-full absolute h-full flex items-center top-0 left-0 md:relative md:top-auto md:left-auto md:h-auto -z-10"
		>
			<img
				src="/assets/hub.jpg"
				srcset="/assets/hub.jpg"
				class=" md:h-auto min-w-full md:min-w-0 md:ml-[10%] absolute md:relative bottom-8 md:bottom-auto right-[-33vw] md:right-auto object-contain max-w-[135%] md:max-w-auto"
				alt=""
			/>
		</div>
	</div>
</div>

<section class="scene section section-two h-screen relative text-white">
	<div class="sentinel-never-sleeps-text absolute z-10 w-full text-center md:inset-y-1/3">
		<p class="title-font text-[5.2vh] md:text-[4.2vh] leading-tight">
			The sentinel <span class="block md:inline">never sleeps</span>
		</p>
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
	class="section-three h-screen relative bg-dark md:flex text-center md:text-left  md:items-center md:justify-evenly flex flex-col md:flex-row items-center justify-center"
>
	<div class="max-w-screen-xl md:mx-24 w-full h-screen relative mx-auto md:flex md:items-center">
		<p
			class="section-three__heading title-font text-white text-[4.2vh] leading-tight w-full md:flex md:justify-end md:mr-12 flex-auto"
		>
			Seamless communication <br /> with the Keus app
		</p>
		<div class="section-three__product-image mt-[8vh] md:mr-24 xl:mr-48 flex-auto">
			<img
				src="/assets/hub-keus-app.png"
				srcset={imgSrcSet('/assets/hub-keus-app.png')}
				class=" w-full md:w-auto object-contain max-w-[254px] md:max-w-[346px] mx-auto"
				alt=""
			/>
		</div>
	</div>
</div>

<div
	class="section-four h-screen relative md:flex text-center md:text-left  md:items-center md:justify-evenly flex flex-col md:flex-row items-center justify-center bg-pale-white"
>
	<div
		class="max-w-screen-xl md:mx-24 w-full h-screen relative mx-auto md:flex md:flex-row-reverse md:items-center"
	>
		<div
			class="section-four__product-image mt-[8vh]  md:mt-0 overflow-hidden md:absolute md:right-0"
		>
			<img
				src="/assets/hub-back.png"
				srcset={imgSrcSet('/assets/hub-back.png')}
				class=" w-full object-contain md:max-w-lg xl:max-w-4xl md:mx-auto"
				alt=""
			/>
		</div>
		<p
			class="section-four__heading title-font text-[4.2vh] mx-4 md:mx-0 leading-tight md:text-right w-128 md:absolute md:right-2/4"
		>
			Connects Keus smart <br />home to the internet
		</p>
	</div>
</div>

<div class="h-screen section-five relative bg-dark overflow-x-hidden text-white ">
	<div class="max-w-screen-xl md:mx-24 w-full h-screen relative mx-auto md:flex md:items-center">
		<div
			class="section-five__heading flex-auto text-center md:text-left  max-w-[296px] mx-auto w-[320px] md:mx-0 md:max-w-[427px] md:-mt-40"
		>
			<p
				class="title-font text-[6vh] md:text-[4.2vh] md:leading-snug leading-tight relative top-12 md:top-auto"
			>
				Secure like <br /> Fort Knox
			</p>
		</div>
		<div
			class="section-five__product-image w-full flex-auto overflow-hidden pt-12 md:w-3/5 md:max-w-[800px] "
		>
			<img src="/assets/hub-chip.png" srcset={imgSrcSet('/assets/hub-chip.png')} alt="" />
		</div>
		<p
			class="section-five__title2 title-font text-[4.2vh] md:text-[2.6vh] leading-9 px-12 md:px-0 text-center md:text-right md:absolute md:left-0 md:top-1/2 md:max-w-[320px]"
		>
			Proprietary security layers to further enchance layers of military grade encryption
		</p>
	</div>
</div>

<section class="h-screen relative section-six product-ezgif overflow-hidden bg-dark text-white">
	<canvas id="product-ezgif" />
	<div
		class="product-info__description  text-center md:text-left w-full md:w-auto absolute top-[10vh] md:top-[20vh] md:left-[20vh]"
		id="product-info__description1"
	>
		<h2 class="text-4xl md:text-42 title-font">Mini but max</h2>
		<p class="mt-4 md:text-24">Superfast and seriously powerful.</p>
	</div>
	<div
		class="product-info__description absolute inset-y-3/4 md:inset-y-1/2 md:mt-16 md:left-[40vh] w-full md:w-auto text-center md:text-right text-[4.2vh] title-font mt-8"
		id="product-info__description2"
	>
		<h2>Multi core performace. <br />Powerful and fast.</h2>
	</div>
	<div
		class="product-info__description  text-center md:text-right w-full md:w-auto absolute bottom-[10vh] md:inset-y-1/2 md:left-[40vh] md:opacity-0"
		id="product-info__description3"
	>
		<h2 class="md:text-[2.6vh] text-[4.2vh] title-font">100+ devices per hub - easy!</h2>
		<p class="md:text-[1.6vh] text-[2.6vh] mt-4 md:mt-0">
			Large Homes or Larger, We’ve got yourcovered
		</p>
	</div>
</section>

<div class="section-seven h-screen w-full relative overflow-hidden md:flex md:items-center">
	<div class="max-w-screen-xl md:mx-24 w-full h-screen relative mx-auto md:flex md:items-center">
		<div class="w-full py-12 mf:py-0 h-full flex flex-wrap content-between md:h-[500px]">
			<p
				class="section-seven__heading text-center md:text-left w-full text-center md:text-left md:max-w-[427px] title-font text-[5.2vh] md:text-[4.2vh] md:leading-snug leading-tight"
			>
				Small and Beautiful
			</p>
			<p
				class="section-seven__title2 title-font md:text-[2.6vh] text-[4.2vh]  leading-9 max-w-[309px] mx-auto md:max-w-none text-center md:text-right w-full"
			>
				Mili-second executions <br />from anywhere in the world
			</p>
		</div>
		<img
			src="/assets/hub-top.png"
			srcset={imgSrcSet('/assets/hub-top.png')}
			class="h-full w-full max-w-[800px] max-h-[785px] object-contain md:mr-0 md:ml-auto section-seven__product-image absolute h-full flex items-center top-0 left-0 md:relative md:top-auto md:left-auto md:h-auto -z-10"
			alt=""
		/>
	</div>
</div>

<div
	class="section-eight h-screen w-full relative bg-[#D0CDC8] overflow-hidden bg-gradient-to-r from-[#D0CDC8] to-[#eae7e4]"
>
	<div class="max-w-screen-xl md:mx-24 w-full h-screen relative mx-auto md:flex md:items-center">
		<p
			class="section-eight__heading text-[4.2vh] md:text-[2.6vh] leading-8 title-font absolute w-full text-center md:text-right left-1/2 md:left-[20%] md:top-1/2 md:bottom-auto transform -translate-x-1/2 md:-translate-y-full max-w-[230px] ml-0 md:mx-auto top-[15vh]"
		>
			Wall or tabletop You decide
		</p>
		<img
			src="/assets/hub-wall-and-desk-mount.jpg"
			srcset={imgSrcSet('/assets/hub-wall-and-desk-mount.jpg')}
			class="h-full w-full mx-auto object-cover section-eight__product-image hidden md:block"
			alt=""
		/>
		<img
			src="/assets/hub-wall-and-desk-mount-mobile.jpg"
			srcset={imgSrcSet('/assets/hub-wall-and-desk-mount.jpg')}
			class="h-full w-full md:w-8/12 object-cover md:mr-0 md:ml-auto section-eight__product-image md:hidden"
			alt=""
		/>
	</div>
</div>
