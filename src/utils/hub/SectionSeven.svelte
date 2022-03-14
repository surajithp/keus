<script context="module">
	// import { gsap } from 'gsap';
	// import { ScrollTrigger } from 'gsap/ScrollTrigger';

	export const sectionSevenAnimation = () => {
		// Section seven
		let sectionSeven = gsap.timeline({
			scrollTrigger: {
				trigger: '.section-seven',
				start: 'top top',
				end: '3000px',
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
						'.section-seven__product-image',
						{
							ease: 'power4.easeOut',
							yPercent: -25,
							duration: 1
						},
						1
					)
					.fromTo(
						'.section-seven__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: 25
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: -70,
							duration: 1
						}
					)
					.to('.section-seven__heading', {}, '+=1');
			},
			all: function () {}
		});
	};
</script>
