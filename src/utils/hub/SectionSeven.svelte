<script context="module">
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger.js';

	export const sectionSevenAnimation = () => {
		// Section seven
		let sectionSeven = gsap.timeline({
			scrollTrigger: {
				trigger: '.section-seven',
				start: 'top top',
				pin: true,
				scrub: 1
			}
		});

		ScrollTrigger.matchMedia({
			// desktop text timeline
			'(min-width: 768px)': function () {
				sectionSeven
					.fromTo(
						'.section-seven__heading',
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
						'.section-seven__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -20
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							xPercent: 0,
							duration: 1
						},
						'+=1'
					)
					.to('.section-seven__heading', {}, '+=1');
			},
			// mobile text timeline
			'(max-width: 767px)': function () {
				sectionSeven

					.fromTo(
						'.section-seven__heading',
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: -100
						},
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: -110
						},
						1
					)
					.to(
						'.section-seven__product-image',
						{
							ease: 'power4.easeOut',
							yPercent: -17,
							duration: 1
						},
						1
					)
					.fromTo(
						'.section-seven__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: -90
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: -100,
							duration: 1
						}
					)
					.to('.section-seven__heading', {}, '+=1');
			},
			all: function () {}
		});
	};
</script>
