<script context="module">
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger.js';

	export const sectionFourAnimation = () => {
		// Section four

		ScrollTrigger.matchMedia({
			// desktop text timeline
			'(min-width: 768px)': function () {
				let sectionFour = gsap.timeline({
					duration: 3,
					scrollTrigger: {
						trigger: '.section-four',
						start: 'top top',
						pin: true,
						scrub: 1
					}
				});
				sectionFour
					.fromTo(
						'.section-four__heading',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -75,
							yPercent: 100
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							xPercent: -50,
							duration: 1
						},
						0
					)
					.to('.section-four__heading', {}, '+=1');
			},
			// mobile text timeline
			'(max-width: 767px)': function () {
				let sectionFour = gsap.timeline({
					duration: 1.5,
					scrollTrigger: {
						trigger: '.section-four',
						start: 'top top',
						pin: true,
						scrub: 1
					}
				});
				sectionFour
					.fromTo(
						'.section-four__product-image',
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: 10
						},
						{
							ease: 'power4.easeOut',
							yPercent: 0,
							duration: 1
						},
						1
					)
					.fromTo(
						'.section-four__heading',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: 15
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: -100
						},
						1
					)

					.to('.section-four__heading', {}, '+=1');
			},
			all: function () {}
		});
	};
</script>
