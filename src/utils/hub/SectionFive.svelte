<script context="module">
	// import { gsap } from 'gsap';
	// import { ScrollTrigger } from 'gsap/ScrollTrigger';

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
					)
					.to('.section-three__heading', {}, '+=2');
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
							yPercent: -40,
							duration: 1
						},
						1
					)
					.fromTo(
						'.section-five__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: -80
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: -100,
							duration: 1
						}
					)
					.to('.section-three__heading', {}, '+=1');
			},
			all: function () {}
		});
	};
</script>
