<script>
	import { onMount } from 'svelte';
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/ScrollTrigger';

	import '../app.css';

	let innerWidth = window.innerWidth;
	let innerHeight = window.innerHeight;

	// let frameNumber = 0,
	// 	playbackConst = 500,
	// 	setHeight,
	// 	video,
	// 	canvas;

	// /* Make sure the video is 'activated' on iOS */
	// function once(el, event, fn, opts) {
	// 	var onceFn = function (e) {
	// 		el.removeEventListener(event, onceFn);
	// 		fn.apply(this, arguments);
	// 	};
	// 	el.addEventListener(event, onceFn, opts);
	// 	return onceFn;
	// }
	// onMount(() => {
	// 	var ctx = canvas.getContext('2d');

	// 	// set canvas size = video size when known
	// 	canvas.width = window.innerWidth;
	// 	canvas.height = window.innerHeight;

	// 	video.addEventListener(
	// 		'play',
	// 		function () {
	// 			var $this = this; //cache
	// 			(function loop() {
	// 				if (!$this.paused && !$this.ended) {
	// 					ctx.drawImage($this, 0, 0);
	// 					setTimeout(loop, 1000 / 30); // drawing at 30fps
	// 				}
	// 			})();
	// 		},
	// 		0
	// 	);

	// 	/* ---------------------------------- */
	// 	/* Scroll Control! */

	// 	gsap.registerPlugin(ScrollTrigger);

	// 	let tl = gsap.timeline({
	// 		defaults: { duration: 12 },
	// 		scrollTrigger: {
	// 			trigger: '#container',
	// 			start: 'top top',
	// 			end: 'bottom bottom',
	// 			scrub: true
	// 		}
	// 	});
	// 	const updateCanvas = () => {
	// 		console.log('callback');
	// 		ctx.drawImage(video, 0, 0, window.innerWidth, window.innerHeight);
	// 	};

	// 	once(video, 'loadedmetadata', () => {
	// 		tl.fromTo(
	// 			video,
	// 			{
	// 				currentTime: 0
	// 			},
	// 			{
	// 				currentTime: video.duration || 1
	// 			}
	// 		);
	// 	});
	// 	tl.eventCallback('onUpdate', updateCanvas);
	// });

	// onMount(() => {
	// 	gsap.registerPlugin(ScrollTrigger);

	// 	// var frame_count = 95,
	// 	// 	offset_value = 1920;
	// 	// var frameWidth = 1920,
	// 	// 	frameHeight = 1080,
	// 	// 	numCols = 5,
	// 	// 	numRows = 19;

	// 	const rows = 19;
	// 	const columns = 5;
	// 	const frame_count = rows * columns;
	// 	const imageWidth = 9600;
	// 	const imageHeight = 20520;
	// 	const horizDiff = imageWidth / columns;
	// 	const vertDiff = imageHeight / rows;

	// 	gsap.set('.viewer-one', { width: horizDiff, height: vertDiff });

	// 	const setPosX = gsap.quickSetter('.image-scene', 'xPercent');
	// 	const setPosY = gsap.quickSetter('.image-scene', 'yPercent');

	// 	const obj = { num: 0 };
	// 	gsap.to(obj, {
	// 		num: frame_count,
	// 		ease: 'steps(' + frame_count + ')',
	// 		scrollTrigger: {
	// 			trigger: '.section-one',
	// 			start: 'top top',
	// 			end: '+=' + imageHeight,
	// 			pin: true,
	// 			anticipatePin: 1,
	// 			scrub: 1
	// 		},
	// 		onUpdate: () => {
	// 			setPosX((-Math.round((obj.num % columns) * horizDiff) * 100) / imageWidth);
	// 			setPosY((-Math.round((obj.num % columns) * vertDiff) * 100) / imageHeight);
	// 		}
	// 	});
	// });

	onMount(() => {
		gsap.registerPlugin(ScrollTrigger);

		const canvas = document.getElementById('hero-lightpass');
		const context = canvas.getContext('2d');

		canvas.width = innerWidth;
		canvas.height = innerHeight;

		const frameCount = 95;
		const currentFrame = (index) =>
			`/assets/bscene/bscene_${(index + 1).toString().padStart(3, '0')}.jpg`;

		const images = [];
		const airpods = {
			frame: 0
		};

		for (let i = 0; i < frameCount; i++) {
			const img = new Image();
			img.src = currentFrame(i);
			images.push(img);
		}

		gsap.to(airpods, {
			frame: frameCount - 1,
			snap: 'frame',
			ease: 'none',
			scrollTrigger: {
				trigger: '.section-one',
				start: 'top top',
				markers: true,
				pin: true,
				scrub: 0.5
			},
			onUpdate: render // use animation onUpdate instead of scrollTrigger's onUpdate
		});

		images[0].onload = render;

		function render() {
			context.clearRect(0, 0, canvas.width, canvas.height);
			context.drawImage(images[airpods.frame], 0, 0);
		}
	});
</script>

<!-- <canvas id="canvas" bind:this={canvas} />

<video
	id="v0"
	tabindex="0"
	autobuffer="autobuffer"
	preload="preload"
	bind:this={video}
	playsinline="true"
	webkit-playsinline="true"
	muted
	controls
	src="/assets/bedroom_animation.mp4"
/>
<div id="container" />

<style>
	/* #v0 {
		width: 0;
		height: 0;
	} */
	#canvas {
		position: fixed;
		top: 50%;
		left: 50%;
		min-width: 100%;
		min-height: 100%;
		transform: translate(-50%, -50%);
	}
	#container {
		height: 500vh;
	}
</style> -->
<svelte:window bind:innerWidth bind:innerHeight />
<div class="h-screen bg-blue-100" />
<section class="scene section section-one h-screen">
	<div class="viewer viewer-one relative overflow-hidden" />
	<canvas id="hero-lightpass" width="" />
</section>

<div class="h-screen bg-green-100" />

<style>
	html,
	body {
		height: 100%;
		background: #021c53;
	}

	.section {
		height: 100%;
		background: black;
		color: white;
	}

	.scene {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100%;
		width: 100%;
		background: black;
	}

	.viewer {
		max-width: 100%;
		max-height: 100%;
		margin-left: auto;
		margin-right: auto;
		/* object-fit: contain; */
		/* background-image: url('/assets/bscene.jpg'); */
		/* background-size: 11580px 11550px; */
		background-repeat: no-repeat;
		background-position: 0 0;
	}
</style>
