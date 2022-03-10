<script context="module">
	import { gsap } from 'gsap/dist/gsap';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';

	gsap.registerPlugin(ScrollTrigger);

	export const sectionFiveAnimation = () => {
		// Section five
		let sectionFive = gsap.timeline({
			scrollTrigger: {
				trigger: '.section-five',
				start: 'top top',
				pin: true,
				scrub: 1
			}
		});

		ScrollTrigger.matchMedia({
			// desktop text timeline
			'(min-width: 768px)': function () {
				sectionFive
					.fromTo(
						'.section-five__heading',
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
						'.section-five__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: 0
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							xPercent: 10,
							duration: 1
						},
						3
					);
			},
			// mobile text timeline
			'(max-width: 767px)': function () {
				sectionFive

					.fromTo(
						'.section-five__heading',
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
						'.section-five__product-image',
						{
							ease: 'power4.easeOut',
							yPercent: -25,
							duration: 1
						},
						1
					)
					.fromTo(
						'.section-five__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: 25
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: -50,
							duration: 1
						}
					)
					.to('.section-three__heading', {}, '+=1');
			},
			all: function () {}
		});
	};
</script>
