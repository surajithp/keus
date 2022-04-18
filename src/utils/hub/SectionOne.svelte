<script context="module">
	import { onMount } from 'svelte';

	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger.js';

	export const sectionOneAnimation = () => {
		// Section one

		ScrollTrigger.matchMedia({
			// desktop text timeline
			'(min-width: 768px)': function () {
				let sectionOne = gsap.timeline({
					scrollTrigger: {
						trigger: '.section-one',
						start: 'top top',
						end: '3000px',
						pin: true,
						scrub: 0.5
					}
				});
				sectionOne
					.fromTo(
						'.section-one__heading',
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: 0
						},
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							duration: 2,
							yPercent: -10
						},
						'+=1'
					)
					.fromTo(
						'.section-one__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -10
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							xPercent: 0,
							duration: 2
						},
						'+=1'
					)
					.to(
						'.section-one__title2',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -10,
							duration: 2
						},
						'+=1'
					)
					.fromTo(
						'.section-one__title3',
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							xPercent: -10
						},
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							xPercent: 0,
							duration: 2
						},
						'+=1'
					)
					.to('.section-one__title3', {}, '+=1');
			},
			// mobile text timeline
			'(max-width: 767px)': function () {
				let sectionOne = gsap.timeline({
					scrollTrigger: {
						trigger: '.section-one',
						start: 'top top',
						end: '1300px',
						pin: true,
						scrub: 0.5
					}
				});
				sectionOne

					.fromTo(
						'.section-one__heading',
						{
							autoAlpha: 1,
							ease: 'power4.easeOut',
							yPercent: 0
						},
						{
							autoAlpha: 0,
							ease: 'power4.easeOut',
							yPercent: -10
						},
						1
					)
					.to(
						'.section-one__product-image',
						{
							ease: 'power4.easeOut',
							yPercent: -10,
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
					)
					.to('.section-one__title2', {}, '+=1');
			},
			all: function () {}
		});
	};
</script>
