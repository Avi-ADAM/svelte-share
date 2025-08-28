<script>
    import { copy } from "svelte-copy";
    import Toast from '$lib/components/Toast.svelte';

  const su = {
    "he": "!העתקת בהצלחה",
    "en": "you copied successfully"
  }
  const er = {
    "he": "שגיאה בהעתקה!",
    "en": "error while copying"
  }

  let showToast = $state(false);
  let toastMessage = $state('');
  let toastType = $state('success');
  const coptTo = {
    "en": "Copy to clipboard",
    "he": "העתקה"
  }
	import Copy from '$lib/icons/copy.svelte';
	
  /**
   * @typedef {Object} Props
   * @property {string} [exampleText]
   * @property {any} url
   */

  /** @type {Props} */
  let { exampleText = 'Copy me!', url, lang = "en" } = $props();
  let checked = $state(false);
  const handleSuccessfullyCopied = (e) => {
    console.log(e)
    checked = true
    showToast = true;
    toastMessage = su[lang];
    toastType = 'success';
    setTimeout(() => {
        checked = false;
        showToast = false;
    }, 3000)
  }

  const handleFailedCopy = () => {
    error = true;
    showToast = true;
    toastMessage = er[lang];
    toastType = 'warning';
    setTimeout(() => {
        checked = true;
        showToast = false;
    }, 3000)
  }
  let error = $state(false);
  
</script>

    <button  use:copy={{text:url, events: ['click'], 
    onCopy:handleSuccessfullyCopied, onError({error}){
        error = true;
        showToast = true;
        toastMessage = er[lang];
        toastType = 'warning';
        setTimeout(() => {
            checked = true;
            showToast = false;
        }, 3000)
    }}}  >
    {#if showToast}
        <Toast message={toastMessage} type={toastType} />
        {/if}
      <span class="sr-only">{coptTo[lang]}</span>
      <Copy
  		{checked}
          {error}
  		width={48}
  	/>
    </button>

    
<style>
	button {
    padding: 0;
		background: transparent;
		border-style: none;
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


