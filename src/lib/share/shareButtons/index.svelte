<script>
	import { browser } from '$app/environment';
    import  Mail from '$lib/share/shareButtons/Mail.svelte'
	import ShareIcon from '$lib/icons/share.svelte';
	import Facebook from '$lib/share/shareButtons/Facebook.svelte';
	import Telegram from '$lib/share/shareButtons/Telegram.svelte';
	import Twitter from '$lib/share/shareButtons/Twitter.svelte';
    import ShareOp from '$lib/icons/shareOp.svelte';
	import Copy from '$lib/share/shareButtons/copy.svelte'
    import Whatsapp from '$lib/share/shareButtons/WhatsApp.svelte'
  import { fly } from 'svelte/transition';
  import Close from '$lib/icons/close.svelte'
    let clicked = $state(false)


	/**
	 * @typedef {Object} Props
	 * @property {string} [slug]
	 * @property {string} [title]
	 * @property {string} [desc]
	 * @property {any} [hashtags] - array of hashtags exclude '#' e.g. ['svelte', 'askRodney']
	 * @property {any} [quote]
	 * @property {any} [related] - array of Twitter users (including '@')
	 * @property {string} [via] - include '@' e.g. '@askRodney'
	 * @property {any} [body]
	 * @property {string} [lang] - language code e.g. 'en', 'he'
	 * @property {string} siteTitle
	 * @property {string} siteUrl
	 */

	/** @type {Props} */
	let {
		lang = "en",
		slug = "",
		title = "",
		desc = "",
		hashtags = [],
		quote = undefined,
		related = [],
		via = '',
		body = "",
		siteTitle,
		siteUrl
	} = $props();
    const url = `${siteUrl}/${slug}`;
	$effect(() => {
		body = desc + " to see this click on " + url
	})


	const handleWebShare = async () => {
		try {
			navigator.share({
				title,
				text: `Shared from ${siteTitle}`,
				url,
			});
		} catch (error) {
			webShareAPISupported = false;
		}
	};

	let webShareAPISupported = $derived(browser && typeof navigator.share !== 'undefined');
</script>

<aside class="container">
	<div class="wrapper">
		 <div class="buttons">
            <button onclick={()=>clicked = !clicked}>
                {#if clicked == false}
            <span class="sr-only">Open Share manu</span>
            <ShareIcon colour="#FF0092" width={48} />
            {:else}
            <span class="sr-only">Open Share manu</span>
            <span class="text">
            <Close height={48} width={48} />
            </span>
            {/if}
         </button>
            {#if clicked == true}
            <span transition:fly|local={{y:-150, duration: 2000}}>
			{#if webShareAPISupported}
				<button onclick={handleWebShare}
					><span class="sr-only">Share</span><ShareOp width={48} /></button
				>
			{/if}
				<Twitter {via} {hashtags} {related} {quote} {url} {title} />
                <Facebook {url} {hashtags} {quote}/>
                <Whatsapp {url} {title} />
				<Telegram {url} {title} /> 
                <Mail  {title} {body} />
				<Copy {url} {lang}/>
            </span>
            {/if}
               
		</div>
	</div>
</aside>

<style >
	.text {
		color: gray;
	}
	.text:hover {
		color:#FF0092;
	}
	.container {
		display: flex;
		flex-direction: row;
		margin-top: 12px;
		width: 48px;
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
	}

	button {
		background: transparent;
		border-style: none;
        border: 0px;
		transition: all 0.2s ease-in-out;
	}

		button {
			transition: all 2s ease-in-out;
		}
	button:focus,
	button:hover {
		transform: scale(1.1);
	}
</style>
