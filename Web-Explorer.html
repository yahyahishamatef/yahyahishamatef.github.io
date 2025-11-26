<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web Explorer</title>
    <!-- Load Tailwind CSS via CDN for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for the iframe container and loading state */
        body {
            font-family: 'Inter', sans-serif;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            background-color: #1a202c; /* Dark background */
        }
        #browser-frame {
            border: none;
            width: 100%;
            flex-grow: 1; /* Make iframe take up remaining space */
        }
        .browser-container {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            position: relative;
        }
        .loading-overlay {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(45, 55, 72, 0.9); /* Dark semi-transparent overlay */
            color: #fff;
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 10;
            font-size: 1.25rem;
            text-align: center;
            border-radius: 0.5rem;
            backdrop-filter: blur(2px);
        }
    </style>
</head>
<body>

    <div class="p-4 bg-gray-800 shadow-xl">
        <div class="max-w-7xl mx-auto">
            <h1 class="text-2xl font-bold text-white mb-4">Web Explorer</h1>
            <div class="flex flex-col sm:flex-row gap-2">
                <input
                    type="url"
                    id="url-input"
                    placeholder="Enter URL (e.g., example.com or https://example.com)"
                    value="https://www.wikipedia.org/"
                    class="flex-grow p-3 rounded-lg border-2 border-gray-600 focus:border-indigo-500 bg-gray-700 text-white placeholder-gray-400 shadow-inner transition duration-150 ease-in-out"
                    onkeydown="if(event.key === 'Enter') document.getElementById('go-button').click();"
                />
                <button
                    id="go-button"
                    onclick="loadUrl()"
                    class="px-6 py-3 bg-indigo-600 text-white font-semibold rounded-lg shadow-md hover:bg-indigo-700 transition duration-150 ease-in-out transform hover:scale-[1.01] active:scale-[0.99] border-b-4 border-indigo-800 active:border-b-0"
                >
                    Browse
                </button>
            </div>
            <div id="status-message" class="mt-2 text-sm text-yellow-400 hidden">
                Note: Some major websites cannot be loaded due to security restrictions (X-Frame-Options or CSP).
            </div>
        </div>
    </div>

    <div class="browser-container max-w-7xl mx-auto w-full p-4">
        <div id="loading-overlay" class="loading-overlay rounded-lg">
            <p>Loading... Please wait.</p>
        </div>
        <!-- The iframe is where the content is displayed -->
        <iframe id="browser-frame" src="https://www.wikipedia.org/" title="Web Content Frame" class="rounded-lg shadow-2xl bg-white"></iframe>
    </div>

    <script>
        // Global variables for elements
        const urlInput = document.getElementById('url-input');
        const browserFrame = document.getElementById('browser-frame');
        const loadingOverlay = document.getElementById('loading-overlay');
        const statusMessage = document.getElementById('status-message');

        /**
         * Validates and normalizes the URL input.
         * @param {string} url The raw URL string from the input.
         * @returns {string} The normalized URL, or null if invalid.
         */
        function normalizeUrl(url) {
            if (!url) return null;
            // Trim whitespace
            url = url.trim();

            // Check if it already has a protocol
            if (!url.match(/^(f|ht)tps?:\/\//i)) {
                // Prepend HTTPS if no protocol is found
                url = 'https://' + url;
            }

            // Simple validation to ensure it looks like a URL
            try {
                new URL(url);
                return url;
            } catch (e) {
                // If URL constructor fails, it's not a valid URL format
                return null;
            }
        }

        /**
         * Function to load the URL into the iframe.
         */
        function loadUrl() {
            const rawUrl = urlInput.value;
            const targetUrl = normalizeUrl(rawUrl);

            if (!targetUrl) {
                // Display a temporary error/info message instead of an alert
                statusMessage.textContent = 'Please enter a valid URL. Trying to access: ' + rawUrl;
                statusMessage.classList.remove('hidden');
                setTimeout(() => statusMessage.classList.add('hidden'), 5000);
                return;
            }

            // Show loading indicator
            loadingOverlay.style.display = 'flex';
            statusMessage.classList.remove('hidden');

            // Update the iframe source
            browserFrame.src = targetUrl;
        }

        /**
         * Event listener to hide the loading overlay once the iframe content loads (or fails to load).
         */
        browserFrame.onload = function() {
            loadingOverlay.style.display = 'none';
            // Show the standard security note after successful or failed load
            statusMessage.textContent = 'Note: Some major websites cannot be loaded due to security restrictions (X-Frame-Options or CSP).';
            statusMessage.classList.remove('hidden');
        };

        // Initial load status setup (for the default value)
        document.addEventListener('DOMContentLoaded', () => {
            loadUrl();
        });

    </script>
</body>
</html>