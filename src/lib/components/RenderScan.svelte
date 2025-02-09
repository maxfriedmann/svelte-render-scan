<script lang="ts">
	import { onMount, onDestroy } from 'svelte';

	class Highlight {
		#element: HTMLDivElement;
		#title: HTMLDivElement;
		target: Node;
		constructor(target: Node) {
			this.target = target;

			this.#element = document.createElement('div');
			this.#title = document.createElement('div');
			this.#element.classList.add('render-perf__box');
			this.#title.classList.add('render-perf__title');
			this.#element.appendChild(this.#title);
		}

		#reasons = new Map<string, number>();
		#totalRerenders = 0;

		notify(why: string) {
			this.#reasons.set(why, (this.#reasons.get(why) || 0) + 1);
			this.#totalRerenders++;

			this.#render();
		}

		get title() {
			let reasons = [...this.#reasons.entries()]
				.map(([key, count]) => `${key} (${count})`)
				.join(' , ');
			let total = 0;

			this.#reasons.values().forEach((x) => (total += x));

			return `x${total} | ${reasons}`;
		}

		#fadeOut: ReturnType<typeof setTimeout> = setTimeout(() => {}, 0);
		#frame: number = 0;
		#isVisible = false;

		updatePosition() {
			if (!this.#isVisible) return;

			let rect = this.#getRect();
			if (!rect) return;

			if (rect.y > window.innerHeight) return;
			if (rect.x > window.innerWidth) return;

			Object.assign(this.#element.style, {
				top: rect.y + 'px',
				left: rect.x + 'px',
				width: rect.width + 'px',
				height: rect.height + 'px'
			});
		}

		#render() {
			cancelAnimationFrame(this.#frame);
			clearTimeout(this.#fadeOut);

			this.#frame = requestAnimationFrame(() => {
				let rect = this.#getRect();

				if (!rect) return;
				if (rect.y > window.innerHeight) return;
				if (rect.x > window.innerWidth) return;

				if (!overlayEl?.contains(this.#element)) {
					overlayEl?.appendChild(this.#element);
					activeHighlights.add(this);
				}

				this.#title.textContent = this.title;
				this.#isVisible = true;

				Object.assign(this.#element.style, {
					top: rect.y + 'px',
					left: rect.x + 'px',
					width: rect.width + 'px',
					height: rect.height + 'px'
				});

				this.#element.style.opacity = '1';
				this.#fadeOut = setTimeout(() => {
					this.#element.style.opacity = '0';
					this.#reasons.clear();
					this.#isVisible = false;
					activeHighlights.delete(this);
				}, 1000);
			});
		}

		#getRect() {
			if (this.target instanceof Element) {
				let rect = this.target.getBoundingClientRect();

				return {
					x: rect.x,
					y: rect.y,
					width: rect.width,
					height: rect.height
				};
			}

			let range = document.createRange();
			range.selectNodeContents(this.target);
			let rects = range.getClientRects();
			let rect = rects[0];

			if (!rect) {
				console.log(`Could not determine coordinates of `, this.target);
				return;
			}

			return {
				x: rect.x,
				y: rect.y,
				width: rect.width,
				height: rect.height
			};
		}
	}

	let overlayEl: HTMLDivElement | null = null;
	let mutationObserver: MutationObserver | null = null;
	let cache = new WeakMap<Node, Highlight>();
	let activeHighlights = new Set<Highlight>();
	let scrollFrame: number = 0;

	function highlightNode(element: Node, why: string) {
		let existing = cache.get(element);

		if (!existing) {
			existing = new Highlight(element);
			cache.set(element, existing);
		}

		existing.notify(why);
	}

	function handleScroll() {
		// Debounce scroll updates with requestAnimationFrame
		cancelAnimationFrame(scrollFrame);
		scrollFrame = requestAnimationFrame(() => {
			// Update all visible highlights
			activeHighlights.forEach((highlight) => {
				console.log('updating', highlight);
				highlight.updatePosition();
			});
		});
	}

	onMount(() => {
		// Create overlay container
		overlayEl = document.createElement('div');
		overlayEl.classList.add('render-perf__container');
		document.body.appendChild(overlayEl);

		// Setup mutation observer
		mutationObserver = new MutationObserver((mutationList) => {
			for (let mutation of mutationList) {
				if (mutation.target instanceof Element) {
					let shouldIgnore = mutation.target.getAttribute('class')?.includes('render-perf__');

					if (shouldIgnore) {
						continue;
					}
				}

				switch (mutation.type) {
					case 'attributes':
						highlightNode(mutation.target, 'attribute changed');
						break;
					case 'childList':
						highlightNode(mutation.target, 'children changed');
						break;
					case 'characterData':
						highlightNode(mutation.target, 'text changed');
						break;
					default:
						console.log(`Unhandled mutation type: ${mutation.type}`);
				}
			}
		});

		mutationObserver.observe(document.body, {
			subtree: true,
			childList: true,
			attributes: true,
			characterData: true
		});

		// Add scroll listener
		window.addEventListener('scroll', handleScroll, { passive: true });
	});

	onDestroy(() => {
		mutationObserver?.disconnect();
		overlayEl?.remove();
		window.removeEventListener('scroll', handleScroll);
		cancelAnimationFrame(scrollFrame);
		mutationObserver = null;
		overlayEl = null;
		cache = new WeakMap();
		activeHighlights.clear();
	});
</script>

<style>
	:global(.render-perf__container) {
		position: fixed;
		pointer-events: none;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		box-sizing: border-box;
		z-index: 1000000000;
	}

	:global(.render-perf__box) {
		box-sizing: border-box;
		font-family: sans-serif;
		display: block;
		position: fixed;
		pointer-events: none;
		border: 1px solid #2189b5;
		background-color: rgba(200, 0, 255, 0.05);
		transition: opacity 0.25s;
	}

	:global(.render-perf__title) {
		display: inline-block;
		min-width: max-content;
		box-sizing: content-box;
		font-size: 0.75rem;
		border: 1px solid #2189b5;
		border-bottom: none;
		padding: 0.125rem 0.5rem;
		color: white;
		background-color: #2189b5;
		position: relative;
		top: -1.5rem;
	}
</style>
