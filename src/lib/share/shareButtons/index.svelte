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
	import { fly } from 'svelte/transition';
	import Close from '$lib/icons/close.svelte';

	let clicked = $state(false);

	/**
	 * @typedef {Object} Props
	 * @property {string} [slug]
	 * @property {string} [title]
	 * @property {string} [desc]
	 * @property {any} [hashtags]
	 * @property {any} [quote]
	 * @property {any} [related]
	 * @property {string} [via]
	 * @property {any} [body]
	 * @property {string} [lang]
	 * @property {string} siteTitle
	 * @property {string} siteUrl
	 * @property {number} [size] - גודל האייקון בפיקסלים (ברירת מחדל 24)
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
		size = 24 // שינוי 1: הוספת פרופ לגודל עם דיפולט קטן ב-50%
	} = $props();

	const url = `${siteUrl}/${slug}`;
	$effect(() => {
		body = desc + ' to see this click on ' + url;
	});

	const handleWebShare = async () => {
		try {
			navigator.share({
				title,
				text: `Shared from ${siteTitle}`,
				url
			});
		} catch (error) {
			// webShareAPISupported = false; // שים לב: זה משתנה derived, לא ניתן להשמה ישירה, אבל לוגיקה תקינה
		}
	};

	let webShareAPISupported = $derived(browser && typeof navigator.share !== 'undefined');
</script>

<aside class="container">
	<div class="wrapper">
		<!-- הוספתי position: relative לכאן ב-CSS -->
		<div class="buttons">
			<button onclick={() => (clicked = !clicked)} style="width: {size}px; height: {size}px;">
				{#if clicked == false}
					<span class="sr-only">Open Share menu</span>
					<!-- שימוש ב-size שהוגדר -->
					<ShareIcon colour="#FF0092" width={size} height={size} />
				{:else}
					<span class="sr-only">Close Share menu</span>
					<span class="text">
						<Close height={size} width={size} />
					</span>
				{/if}
			</button>

			{#if clicked == true}
				<!-- שינוי 2: עטיפה ב-div ושימוש ב-absolute כדי לא לדחוף תוכן -->
				<div 
                    class="share-dropdown" 
                    transition:fly|local={{ y: -20, duration: 500 }}
                >
					{#if webShareAPISupported}
						<button onclick={handleWebShare}>
                            <span class="sr-only">Share</span>
                            <ShareOp width={size} height={size} />
                        </button>
					{/if}
					
                    <!-- כאן ייתכן שתצטרך להעביר את ה-size גם לקומפוננטות האחרות אם הן תומכות בזה -->
						<Twitter {via} {size} {hashtags} {related} {quote} {url} {title} />
						<Facebook {url} {size}  {hashtags} {quote} />
						<Whatsapp {url} {size}  {title} />
						<Telegram {url} {size}  {title} />
						<Mail {title} {body} {size} />
						<Copy {url} {lang} {size}  />
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
		/* width: 48px;  מחקתי את הרוחב הקבוע כדי לא להגביל את הקונטיינר */
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
        position: relative; /* קריטי עבור המיקום של התפריט הצף */
        display: flex;
        align-items: center;
        justify-content: center;
	}

    /* המחלקה החדשה עבור התפריט הצף */
    .share-dropdown {
        position: absolute;
        top: 100%; /* ממקם את זה ישירות מתחת לכפתור */
        right: 0; /* יישור לימין (או left: 0 לשמאל) */
        z-index: 100; /* מוודא שזה מעל אלמנטים אחרים */
        
        display: flex;
        flex-direction: column; /* או row אם אתה רוצה שורה */
        gap: 8px; /* רווח בין האייקונים */
        background: white; /* רצוי לתת רקע כדי שהאייקונים יהיו ברורים */
        padding: 8px;
        border-radius: 8px;
        box-shadow: 0 4px 6px rgba(0,0,0,0.1); /* צל עדין */
        min-width: max-content;
    }

	button {
		background: transparent;
		border-style: none;
		border: 0px;
		transition: all 0.2s ease-in-out;
        padding: 0; /* ביטול ריווח פנימי כדי שהגודל יהיה מדויק */
        display: flex; /* למרכז את האייקון בתוך הכפתור */
        align-items: center;
        justify-content: center;
        cursor: pointer;
	}

	button:focus,
	button:hover {
		transform: scale(1.1);
	}
</style>