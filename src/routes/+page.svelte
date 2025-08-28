<script>
    import { ShareButtons } from '$lib';

    const siteInfo = {
        siteTitle: "Svelte Share Demo",
        siteUrl: "https://example.com",
    };

    const shareConfig = {
        slug: "demo-page",
        title: "Check out this awesome Svelte Share component!",
        desc: "A versatile social sharing component for SvelteKit applications",
        hashtags: ["svelte", "sveltekit", "webdev", "opensource"],
        via: "@YourTwitterHandle",
        related: ["@sveltejs"],
        quote: "Make sharing easy in your SvelteKit apps with Svelte Share",
        lang: "en"
    };
</script>

<main class="container">
    <h1>Svelte Share Demo</h1>
    
    <section class="demo-section">
        <div class="content">
            <h2>Features</h2>
            <ul>
                <li>✨ Multiple social platforms support</li>
                <li>📱 Mobile-friendly design</li>
                <li>🌐 Web Share API support</li>
                <li>📋 Copy to clipboard functionality</li>
                <li>🌍 Internationalization support</li>
                <li>🎨 Customizable appearance</li>
            </ul>

            <div class="share-container">
                <h3>Try it out!</h3>
                <p>Click the share button below to see it in action:</p>
                <ShareButtons {...siteInfo} {...shareConfig} />
            </div>
        </div>
    </section>

    <section class="code-section">
        <h2>Implementation</h2>
        <p>Add the share buttons to your SvelteKit project in three simple steps:</p>
        
        <div class="code-steps">
            <div class="step">
                <h4>1. Install the package</h4>
                <pre><code>npm install @1lev1/svelte-share</code></pre>
            </div>

            <div class="step">
                <h4>2. Import the component</h4>
                <pre><code>{`import { ShareButtons } from '@1lev1/svelte-share';`}</code></pre>
            </div>

            <div class="step">
                <h4>3. Use in your component</h4>
                <pre><code>{`<script>
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

<ShareButtons {...siteInfo} {...shareConfig} />`}</code></pre>
            </div>
        </div>

        <div class="api-docs">
            <h3>Available Props</h3>
            <table>
                <thead>
                    <tr>
                        <th>Prop</th>
                        <th>Type</th>
                        <th>Description</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>siteTitle</td>
                        <td>string</td>
                        <td>Your website's name</td>
                    </tr>
                    <tr>
                        <td>siteUrl</td>
                        <td>string</td>
                        <td>Your website's URL</td>
                    </tr>
                    <tr>
                        <td>slug</td>
                        <td>string</td>
                        <td>Page path/slug</td>
                    </tr>
                    <tr>
                        <td>title</td>
                        <td>string</td>
                        <td>Share title</td>
                    </tr>
                    <tr>
                        <td>desc</td>
                        <td>string</td>
                        <td>Share description</td>
                    </tr>
                    <tr>
                        <td>hashtags</td>
                        <td>string[]</td>
                        <td>Array of hashtags (without #)</td>
                    </tr>
                    <tr>
                        <td>via</td>
                        <td>string</td>
                        <td>Twitter handle (with @)</td>
                    </tr>
                    <tr>
                        <td>lang</td>
                        <td>string</td>
                        <td>"en" or "he"</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </section>

    <section class="credits-section">
        <h2>Credits</h2>
        <p>This component is built with inspiration from and gratitude to:</p>
        <div class="credits-list">
            <div class="credit-item">
                <h4>svelte-share-buttons-component</h4>
                <p>Our component is inspired by and based on <a href="https://github.com/pchynoweth/svelte-share-buttons-component" target="_blank" rel="noopener noreferrer">svelte-share-buttons-component</a> by Paul Chynoweth.</p>
            </div>
            <div class="credit-item">
                <h4>svelte-copy</h4>
                <p>Copy to clipboard functionality is powered by <a href="https://github.com/ghostdevv/svelte-copy" target="_blank" rel="noopener noreferrer">svelte-copy</a>, a reliable and efficient clipboard management library.</p>
            </div>
        </div>
    </section>
 
</main>
   <footer class="footer">
	<a class="hover:decoration-1 hover:underline" href="https://www.1lev1.com/user/1" target="_blank" rel="noopener noreferrer">
		created by Baruch Babayof
	</a>
</footer>
<style>
    	.footer {
		text-align: center;
		padding: 1rem;
		background-color: #EEE8AA;
		border-top: 1px solid #e9ecef;
		color: #FF0092;
	}
	
    .container {
        max-width: 800px;
        margin: 0 auto;
        padding: 2rem;
    }

    h1 {
        font-size: 2.5rem;
        color: #FF0092;
        text-align: center;
        margin-bottom: 2rem;
    }

    .demo-section {
        background: #f8f9fa;
        border-radius: 12px;
        padding: 2rem;
        margin-top: 2rem;
    }

    .content {
        display: flex;
        flex-direction: column;
        gap: 1.5rem;
    }

    h2 {
        font-size: 1.8rem;
        color: #333;
        margin-bottom: 1rem;
    }

    ul {
        list-style: none;
        padding: 0;
    }

    li {
        margin-bottom: 0.8rem;
        font-size: 1.1rem;
    }

    .share-container {
        background: white;
        padding: 2rem;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        margin-top: 2rem;
    }

    h3 {
        font-size: 1.4rem;
        color: #444;
        margin-bottom: 1rem;
    }

    p {
        color: #666;
        margin-bottom: 1.5rem;
    }

    .code-section {
        margin-top: 3rem;
        padding: 2rem;
    }

    .code-steps {
        display: flex;
        flex-direction: column;
        gap: 2rem;
        margin: 2rem 0;
    }

    .step {
        background: #f8f9fa;
        border-radius: 8px;
        padding: 1.5rem;
    }

    .step h4 {
        color: #FF0092;
        margin-bottom: 1rem;
        font-size: 1.2rem;
    }

    pre {
        background: #2d2d2d;
        padding: 1.5rem;
        border-radius: 6px;
        overflow-x: auto;
    }

    code {
        color: #fff;
        font-family: 'Fira Code', monospace;
        font-size: 0.9rem;
        line-height: 1.5;
    }

    .api-docs {
        margin-top: 3rem;
    }

    table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 1rem;
    }

    th, td {
        padding: 0.75rem;
        text-align: left;
        border-bottom: 1px solid #ddd;
    }

    th {
        background: #f8f9fa;
        font-weight: 600;
    }

    td {
        color: #666;
    }

    .credits-section {
        margin-top: 3rem;
        padding: 2rem;
        background: #f8f9fa;
        border-radius: 12px;
    }

    .credits-list {
        display: flex;
        flex-direction: column;
        gap: 2rem;
        margin-top: 1.5rem;
    }

    .credit-item {
        background: white;
        padding: 1.5rem;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
    }

    .credit-item h4 {
        color: #FF0092;
        margin-bottom: 0.5rem;
        font-size: 1.2rem;
    }

    .credit-item p {
        margin: 0;
    }

    a {
        color: #FF0092;
        text-decoration: none;
        transition: color 0.2s ease;
    }

    a:hover {
        color: #d4007a;
        text-decoration: underline;
    }
</style>