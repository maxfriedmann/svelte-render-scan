# Svelte Render Scan

Visual debugging tool for Svelte applications.

Svelte port of [render-scan](https://github.com/NullVoxPopuli/render-scan//).

![Screenshot of svelte-render-scan in action](.github/render-scan-demo.gif)

## Installation

```bash
npm install -D svelte-render-scan
```

## Usage

```svelte
<script>
	import { RenderScan } from 'svelte-render-scan';
</script>

<RenderScan />
```

The overlay button appears in the bottom right corner and is enabled by default. You can disable it by clicking on it or setting the `initialEnabled` prop to `false`.

## Props

- `initialEnabled` (default: `true`) - Start with render scanning enabled
- `offsetLeft` (default: `0`) - Offset the button left from its default position

## Development

Clone this repository and install dependencies:

```bash
nvm use
npm install
npm run dev
```

## License

MIT
