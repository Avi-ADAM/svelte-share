# Svelte Share

[![npm version](https://badge.fury.io/js/%401lev1%2Fsvelte-share.svg)](https://www.npmjs.com/package/@1lev1/svelte-share)

A versatile social sharing component for SvelteKit applications, with support for multiple platforms and languages. Based on and inspired by [svelte-share-buttons-component](https://github.com/pchynoweth/svelte-share-buttons-component)

## Features

- ✨ Multiple social platforms support
- 📱 Mobile-friendly design
- 🌐 Web Share API support
- 📋 Copy to clipboard functionality
- 🌍 Internationalization support (English and Hebrew)
- 🎨 Customizable appearance
## Demo 

check out the [demo](https://avi-adam.github.io/svelte-share/) 

## Installation

```bash
npm install @1lev1/svelte-share
```

## Usage

```svelte
<script>
    import { ShareButtons } from '@1lev1/svelte-share';

    const siteInfo = {
        siteTitle: "Your Site Name",
        siteUrl: "https://yoursite.com"
    };

    const shareConfig = {
        slug: "page-slug",
        title: "Your Page Title",
        desc: "Page description",
        hashtags: ["svelte", "webdev"],
        via: "@TwitterHandle",
        related: ["@sveltejs"],
        quote: "Share quote",
        lang: "en"  // or "he" for Hebrew
    };
</script>

<ShareButtons {...siteInfo} {...shareConfig} />
```

## Props

| Prop | Type | Description |
|------|------|-------------|
| siteTitle | string | Your website's name |
| siteUrl | string | Your website's URL |
| slug | string | Page path/slug |
| title | string | Share title |
| desc | string | Share description |
| hashtags | string[] | Array of hashtags (without #) |
| via | string | Twitter handle (with @) |
| lang | string | "en" or "he" |
| quote | string | Share quote (optional) |
| related | string[] | Related Twitter handles (optional) |
| qrImageSize | number | QR image size in pixels (default: 320) |
| buildQrUrl | (blockUrl: string, imageSize?: number) => string | Optional QR URL builder override |

## Supported Platforms

- Web Share API (when available)
- Twitter
- Facebook
- WhatsApp
- Telegram
- Email
- Copy to clipboard
- QR code (full-screen modal)


### QR customization (optional)

```svelte
<script>
    import { ShareButtons } from '@1lev1/svelte-share';

    function buildQrUrl(blockUrl, imageSize = 320) {
        return `https://api.qrserver.com/v1/create-qr-code/?size=${imageSize}x${imageSize}&data=${encodeURIComponent(blockUrl)}`;
    }
</script>

<ShareButtons
    siteTitle="Your Site Name"
    siteUrl="https://yoursite.com"
    slug="page-slug"
    buildQrUrl={buildQrUrl}
    qrImageSize={320}
/>
```
## Credits

This component is based on [svelte-share-buttons-component](https://github.com/pchynoweth/svelte-share-buttons-component) by Paul Chynoweth, with additional features and modifications.

## License

1lev1





