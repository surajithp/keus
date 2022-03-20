<script context="module">
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger.js';

	export const sectionFourAnimation = () => {
		// Section four
		let sectionFour = gsap.timeline({
			duration: 2,
			scrollTrigger: {
				trigger: '.section-four',
				start: 'top top',
				pin: true,
				scrub: 1
			}
		});

		ScrollTrigger.matchMedia({
			// desktop text timeline
			'(min-width: 768px)': function () {
				sectionFour
					.fromTo(
						'.section-four__heading',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: 0,
							yPercent: 100
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							xPercent: 10,
							duration: 1
						},
						0
					)
					.to('.section-four__heading', {}, '+=1');
			},
			// mobile text timeline
			'(max-width: 767px)': function () {
				sectionFour
					.fromTo(
						'.section-four__product-image',
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: 0
						},
						{
							ease: 'power4.easeOut',
							yPercent: -20,
							duration: 1
						},
						1
					)
					.fromTo(
						'.section-four__heading',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: 25
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: -25
						},
						1
					)

					.to('.section-four__heading', {}, '+=1');
			},
			all: function () {}
		});
	};
</script>
