<script>
	/**
	 * @typedef {Object} Props
	 * @property {number} [width]
	 * @property {number} [height]
	 */

	/** @type {Props} */
	let { width = 48, height = 48 } = $props();
</script>

<svg {width} {height} viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
	<rect x="3" y="3" width="7" height="7" rx="1.2" stroke="currentColor" stroke-width="2" />
	<rect x="14" y="3" width="7" height="7" rx="1.2" stroke="currentColor" stroke-width="2" />
	<rect x="3" y="14" width="7" height="7" rx="1.2" stroke="currentColor" stroke-width="2" />
	<rect x="6" y="6" width="1" height="1" fill="currentColor" stroke="currentColor" stroke-width="1" />
	<rect x="17" y="6" width="1" height="1" fill="currentColor" stroke="currentColor" stroke-width="1" />
	<rect x="6" y="17" width="1" height="1" fill="currentColor" stroke="currentColor" stroke-width="1" />
	<rect x="14" y="14" width="2" height="2" fill="currentColor" />
	<rect x="18" y="14" width="2" height="2" fill="currentColor" />
	<rect x="14" y="18" width="2" height="2" fill="currentColor" />
	<rect x="18" y="18" width="2" height="2" fill="currentColor" />
</svg>

<style>
	svg {
		color: rgb(107, 114, 128);
	}

	svg:hover {
		color: #ff0092;
	}
</style>
