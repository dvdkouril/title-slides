<script lang="ts">
	import { onMount } from "svelte";

	let appEl: HTMLElement;

	function makeGraphic(
		parentEl: HTMLElement,
		text: string,
		t: number = 1,
		mouseX = 1,
		mouseY = 1,
	) {
		const svgEl = document.createElementNS(
			"http://www.w3.org/2000/svg",
			"svg",
		);
		svgEl.setAttribute("width", "100%");
		svgEl.setAttribute("height", "100%");
		svgEl.setAttribute("style", "font-family: sans-serif; display: block;");

		const numOfGhosts = 50;
		const docW = document.documentElement.clientWidth;
		const docH = document.documentElement.clientHeight;
		const x = mouseX - docW / 2;
		const y = mouseY - docH / 2;
		const dirMagnitude = Math.sqrt(x * x + y * y);
		//const offsetDirectionX = x / dirMagnitude;
		const offsetDirectionX = 0;
		const offsetDirectionY = y / dirMagnitude;
		console.log(`${offsetDirectionX}, ${offsetDirectionY}`);
		const offsetSpacing = 5;
		for (let i = 0; i < numOfGhosts; i++) {
			const offset = i * offsetSpacing;
			const textTest = document.createElementNS(
				"http://www.w3.org/2000/svg",
				"text",
			);
			const opacity = i / numOfGhosts;
			textTest.textContent = text;
			const sinOffset = Math.sin(t * 0.002 + i * 0.5) * 10;
			textTest.setAttribute(
				"x",
				`${10 + offset * offsetDirectionX + sinOffset}`,
			);
			textTest.setAttribute("y", `${800 + offset * offsetDirectionY}`);
			textTest.setAttribute("font-size", `${70}`);
			//textTest.setAttribute("fill", "none");
			textTest.setAttribute("fill", "#ffffff");
			//textTest.setAttribute("fill-opacity", `${opacity}`);
			textTest.setAttribute("stroke", "black");
			textTest.setAttribute("stroke-opacity", `${opacity}`);
			textTest.setAttribute("stroke-width", `${0.5}`);
			svgEl.appendChild(textTest);
		}

		parentEl.replaceChildren();
		parentEl.appendChild(svgEl);

		//return svgEl;
	}

	onMount(() => {
		let tick = (t: number) => {
			console.log("hi");
			requestAnimationFrame(tick);
			makeGraphic(
				appEl,
				"a path toward composable visualization tools",
				t,
			);
		};
		requestAnimationFrame(tick);

		//let loop = (now: number) => {
		//	let pixelDelayMs = 100;
		//	let fadeDurationMs = 700;
		//
		//	tpixels = pixels.map((pixel, i) => {
		//		let firstTimestamp = pixels[0].timestamp;
		//		let globalStart = firstTimestamp + globalThis.api.delay;
		//		let pixelStart = pixel.timestamp + pixelDelayMs;
		//		let animStart = Math.max(globalStart, pixelStart);
		//
		//		if (now < animStart) {
		//			return { x: pixel.x, y: pixel.y };
		//		}
		//
		//		let fade = Math.min(1, (now - animStart) / fadeDurationMs);
		//		let t = (now - animStart) / 1000;
		//
		//		let fn = resolveAnimation();
		//		let { x, y } = fn(i, t);
		//
		//		return {
		//			x: pixel.x + x * fade,
		//			y: pixel.y + y * fade,
		//		};
		//	});
		//
		//	requestAnimationFrame(loop);
		//};
		//
		//requestAnimationFrame(loop);
	});
</script>

<div bind:this={appEl} style="width: 100vw; height: 100vh">
	<h2>a path toward composable visualization tools</h2>
	<!--<svg></svg>-->
</div>
