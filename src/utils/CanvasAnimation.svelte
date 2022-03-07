<script>
	import { onMount } from 'svelte';
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/ScrollTrigger';
	export let frameCount, assetUrl;
	let canvas, innerHeight;

	onMount(() => {
		gsap.registerPlugin(ScrollTrigger);

		const context = canvas.getContext('2d');

		canvas.width = document.body.clientWidth;
		canvas.height = innerHeight;

		const currentFrame = (index) => `${assetUrl}${(index + 1).toString().padStart(3, '0')}.jpg`;

		const images = [];
		const frames = {
			frame: 0
		};

		for (let i = 0; i < frameCount; i++) {
			const img = new Image();
			img.src = currentFrame(i);
			images.push(img);
		}

		gsap.timeline().to(frames, {
			frame: frameCount - 1,
			snap: 'frame',
			ease: 'none',
			scrollTrigger: {
				trigger: canvas,
				start: 'top top',
				pin: true,
				scrub: 0.5
			},
			onUpdate: render // use animation onUpdate instead of scrollTrigger's onUpdate
		});

		images[0].onload = render;

		function render() {
			context.clearRect(0, 0, canvas.width, canvas.height);
			context.drawImage(images[frames.frame], 0, 0);
		}
	});
</script>

<svelte:window bind:innerHeight />

<canvas class="canvas" bind:this={canvas} />
