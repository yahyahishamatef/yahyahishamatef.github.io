<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minimalist Browser</title>
    <!-- Load Tailwind CSS for modern styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom scrollbar to make it look cleaner */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #1f2937; }
        ::-webkit-scrollbar-thumb { background: #374151; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #4b5563; }

        body {
            font-family: 'Inter', sans-serif;
            background-color: #111827; /* Dark background */
            color: #f3f4f6;
            height: 100vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        /* Iframe styling */
        #content-frame {
            border: 2px solid #374151;
            border-radius: 0.75rem;
            flex-grow: 1; /* Take up remaining space */
            margin: 1rem;
            background-color: white; /* Default background for loaded pages */
        }
    </style>
    <!-- Load Lucide icons for navigation buttons -->
    <script src="https://unpkg.com/lucide@latest"></script>
</head>
<body>

    <!-- Browser Control Bar -->
    <div id="browser-bar" class="flex items-center gap-2 p-4 bg-gray-800 shadow-xl">
        
        <!-- Navigation Buttons -->
        <button id="back-btn" title="Back" class="p-2 bg-gray-700 hover:bg-gray-600 rounded-lg transition disabled:opacity-50 disabled:cursor-not-allowed">
            <i data-lucide="arrow-left" class="w-5 h-5"></i>
        </button>
        <button id="forward-btn" title="Forward" class="p-2 bg-gray-700 hover:bg-gray-600 rounded-lg transition disabled:opacity-50 disabled:cursor-not-allowed">
            <i data-lucide="arrow-right" class="w-5 h-5"></i>
        </button>
        <button id="reload-btn" title="Reload" class="p-2 bg-gray-700 hover:bg-gray-600 rounded-lg transition">
            <i data-lucide="rotate-cw" class="w-5 h-5"></i>
        </button>

        <!-- Address Bar -->
        <input type="text" id="address-bar" placeholder="Enter URL (e.g., https://example.com)" 
               class="flex-grow p-2.5 bg-gray-700 text-white border border-gray-600 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 transition"
               value="https://www.w3.org">
        
        <!-- Go Button -->
        <button id="go-btn" title="Go" class="p-2.5 px-4 bg-blue-600 hover:bg-blue-700 text-white font-medium rounded-lg transition">
            Go
        </button>

        <!-- Loading Indicator -->
        <div id="loading-indicator" class="w-4 h-4 rounded-full bg-blue-400 animate-pulse hidden" title="Loading..."></div>
    </div>

    <!-- Content Frame -->
    <iframe id="content-frame" src="https://www.w3.org" sandbox="allow-scripts allow-forms allow-same-origin allow-popups allow-pointer-lock"></iframe>
    
    <!-- Status Message -->
    <div id="status-message" class="text-sm p-2 text-center bg-gray-900 text-yellow-400">
        Note: Many major websites (like Google, Wikipedia) block embedding due to security policies (CORS/CSP). You may see blank pages for those sites.
    </div>

    <script>
        // DOM Elements
        const addressBar = document.getElementById('address-bar');
        const contentFrame = document.getElementById('content-frame');
        const backBtn = document.getElementById('back-btn');
        const forwardBtn = document.getElementById('forward-btn');
        const reloadBtn = document.getElementById('reload-btn');
        const goBtn = document.getElementById('go-btn');
        const loadingIndicator = document.getElementById('loading-indicator');

        // State Management for History
        let history = [];
        let historyIndex = -1;

        // --- Utility Functions ---

        /**
         * Ensures the URL has a protocol (http/https).
         * @param {string} url 
         * @returns {string} Protocol-prefixed URL.
         */
        function normalizeUrl(url) {
            url = url.trim();
            if (!url.match(/^(?:http|https):\/\//i)) {
                return 'https://' + url;
            }
            return url;
        }

        /**
         * Updates the UI state of navigation buttons (disabled/enabled).
         */
        function updateNavigationButtons() {
            backBtn.disabled = historyIndex <= 0;
            forwardBtn.disabled = historyIndex >= history.length - 1;
        }

        /**
         * Shows or hides the loading spinner.
         * @param {boolean} isLoading 
         */
        function setLoading(isLoading) {
            loadingIndicator.classList.toggle('hidden', !isLoading);
            goBtn.disabled = isLoading;
            addressBar.disabled = isLoading;
        }

        // --- Core Navigation Functions ---

        /**
         * Loads a URL into the iframe and updates history.
         * @param {string} url The URL to load.
         * @param {boolean} isHistoryChange True if moving back/forward, false otherwise.
         */
        function navigate(url, isHistoryChange = false) {
            const normalizedUrl = normalizeUrl(url);

            setLoading(true);
            addressBar.value = normalizedUrl;
            contentFrame.src = normalizedUrl;

            if (!isHistoryChange) {
                // Clear any forward history
                history = history.slice(0, historyIndex + 1);
                history.push(normalizedUrl);
                historyIndex++;
            }
            
            updateNavigationButtons();
        }

        /**
         * Handles the 'Go' action from the button or Enter key.
         */
        function handleGo() {
            const url = addressBar.value;
            if (url) {
                navigate(url);
            }
        }

        // --- Event Handlers ---

        // 1. Address Bar (Enter Key)
        addressBar.addEventListener('keypress', (event) => {
            if (event.key === 'Enter') {
                event.preventDefault(); // Prevent default form submission behavior
                handleGo();
            }
        });

        // 2. Go Button
        goBtn.addEventListener('click', handleGo);

        // 3. Back Button
        backBtn.addEventListener('click', () => {
            if (historyIndex > 0) {
                historyIndex--;
                const url = history[historyIndex];
                addressBar.value = url;
                contentFrame.src = url;
                updateNavigationButtons();
            }
        });

        // 4. Forward Button
        forwardBtn.addEventListener('click', () => {
            if (historyIndex < history.length - 1) {
                historyIndex++;
                const url = history[historyIndex];
                addressBar.value = url;
                contentFrame.src = url;
                updateNavigationButtons();
            }
        });

        // 5. Reload Button
        reloadBtn.addEventListener('click', () => {
            contentFrame.contentWindow.location.reload();
        });

        // 6. Iframe Load Event (indicates loading finished)
        contentFrame.addEventListener('load', () => {
            setLoading(false);
            
            // Try to update address bar with final URL (may fail due to cross-origin policies)
            try {
                const currentUrl = contentFrame.contentWindow.location.href;
                if (currentUrl !== 'about:blank') {
                     addressBar.value = currentUrl;
                }
            } catch (e) {
                // Ignore security error from trying to access cross-origin URL
                console.warn('Could not read iframe location due to cross-origin security restriction.');
            }
            
            // Ensure the history is correct if the initial load was a redirect
            if (history[historyIndex] !== addressBar.value) {
                // This is a gentle way to fix history if the iframe redirects internally
                history[historyIndex] = addressBar.value;
            }
        });

        // Initialize Lucide icons
        lucide.createIcons();

        // Initial setup: Load the default URL and disable back/forward
        navigate(addressBar.value);
    </script>
</body>
</html>