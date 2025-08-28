<script>
  import Toast from '$lib/components/Toast.svelte';
  import { copyToClipboard } from '$lib/utilities/copy.js';
  import Copy from '$lib/icons/copy.svelte';
  
  const su = {
    "he": "!העתקת בהצלחה",
    "en": "you copied successfully"
  }
  
  const er = {
    "he": "שגיאה בהעתקה!",
    "en": "error while copying"
  }
  
  const coptTo = {
    "en": "Copy to clipboard",
    "he": "העתקה"
  }
  
  /**
   * @typedef {Object} Props
   * @property {string} [exampleText]
   * @property {any} url
   * @property {string} [lang]
   */
  
  /** @type {Props} */
  let { exampleText = 'Copy me!', url, lang = "en" } = $props();
  
  let showToast = $state(false);
  let toastMessage = $state('');
  let toastType = $state('success');
  let checked = $state(false);
  let error = $state(false);
  
  const handleSuccessfullyCopied = () => {
    checked = true;
    error = false;
    setTimeout(() => {
      checked = false;
    }, 30000);
  }
  
  const handleFailedCopy = () => {
    error = true;
    checked = false;
    showToast = true;
    toastMessage = er[lang];
    toastType = 'warning';
    
    setTimeout(() => {
      showToast = false;
    }, 3000);
  }
  
  const handleCopyClick = async () => {
    try {
      await copyToClipboard(url);
      handleSuccessfullyCopied();
    } catch (err) {
      console.error('Failed to copy:', err);
      handleFailedCopy();
    }
  }
</script>

<button onclick={handleCopyClick}>
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

  button:focus,
  button:hover {
    transform: scale(1.1);
  }
</style>