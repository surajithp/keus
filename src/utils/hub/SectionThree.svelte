<script context="module">
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger.js';

	export const sectionThreeAnimation = () => {
		// Section three
		let sectionThree = gsap.timeline({
			duration: 3,
			scrollTrigger: {
				trigger: '.section-three',
				start: 'top top',
				pin: true,
				scrub: 1
			}
		});

		ScrollTrigger.matchMedia({
			// desktop text timeline
			'(min-width: 768px)': function () {
				sectionThree
					.fromTo(
						'.section-three__heading',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: 0
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							xPercent: 20,
							duration: 1
						},
						0
					)
					.to('.section-three__heading', {}, '+=1');
			},
			// mobile text timeline
			'(max-width: 767px)': function () {
				sectionThree
					.fromTo(
						'.section-three__heading',
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: 25
						},
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: 20
						},
						1
					)
					.fromTo(
						'.section-three__product-image',
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
					.to('.section-three__heading', {}, '+=1');
			},
			all: function () {}
		});
	};
</script>
