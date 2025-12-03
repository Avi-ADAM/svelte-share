<script>
	import EmailIcon from '$lib/icons/Email.svelte';

	let { title, body, size = 48 } = $props();

	const baseUrl = 'mailto:';
	const parametersObject = {
		body: body,
		subject: title,
	};
	const params = Object.entries(parametersObject)
		.filter(([, value]) => value ?? false)
		.map(([key, value]) => `${encodeURIComponent(key)}=${encodeURIComponent(String(value))}`)
		.join('&');

	const urlWithParameters = params === '' ? baseUrl : `${baseUrl}?${params}`;

	
</script>

<a href="{urlWithParameters}"
	><span class="sr-only">Share on Email</span><EmailIcon
		width={size}
		height={size}
	/></a
>

<style>

	a {
		background: transparent;
		border-style: none;
		transition: all 0.2s ease-in-out;
		padding: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
	}

	a:focus,
	a:hover {
		transform: scale(1.1);
	}

</style>
