<script context="module">
	import Text from './utils/Text.svelte';

	export class CustomVisualization extends HTMLElement {
		connectedCallback() {
			// Get custom element attributes
			const attributeNames = new Set(this.getAttributeNames());
			const props = {}
			for (const attributeName of attributeNames) {
				// Try to JSON parse, and if that fails assume to be a string input
				try {
					props[attributeName] = JSON.parse(this.getAttribute(attributeName));
				} catch (_e) {
					props[attributeName] = this.getAttribute(attributeName);
				}
			}

			// Create a mount point
			const mountPoint = document.createElement("div")

			// Put a Svelte component inside of it (note it can't be directly
			// declared in this file).
			new Text({
				target: mountPoint,
				props
			});

			// Render
    		this.attachShadow({ mode: "open" }).appendChild(mountPoint);
		}
	}
</script>
