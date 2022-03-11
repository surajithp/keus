<script context="module">
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/ScrollTrigger';

	gsap.registerPlugin(ScrollTrigger);

	export const sectionOneAnimation = () => {
		// Section one
		let sectionOne = gsap.timeline({
			scrollTrigger: {
				trigger: '.section-one',
				start: 'top top',
				pin: true,
				scrub: 1
			}
		});

		ScrollTrigger.matchMedia({
			// desktop text timeline
			'(min-width: 768px)': function () {
				sectionOne
					.fromTo(
						'.section-one__heading',
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
						'.section-one__title2',
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
						'.section-one__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -25,
							duration: 1
						},
						'+=1'
					)
					.fromTo(
						'.section-one__title3',
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
						}
					)
					.to(
						'.section-one__title3',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -25,
							duration: 1
						},
						'+=1'
					)
					.to('.section-one__title3', {}, '+=1');
			},
			// mobile text timeline
			'(max-width: 767px)': function () {
				sectionOne

					.fromTo(
						'.section-one__heading',
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
						'.section-one__product-image',
						{
							ease: 'power4.easeOut',
							yPercent: -18,
							duration: 1
						},
						1
					)
					.fromTo(
						'.section-one__title2',
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
						'.section-one__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: -40,
							duration: 1
						},
						'+=1'
					)
					.fromTo(
						'.section-one__title3',
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
