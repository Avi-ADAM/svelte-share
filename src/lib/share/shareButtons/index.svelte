<script>
	import { browser } from '$app/environment';
	import Mail from '$lib/share/shareButtons/Mail.svelte';
	import ShareIcon from '$lib/icons/share.svelte';
	import Facebook from '$lib/share/shareButtons/Facebook.svelte';
	import Telegram from '$lib/share/shareButtons/Telegram.svelte';
	import Twitter from '$lib/share/shareButtons/Twitter.svelte';
	import ShareOp from '$lib/icons/shareOp.svelte';
	import Copy from '$lib/share/shareButtons/copy.svelte';
	import Whatsapp from '$lib/share/shareButtons/WhatsApp.svelte';
	import Qr from '$lib/share/shareButtons/QR.svelte';
	import { fly } from 'svelte/transition';
	import Close from '$lib/icons/close.svelte';

	let clicked = $state(false);

	/**
	 * @typedef {Object} Props
	 * @property {string} [slug]
	 * @property {string} [title]
	 * @property {string} [desc]
	 * @property {string[]} [hashtags]
	 * @property {string} [quote]
	 * @property {string[]} [related]
	 * @property {string} [via]
	 * @property {string} [body]
	 * @property {'en' | 'he'} [lang]
	 * @property {string} siteTitle
	 * @property {string} siteUrl
	 * @property {number} [size]
	 * @property {number} [qrImageSize]
	 * @property {(blockUrl: string, imageSize?: number) => string} [buildQrUrl]
	 */

	/** @type {Props} */
	let {
		lang = 'en',
		slug = '',
		title = '',
		desc = '',
		hashtags = [],
		quote = undefined,
		related = [],
		via = '',
		body = '',
		siteTitle,
		siteUrl,
		size = 24,
		qrImageSize = 320,
		buildQrUrl = undefined
	} = $props();

	const url = `${siteUrl}/${slug}`;

	$effect(() => {
		body = `${desc} to see this click on ${url}`;
	});

	const handleWebShare = async () => {
		try {
			navigator.share({
				title,
				text: `Shared from ${siteTitle}`,
				url
			});
		} catch (error) {
			// Ignore cancellation errors from native share sheets.
		}
	};

	let webShareAPISupported = $derived(browser && typeof navigator.share !== 'undefined');
</script>

<aside class="container">
	<div class="wrapper">
		<div class="buttons">
			<button onclick={() => (clicked = !clicked)} style="width: {size}px; height: {size}px;">
				{#if clicked == false}
					<span class="sr-only">Open Share menu</span>
					<ShareIcon colour="#FF0092" width={size} height={size} />
				{:else}
					<span class="sr-only">Close Share menu</span>
					<span class="text">
						<Close height={size} width={size} />
					</span>
				{/if}
			</button>

			{#if clicked == true}
				<div class="share-dropdown" transition:fly|local={{ y: -20, duration: 500 }}>
					{#if webShareAPISupported}
						<button onclick={handleWebShare}>
							<span class="sr-only">Share</span>
							<ShareOp width={size} height={size} />
						</button>
					{/if}

					<Twitter {via} {size} {hashtags} {related} {quote} {url} {title} />
					<Facebook {url} {size} {hashtags} {quote} />
					<Whatsapp {url} {size} {title} />
					<Telegram {url} {size} {title} />
					<Mail {title} {body} {size} />
					<Copy {url} {lang} {size} />
					<Qr {url} {lang} {size} {qrImageSize} {buildQrUrl} />
				</div>
			{/if}
		</div>
	</div>
</aside>

<style>
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

	.text {
		color: gray;
	}

	.text:hover {
		color: #ff0092;
	}

	.container {
		display: flex;
		flex-direction: row;
		margin-top: 12px;
	}

	.wrapper {
		display: flex;
		flex-direction: row;
		margin-left: auto;
		font-weight: bold;
		font-size: 24px;
	}

	.buttons {
		margin-left: 4px;
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.share-dropdown {
		position: absolute;
		top: 100%;
		right: 0;
		z-index: 9999;
		display: flex;
		flex-direction: column;
		gap: 8px;
		background: white;
		padding: 8px;
		border-radius: 8px;
		box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
		min-width: max-content;
	}

	button {
		background: transparent;
		border-style: none;
		border: 0;
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
</style>
