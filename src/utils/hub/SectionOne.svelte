<script context="module">
	import { gsap } from 'gsap/dist/gsap';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';

	gsap.registerPlugin(ScrollTrigger);

	export const sectionOneAnimation = () => {
		// Section Zero
		let sectionZero = gsap.timeline({
			scrollTrigger: {
				trigger: '.section-zero',
				start: 'top top',
				pin: true,
				scrub: 1
			}
		});

		ScrollTrigger.matchMedia({
			// desktop text timeline
			'(min-width: 768px)': function () {
				sectionZero
					.fromTo(
						'.section-zero__heading',
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
						'.section-zero__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -25
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							xPercent: 30,
							duration: 1
						},
						3
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
			},
			// mobile text timeline
			'(max-width: 767px)': function () {
				sectionZero

					.fromTo(
						'.section-zero__heading',
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
					.to(
						'.section-zero__product-image',
						{
							ease: 'power4.easeOut',
							yPercent: -25,
							duration: 1
						},
						1
					)
					.fromTo(
						'.section-zero__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: 25
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: 0,
							duration: 1
						}
					)
					.to(
						'.section-zero__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: -40,
							duration: 1
						},
						'+=1'
					)
					.fromTo(
						'.section-zero__title3',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut'
							// xPercent: -25
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							// xPercent: 40,
							duration: 1
						}
					);
			},
			all: function () {}
		});
	};
</script>
