<script>
	import { browser } from '$app/environment';
	import Close from '$lib/icons/close.svelte';
	import QrIcon from '$lib/icons/qr.svelte';

	const labels = {
		en: {
			open: 'Show QR code',
			title: 'Scan to open this link',
			close: 'Close QR modal',
			image: 'QR code for shared link'
		},
		he: {
			open: 'הצג קוד QR',
			title: 'סרקו לפתיחת הקישור',
			close: 'סגור חלון QR',
			image: 'קוד QR לקישור שיתוף'
		}
	};

	/**
	 * @typedef {Object} Props
	 * @property {string} url
	 * @property {'en' | 'he'} [lang]
	 * @property {number} [size]
	 * @property {number} [qrImageSize]
	 * @property {(blockUrl: string, imageSize?: number) => string} [buildQrUrl]
	 */

	/** @type {Props} */
	let { url, lang = 'en', size = 48, qrImageSize = 320, buildQrUrl = undefined } = $props();

	let isOpen = $state(false);

	const currentLang = $derived(lang in labels ? lang : 'en');

	/** @param {string} blockUrl @param {number} [imageSize] */
	function defaultBuildQrUrl(blockUrl, imageSize = 320) {
		return `https://api.qrserver.com/v1/create-qr-code/?size=${imageSize}x${imageSize}&data=${encodeURIComponent(blockUrl)}`;
	}

	/**
	 * @param {string} blockUrl
	 * @param {number} imageSize
	 * @param {((blockUrl: string, imageSize?: number) => string) | undefined} urlBuilder
	 */
	function getQrSource(blockUrl, imageSize, urlBuilder) {
		const safeSize = Number.isFinite(imageSize) && imageSize > 0 ? imageSize : 320;
		const selectedBuilder = typeof urlBuilder === 'function' ? urlBuilder : defaultBuildQrUrl;

		try {
			const result = selectedBuilder(blockUrl, safeSize);
			if (typeof result === 'string' && result.length > 0) {
				return result;
			}
		} catch (error) {
			console.error('Failed to build QR URL:', error);
		}

		return defaultBuildQrUrl(blockUrl, safeSize);
	}

	const qrSource = $derived(getQrSource(url, qrImageSize, buildQrUrl));

	const openModal = () => {
		isOpen = true;
	};

	const closeModal = () => {
		isOpen = false;
	};

	/** @param {MouseEvent} event */
	const handleOverlayClick = (event) => {
		if (event.target === event.currentTarget) {
			closeModal();
		}
	};

	/** @param {KeyboardEvent} event */
	const handleKeyDown = (event) => {
		if (event.key === 'Escape') {
			closeModal();
		}
	};

	$effect(() => {
		if (!browser || !isOpen) {
			return;
		}

		const previousOverflow = document.body.style.overflow;
		document.body.style.overflow = 'hidden';

		return () => {
			document.body.style.overflow = previousOverflow;
		};
	});
</script>

<button onclick={openModal}>
	<span class="sr-only">{labels[currentLang].open}</span>
	<QrIcon width={size} height={size} />
</button>

{#if isOpen}
	<div
		class="qr-modal"
		role="dialog"
		aria-modal="true"
		aria-label={labels[currentLang].title}
		tabindex="-1"
		onclick={handleOverlayClick}
		onkeydown={handleKeyDown}
	>
		<div class="qr-panel">
			<button class="close-button" onclick={closeModal}>
				<span class="sr-only">{labels[currentLang].close}</span>
				<Close width={28} height={28} />
			</button>
			<p>{labels[currentLang].title}</p>
			<img src={qrSource} alt={labels[currentLang].image} width={qrImageSize} height={qrImageSize} />
		</div>
	</div>
{/if}

<style>
	button {
		background: transparent;
		border-style: none;
		transition: all 0.2s ease-in-out;
		padding: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
	}

	button:focus,
	button:hover {
		transform: scale(1.1);
	}

	.qr-modal {
		position: fixed;
		inset: 0;
		z-index: 9999;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 24px;
		background: rgba(15, 23, 42, 0.75);
	}

	.qr-panel {
		position: relative;
		width: min(100%, 460px);
		border-radius: 14px;
		padding: 36px 24px 24px;
		background: #fff;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 16px;
		box-shadow: 0 24px 48px rgba(15, 23, 42, 0.25);
	}

	.qr-panel p {
		margin: 0;
		font-size: 1rem;
		font-weight: 600;
		color: #0f172a;
	}

	.qr-panel img {
		max-width: 100%;
		height: auto;
		border-radius: 8px;
		background: #fff;
	}

	.close-button {
		position: absolute;
		top: 12px;
		right: 12px;
	}

	:global(.sr-only) {
		position: absolute;
		width: 1px;
		height: 1px;
		padding: 0;
		margin: -1px;
		overflow: hidden;
		clip: rect(0, 0, 0, 0);
		white-space: nowrap;
		border-width: 0;
	}
</style>
