<script context="module">
	import { gsap } from 'gsap/dist/gsap';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';

	gsap.registerPlugin(ScrollTrigger);

	export const sectionEightAnimation = () => {
		// Section eight
		let sectionEight = gsap.timeline({
			duration: 3,
			scrollTrigger: {
				trigger: '.section-eight',
				start: 'top top',
				pin: true,
				scrub: 1
			}
		});

		ScrollTrigger.matchMedia({
			// desktop text timeline
			'(min-width: 768px)': function () {
				sectionEight
					.fromTo(
						'.section-eight__heading',
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
          .to('.section-eight__heading', {}, '+=1');
			},
			// mobile text timeline
			'(max-width: 767px)': function () {
				sectionEight
					.fromTo(
						'.section-eight__heading',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: 0
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
              duration: 1,
							yPercent: -30
						},
					)
          .to('.section-eight__heading', {}, '+=1');
			},
			all: function () {}
		});
	};
</script>
