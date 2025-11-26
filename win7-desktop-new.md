<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Windows 7 Desktop Simulation</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Segoe+UI:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        /* General Reset and Font */
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden; 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            user-select: none;
            cursor: default; 
        }

        /* Windows 7 Desktop Background (Simulated) */
        #desktop-bg {
            height: calc(100vh - 40px);
            background: #004d6d url('https://placehold.co/1920x1080/004d6d/FFFFFF/png?text=W%20I%20N%20D%20O%20W%20S%20%207') no-repeat center center / cover;
            position: relative;
            overflow: hidden;
            z-index: 0;
        }

        /* Windows 7 Taskbar */
        #taskbar {
            height: 40px;
            /* Aero Glass effect simulation */
            background-color: rgba(0, 0, 0, 0.4);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-top: 1px solid rgba(255, 255, 255, 0.2);
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            display: flex;
            align-items: center;
            padding: 0 4px;
            z-index: 1000;
        }

        /* Start Button */
        #start-button {
            width: 70px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 15px;
            margin-right: 8px;
            cursor: pointer;
            transition: all 0.1s ease;
            font-size: 14px;
            font-weight: 600;
            color: white;
            background: radial-gradient(circle at 10px 10px, #2087d8, #005fbc);
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
            text-shadow: 1px 1px 1px rgba(0, 0, 0, 0.4);
        }
        #start-button:hover {
            background: radial-gradient(circle at 10px 10px, #399bf6, #1271d4);
            box-shadow: 0 1px 5px rgba(0, 0, 0, 0.7);
        }
        #start-button.active {
            background: radial-gradient(circle at 10px 10px, #005fbc, #2087d8);
            box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.5);
        }


        /* System Tray (Clock and Icons) */
        #system-tray {
            margin-left: auto;
            display: flex;
            align-items: center;
            height: 100%;
            padding: 0 6px;
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 2px;
            font-size: 13px;
            color: #ccc;
        }
        #system-tray > div {
            padding: 0 4px;
            cursor: pointer;
            transition: background 0.1s;
            margin: 0 2px;
        }
        #system-tray > div:hover {
            background-color: rgba(255, 255, 255, 0.2);
        }
        
        /* Desktop Icon Styling */
        .desktop-icon {
            width: 80px;
            height: 80px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 5px;
            margin: 10px;
            color: white;
            font-size: 12px;
            text-align: center;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.7);
            cursor: pointer;
            border: 1px solid transparent;
            border-radius: 4px;
        }
        .desktop-icon:hover {
            background-color: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        .desktop-icon-img {
            width: 40px;
            height: 40px;
            margin-bottom: 4px;
            filter: drop-shadow(2px 2px 2px rgba(0, 0, 0, 0.5));
        }

        /* --- Window Styles (Aero) --- */
        .win7-window {
            position: absolute;
            width: 700px;
            height: 500px;
            max-width: 90vw;
            max-height: calc(100vh - 80px);
            background: rgba(255, 255, 255, 0.9);
            border: 1px solid rgba(0, 0, 0, 0.2);
            border-radius: 8px;
            box-shadow: 0 8px 30px rgba(0, 0, 0, 0.4), 0 0 1px rgba(0, 0, 0, 0.6);
            overflow: hidden;
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            z-index: 10;
            display: none; 
            transition: all 0.2s ease-in-out; 
            min-width: 300px; /* Minimum size for resizing */
            min-height: 200px;
        }

        .win7-window.maximized {
            width: 100vw !important;
            height: calc(100vh - 40px) !important; 
            left: 0 !important;
            top: 0 !important;
            transform: none !important;
            border-radius: 0;
            border: none;
            box-shadow: none;
        }

        .win7-titlebar {
            padding: 8px 12px;
            background: linear-gradient(to bottom, #e0efff 0%, #c4d7fa 100%);
            border-bottom: 1px solid #a4bdfa;
            color: #1f3a9a;
            font-weight: 600;
            display: flex;
            justify-content: space-between;
            align-items: center;
            cursor: grab;
        }
        
        .win7-controls {
            display: flex;
            gap: 1px;
            height: 20px;
        }
        .win7-control-btn {
            width: 30px;
            height: 20px;
            line-height: 20px;
            text-align: center;
            font-size: 14px;
            cursor: pointer;
            transition: background 0.1s;
            border: none;
            background: transparent;
            color: #1f3a9a;
            margin: 0;
            padding: 0;
            outline: none;
        }
        .win7-control-btn:hover {
            background: rgba(255, 255, 255, 0.6);
        }
        .win7-control-btn.close-btn:hover {
            background: #e81123;
            color: white;
        }

        /* Browser Specific Styles */
        #browser-toolbar {
            display: flex;
            align-items: center;
            padding: 8px 10px;
            background-color: #f7f7f7;
            border-bottom: 1px solid #ddd;
        }
        /* Explorer App Specific Styles */
        .explorer-toolbar {
            display: flex;
            align-items: center;
            padding: 6px 10px;
            background: linear-gradient(to bottom, #f9fbfc 0%, #e8f0f6 100%);
            border-bottom: 1px solid #d0d7e0;
            font-size: 13px;
        }
        .nav-btn {
            background: linear-gradient(to bottom, #f2f2f2 0%, #e0e0e0 100%);
            border: 1px solid #aaa;
            border-radius: 3px;
            padding: 4px 8px;
            margin-right: 5px;
            font-size: 14px;
            cursor: pointer;
            color: #333;
            transition: all 0.1s;
            box-shadow: 0 1px 1px rgba(0,0,0,0.1);
        }
        .nav-btn:hover {
            background: linear-gradient(to bottom, #fff 0%, #f0f0f0 100%);
        }
        #url-input, #explorer-address-bar {
            flex-grow: 1;
            padding: 4px 8px;
            border: 1px solid #ccc;
            border-radius: 3px;
            margin-right: 10px;
            font-size: 14px;
            box-shadow: inset 0 1px 2px rgba(0,0,0,0.1);
            background-color: white;
        }
        #browser-frame {
            width: 100%;
            height: calc(100% - 40px); 
            border: none;
            background-color: white;
        }

        /* Explorer Content Area */
        .explorer-content {
            display: flex;
            height: calc(100% - 40px); /* Minus toolbar height */
        }
        .nav-pane {
            width: 200px;
            background-color: #f0f6fa;
            border-right: 1px solid #e0e0e0;
            padding: 10px 0;
            overflow-y: auto;
        }
        .file-view {
            flex-grow: 1;
            background-color: white;
            padding: 10px;
            display: flex;
            flex-wrap: wrap;
            align-content: flex-start; /* Important for grid layout */
            overflow-y: auto;
        }
        .nav-item {
            padding: 4px 10px;
            cursor: pointer;
            font-size: 13px;
            color: #333;
        }
        .nav-item:hover {
            background-color: #dbe8f3;
        }
        .nav-item.selected {
            background-color: #c9e0f3;
            font-weight: 600;
            border: 1px solid #aad0f5;
        }
        
        /* File/Folder Icon Styles */
        .file-icon {
            width: 80px;
            height: 80px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 5px;
            margin: 5px;
            font-size: 12px;
            text-align: center;
            cursor: pointer;
            border: 1px solid transparent;
            border-radius: 4px;
            transition: background 0.1s;
        }
        .file-icon:hover {
            background-color: #e0eaf3;
            border: 1px solid #c9e0f3;
        }
        .file-icon-img {
            width: 32px;
            height: 32px;
            margin-bottom: 4px;
        }
        .folder-icon {
            color: #ffb74d; /* Orange folder color */
        }
        .disk-icon {
            color: #007bff; /* Blue disk color */
        }

        /* Notepad Specific Styles */
        .notepad-content {
            padding: 0;
            height: calc(100% - 30px); /* Minus Menu Bar Height */
        }
        .notepad-menu-bar {
            height: 30px;
            padding: 5px 8px;
            border-bottom: 1px solid #ccc;
            background-color: #f0f0f0;
            font-size: 13px;
        }
        #notepad-textarea {
            width: 100%;
            height: 100%;
            padding: 10px;
            border: none;
            resize: none;
            outline: none;
            font-family: 'Consolas', 'Courier New', monospace;
            font-size: 14px;
            line-height: 1.4;
        }


        /* --- Start Menu Styles --- */
        #start-menu {
            position: fixed;
            bottom: 40px; 
            left: 0;
            width: 360px;
            height: 500px;
            background-color: rgba(200, 220, 255, 0.8);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border-top-right-radius: 8px;
            border-top-left-radius: 8px;
            border: 1px solid rgba(0, 0, 0, 0.2);
            border-bottom: none;
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
            display: none; 
            z-index: 999;
            color: #333;
            font-size: 14px;
            overflow: hidden;
            display: flex;
        }
        /* Left Column (Pinned/Recent Programs) */
        #start-menu-left {
            width: 60%;
            background-color: rgba(255, 255, 255, 0.7);
            border-right: 1px solid rgba(0, 0, 0, 0.1);
            display: flex;
            flex-direction: column;
            padding: 5px 0;
        }
        .start-menu-item {
            padding: 4px 10px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: background 0.1s;
        }
        .start-menu-item:hover {
            background: #aad0f5; /* Hover blue */
        }
        /* Right Column (User Folders/Settings) */
        #start-menu-right {
            width: 40%;
            padding: 5px 0;
            background: linear-gradient(to right, #e0efff, #c4d7fa); 
            border-top-right-radius: 8px;
            display: flex;
            flex-direction: column;
        }
        .start-menu-profile {
            padding: 10px;
            border-bottom: 1px solid #a4bdfa;
            font-weight: 600;
            color: #1f3a9a;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        #start-menu-footer {
            margin-top: auto;
            padding: 8px 10px;
            background-color: rgba(0, 0, 0, 0.1);
            border-top: 1px solid rgba(0, 0, 0, 0.1);
            display: flex;
            justify-content: flex-end;
        }
        #shutdown-btn {
            background: #c34c4c;
            color: white;
            padding: 4px 10px;
            border-radius: 3px;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.1s;
        }
        #shutdown-btn:hover {
            background: #e81123;
        }
        
        /* --- Shutdown/Startup Screen Styles --- */
        #shutdown-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background-color: black;
            color: white;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            z-index: 2000; 
        }
        #startup-button {
            padding: 10px 20px;
            font-size: 18px;
            font-weight: bold;
            border-radius: 5px;
            cursor: pointer;
            background: linear-gradient(to bottom, #2087d8, #005fbc);
            border: 2px solid #005fbc;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.5);
            transition: all 0.1s;
        }
        #startup-button:hover {
            background: linear-gradient(to bottom, #399bf6, #1271d4);
        }
        
    </style>
</head>
<body>

    <!-- 1. The main desktop background and icon area -->
    <div id="desktop-bg" style="display: none;">
        
        <!-- Desktop Icons -->
        <div class="desktop-icon" style="position: absolute; top: 20px; left: 20px;" id="computer-icon">
            <!-- Computer Icon (SVG) -->
            <svg class="desktop-icon-img disk-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" stroke="white" stroke-linecap="round" stroke-linejoin="round">
                <rect x="4" y="3" width="16" height="12" rx="1" ry="1" fill="#FFFFFF" stroke-width="1.5"/>
                <path d="M3 15h18" stroke="#34495e" stroke-width="1.5"/>
                <path d="M12 17v4" stroke="#FFFFFF" stroke-width="1.5"/>
                <path d="M8 21h8" stroke="#FFFFFF" stroke-width="1.5"/>
            </svg>
            <span>Computer</span>
        </div>
        
        <!-- Internet Explorer Icon -->
        <div class="desktop-icon" style="position: absolute; top: 120px; left: 20px;" id="ie-icon">
            <!-- IE Icon (SVG) -->
            <svg class="desktop-icon-img" viewBox="0 0 24 24" fill="#0072C6" xmlns="http://www.w3.org/2000/svg">
                <circle cx="12" cy="12" r="10" stroke="#005A9C" stroke-width="2" fill="#0072C6"/>
                <path d="M10 8C10 6.89543 10.8954 6 12 6C13.1046 6 14 6.89543 14 8V12H10V8Z" fill="#FFFFFF"/>
                <path d="M12 12C14.2091 12 16 10.2091 16 8V7C16 5.34315 14.6569 4 13 4" stroke="#FFFFFF" stroke-width="2" stroke-linecap="round"/>
                <path d="M12 16C9.79086 16 8 17.7909 8 20V21C8 22.6569 9.34315 24 11 24" stroke="#FFFFFF" stroke-width="2" stroke-linecap="round"/>
                <path d="M12 14V20" stroke="#0072C6" stroke-width="2" stroke-linecap="round"/>
                <path d="M14 14C14 15.1046 13.1046 16 12 16C10.8954 16 10 15.1046 10 14V10H14V14Z" fill="#FFFFFF"/>
                <path d="M12 12L14 10" stroke="#005A9C" stroke-width="2" stroke-linecap="round"/>
            </svg>
            <span>Internet Explorer 9</span>
        </div>

        <!-- 2. Application Window: Internet Explorer 9 -->
        <div class="win7-window" id="ie-window" style="z-index: 10;">
            
            <div class="win7-titlebar" id="ie-titlebar">
                <span id="ie-title">Internet Explorer 9 - Wikipedia - Windows 7</span>
                <div class="win7-controls">
                    <button class="win7-control-btn min-btn" onclick="toggleWindow('ie-window')">—</button>
                    <button class="win7-control-btn max-btn" id="ie-max-btn" onclick="toggleMaximize('ie-window', 'ie-max-btn')">□</button>
                    <button class="win7-control-btn close-btn" onclick="toggleWindow('ie-window')">×</button>
                </div>
            </div>

            <div id="browser-toolbar">
                <button class="nav-btn" onclick="navigateTo('back')">← Back</button>
                <button class="nav-btn" onclick="navigateTo('forward')">→ Forward</button>
                <input type="text" id="url-input" placeholder="https://www.google.com" value="https://wikipedia.org/wiki/Windows_7">
                <button class="nav-btn" onclick="navigateTo('go')">Go</button>
            </div>

            <iframe id="browser-frame" 
                    src="https://wikipedia.org/wiki/Windows_7" 
                    frameborder="0">
            </iframe>
        </div>
        
        <!-- 2b. Application Window: File Explorer (Computer) -->
        <div class="win7-window" id="explorer-window" style="z-index: 20; width: 800px; height: 600px; display: none;">
            
            <div class="win7-titlebar" id="explorer-titlebar">
                <span id="explorer-title">Computer</span>
                <div class="win7-controls">
                    <button class="win7-control-btn min-btn" onclick="toggleWindow('explorer-window')">—</button>
                    <button class="win7-control-btn max-btn" id="explorer-max-btn" onclick="toggleMaximize('explorer-window', 'explorer-max-btn')">□</button>
                    <button class="win7-control-btn close-btn" onclick="toggleWindow('explorer-window')">×</button>
                </div>
            </div>
            
            <!-- Explorer Toolbar -->
            <div class="explorer-toolbar">
                <button class="nav-btn" onclick="navigateExplorer('back')">←</button>
                <button class="nav-btn" onclick="navigateExplorer('forward')">→</button>
                <input type="text" id="explorer-address-bar" readonly style="cursor: default; width: 350px;" value="Computer">
                <div class="ml-auto flex items-center gap-2 text-gray-700">
                    <button class="nav-btn text-xs">Organize</button>
                    <button class="nav-btn text-xs">New Folder</button>
                </div>
            </div>

            <!-- Content Area: Navigation Pane (Left) and File View (Right) -->
            <div class="explorer-content">
                <div class="nav-pane" id="nav-pane">
                    <div class="nav-item selected" data-path="Computer" onclick="loadFolder('Computer', event)">
                        <span class="disk-icon mr-2">💻</span> Computer
                    </div>
                    <div class="nav-item" data-path="C:/" onclick="loadFolder('C:/', event)">
                        <span class="disk-icon mr-2">🖴</span> Local Disk (C:)
                    </div>
                    <div class="nav-item" data-path="Documents" onclick="loadFolder('Documents', event)">
                        <span class="folder-icon mr-2">📄</span> Documents
                    </div>
                    <div class="nav-item" data-path="Desktop" onclick="loadFolder('Desktop', event)">
                        <span class="folder-icon mr-2">🖼️</span> Desktop
                    </div>
                    <div class="nav-item" data-path="Network" onclick="window.alert('Network location is unavailable.')">
                        <span class="disk-icon mr-2">🌐</span> Network
                    </div>
                </div>
                <div class="file-view" id="file-view">
                    <!-- File and folder icons will be rendered here -->
                </div>
            </div>
        </div>

        <!-- 2c. Application Window: Notepad -->
        <div class="win7-window" id="notepad-window" style="z-index: 30; width: 600px; height: 400px; display: none;">
            
            <div class="win7-titlebar" id="notepad-titlebar">
                <span id="notepad-title">Untitled - Notepad</span>
                <div class="win7-controls">
                    <button class="win7-control-btn min-btn" onclick="toggleWindow('notepad-window')">—</button>
                    <button class="win7-control-btn max-btn" id="notepad-max-btn" onclick="toggleMaximize('notepad-window', 'notepad-max-btn')">□</button>
                    <button class="win7-control-btn close-btn" onclick="toggleWindow('notepad-window')">×</button>
                </div>
            </div>
            
            <div class="notepad-menu-bar">
                <span class="mr-3 cursor-pointer hover:underline">File</span>
                <span class="mr-3 cursor-pointer hover:underline">Edit</span>
                <span class="mr-3 cursor-pointer hover:underline">Format</span>
                <span class="mr-3 cursor-pointer hover:underline">View</span>
                <span class="mr-3 cursor-pointer hover:underline">Help</span>
            </div>

            <div class="notepad-content">
                <textarea id="notepad-textarea"></textarea>
            </div>
        </div>


        <!-- 4. The Start Menu -->
        <div id="start-menu">
            <div id="start-menu-left">
                <!-- User Profile and Search Bar placeholder -->
                <div class="start-menu-profile">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <circle cx="12" cy="6" r="4" fill="#6699ff"/>
                        <path d="M20 20.25c0-3.3-2.7-6-6-6h-4c-3.3 0-6 2.7-6 6v.75h16v-.75z" fill="#6699ff"/>
                    </svg>
                    <span>User Profile</span>
                </div>

                <!-- Search Box -->
                <div style="padding: 5px 10px 10px;">
                    <input type="text" placeholder="Search programs and files" style="width: 100%; padding: 4px; border: 1px solid #ccc; border-radius: 3px; box-shadow: inset 0 1px 2px rgba(0,0,0,0.1);">
                </div>

                <!-- Pinned/Recent Apps -->
                <div class="start-menu-item" onclick="toggleWindow('ie-window', event)">
                    <span class="text-xl">e</span>
                    <span>Internet Explorer 9</span>
                </div>
                <div class="start-menu-item" onclick="toggleWindow('explorer-window', event)">
                    <span class="text-xl">📁</span>
                    <span>Windows Explorer</span>
                </div>
                <div class="start-menu-item" onclick="toggleWindow('notepad-window', event)">
                    <span class="text-xl">📝</span>
                    <span>Notepad</span>
                </div>
                <div class="start-menu-item" onclick="window.alert('Calculator is not implemented yet.')">
                    <span class="text-xl">🧮</span>
                    <span>Calculator</span>
                </div>
                
                <div class="mt-auto border-t border-gray-300">
                    <div class="start-menu-item">
                        <span class="text-xl">📂</span>
                        <span>All Programs</span>
                    </div>
                </div>
            </div>
            
            <div id="start-menu-right">
                <!-- Common Folders -->
                <div class="start-menu-item" onclick="toggleWindow('explorer-window', event)">
                    <span class="text-xl">💻</span>
                    <span>Computer</span>
                </div>
                <div class="start-menu-item" onclick="loadFolder('Documents', event)">
                    <span class="text-xl">🖼️</span>
                    <span>Pictures</span>
                </div>
                <div class="start-menu-item" onclick="loadFolder('Documents', event)">
                    <span class="text-xl">📄</span>
                    <span>Documents</span>
                </div>
                <div class="start-menu-item" onclick="window.alert('Control Panel is not implemented yet.')">
                    <span class="text-xl">⚙️</span>
                    <span>Control Panel</span>
                </div>

                <!-- Shutdown Area -->
                <div id="start-menu-footer">
                    <div id="shutdown-btn" onclick="shutdown()">
                        Shutdown
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- 3. The Taskbar -->
    <div id="taskbar" style="display: none;">
        <div id="start-button" onclick="toggleStartMenu()">
            Start
        </div>
        
        <!-- Taskbar Icons -->
        <div class="h-8 p-1 mr-2 flex items-center bg-white/20 hover:bg-white/30 rounded-sm cursor-pointer" id="ie-taskbar-icon" onclick="toggleWindow('ie-window')">
            <span class="text-white text-xl">e</span>
        </div>
        <div class="h-8 p-1 mr-2 flex items-center bg-white/20 hover:bg-white/30 rounded-sm cursor-pointer" id="explorer-taskbar-icon" onclick="toggleWindow('explorer-window')">
            <span class="text-white text-xl">📁</span>
        </div>
        <div class="h-8 p-1 mr-2 flex items-center bg-white/20 hover:bg-white/30 rounded-sm cursor-pointer" id="notepad-taskbar-icon" onclick="toggleWindow('notepad-window')">
            <span class="text-white text-xl">📝</span>
        </div>


        <div id="system-tray">
            <!-- Speaker Icon -->
            <div>🔊</div>
            <!-- Network Icon -->
            <div>🌐</div>
            <!-- Clock -->
            <div id="clock">10:00 AM</div>
        </div>
    </div>
    
    <!-- 5. Shutdown/Startup Overlay -->
    <div id="shutdown-overlay" style="display: flex;">
        <h1 class="text-4xl font-light mb-8">Windows 7 Simulation</h1>
        <button id="startup-button" onclick="startup()">
            Start Up Windows 7
        </button>
    </div>

    <!-- Application JavaScript Logic -->
    <script>
        // --- CONSTANTS AND STATE ---
        const Z_INDEX_BASE = 10;
        let highestZIndex = Z_INDEX_BASE;
        
        // Virtual File System Structure
        const VFS = {
            'Computer': {
                type: 'system',
                contents: {
                    'Local Disk (C:)': { type: 'drive', path: 'C:/' },
                    'Documents': { type: 'folder', path: 'Documents' },
                    'Desktop': { type: 'folder', path: 'Desktop' },
                }
            },
            'C:/': {
                type: 'drive',
                contents: {
                    // Contents are empty objects but the path resolves correctly now
                    'Program Files (x86)': { type: 'folder', path: 'C:/Program Files (x86)', contents: {} },
                    'Windows': { type: 'folder', path: 'C:/Windows', contents: {} },
                    'Users': { type: 'folder', path: 'C:/Users', contents: {
                        'User': { type: 'folder', path: 'C:/Users/User', contents: {
                            'My Documents': { type: 'folder', path: 'Documents', contents: {} }
                        } }
                    }},
                    'BootLog.txt': { type: 'file', path: 'C:/BootLog.txt', content: "Boot successful. All drivers loaded.\nLast Shutdown: 2025-11-20 15:35:00" },
                    // SYSTEM FILE UNPROTECTED: Added content for previously restricted files
                    'Pagefile.sys': { type: 'file', path: 'C:/Pagefile.sys', content: "******************\n** WARNING: THIS IS A SYSTEM FILE **\n******************\n\nThis file is used by the operating system for virtual memory and should not be modified.\n\nSimulated File Size: 4,096 MB" },
                    'Boot.ini': { type: 'file', path: 'C:/Boot.ini', content: ";\n;Warning: Don't edit this file manually\n[boot loader]\ntimeout=30\ndefault=multi(0)disk(0)rdisk(0)partition(1)\\WINDOWS\n[operating systems]\nmulti(0)disk(0)rdisk(0)partition(1)\\WINDOWS=\"Microsoft Windows 7\" /noexecute=optin /fastdetect" }
                }
            },
            'Documents': {
                type: 'folder',
                contents: {
                    'Work': { type: 'folder', path: 'Documents/Work', contents: {} },
                    'Personal': { type: 'folder', path: 'Documents/Personal', contents: {} },
                    'README.txt': { type: 'file', path: 'Documents/README.txt', content: "Welcome to your virtual Documents folder!\n\nThis is a simple text file demonstrating the Notepad application." },
                    'Photo.jpg': { type: 'file', path: 'Documents/Photo.jpg' }
                }
            },
            'Desktop': {
                type: 'folder',
                contents: {
                    'Computer': { type: 'system', path: 'Computer' }, // Link to Computer view
                    'Internet Explorer 9': { type: 'system', path: 'ie-window' }, // Link to IE
                    'New Document.docx': { type: 'file', path: 'Desktop/New Document.docx', content: "This is a simulated document file. For simplicity, we'll open it in Notepad." }
                }
            }
        };

        // Explorer history for Back/Forward navigation
        let explorerHistory = ['Computer'];
        let explorerHistoryIndex = 0;
        let currentPath = 'Computer';
        
        // Global Bounds Storage for all windows
        const windowBounds = {
            'ie-window': {
                width: 700, height: 500, left: '50%', top: '50%', transform: 'translate(-50%, -50%)', isMaximized: false
            },
            'explorer-window': {
                width: 800, height: 600, left: '60%', top: '40%', transform: 'translate(-50%, -50%)', isMaximized: false
            },
            'notepad-window': {
                width: 600, height: 400, left: '45%', top: '30%', transform: 'translate(-50%, -50%)', isMaximized: false
            }
        };
        
        // --- DOM ELEMENTS ---
        const desktopBg = document.getElementById('desktop-bg');
        const taskbar = document.getElementById('taskbar');
        const shutdownOverlay = document.getElementById('shutdown-overlay');

        // IE Window elements
        const ieWindow = document.getElementById('ie-window');
        const ieTitle = document.getElementById('ie-title');
        const urlInput = document.getElementById('url-input');
        const browserFrame = document.getElementById('browser-frame');
        
        // Explorer Window elements
        const explorerWindow = document.getElementById('explorer-window');
        const explorerTitle = document.getElementById('explorer-title');
        const explorerAddressBar = document.getElementById('explorer-address-bar');
        const navPane = document.getElementById('nav-pane');
        const fileView = document.getElementById('file-view');

        // Notepad Window elements
        const notepadWindow = document.getElementById('notepad-window');
        const notepadTitle = document.getElementById('notepad-title');
        const notepadTextarea = document.getElementById('notepad-textarea');
        
        // Start Menu elements
        const startButton = document.getElementById('start-button');
        const startMenu = document.getElementById('start-menu');
        
        // Icons
        document.getElementById('computer-icon').addEventListener('click', () => toggleWindow('explorer-window'));
        document.getElementById('ie-icon').addEventListener('click', () => toggleWindow('ie-window'));

        // --- Utility: Custom Alert ---
        window.alert = function(message) {
            console.log("App Alert:", message);
            const alertBox = document.createElement('div');
            alertBox.textContent = "❗ " + message;
            alertBox.style.cssText = 'position: fixed; bottom: 50px; right: 10px; background: #fff; color: #333; padding: 10px; border-radius: 5px; box-shadow: 0 4px 8px rgba(0,0,0,0.2); z-index: 10001; animation: fadeout 3s forwards;';
            document.body.appendChild(alertBox);

            const style = document.createElement('style');
            style.textContent = `@keyframes fadeout { 0% { opacity: 1; } 80% { opacity: 1; } 100% { opacity: 0; display: none; } }`;
            document.head.appendChild(style);
            
            setTimeout(() => {
                alertBox.remove();
                style.remove();
            }, 3000);
        };
        
        // --- 1. Clock Update ---
        function updateClock() {
            const now = new Date();
            const timeString = now.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' });
            document.getElementById('clock').textContent = timeString;
        }
        setInterval(updateClock, 1000);
        updateClock();

        // --- SHUTDOWN/STARTUP FUNCTIONALITY ---
        function shutdown() {
            if (startMenu.style.display === 'flex') {
                toggleStartMenu();
            }
            desktopBg.style.display = 'none';
            taskbar.style.display = 'none';
            shutdownOverlay.style.display = 'flex';
        }

        function startup() {
            shutdownOverlay.style.display = 'none';
            desktopBg.style.display = 'block';
            taskbar.style.display = 'flex';
        }

        // --- START MENU FUNCTIONALITY ---
        function toggleStartMenu() {
            const isOpen = startMenu.style.display === 'flex';
            if (isOpen) {
                startMenu.style.display = 'none';
                startButton.classList.remove('active');
            } else {
                startMenu.style.display = 'flex';
                startButton.classList.add('active');
            }
        }

        document.addEventListener('click', (e) => {
            const isClickInsideStartMenu = startMenu.contains(e.target);
            const isClickOnStartButton = startButton.contains(e.target);

            if (taskbar.style.display !== 'none' && startMenu.style.display === 'flex' && !isClickInsideStartMenu && !isClickOnStartButton) {
                startMenu.style.display = 'none';
                startButton.classList.remove('active');
            }
        });


        // --- WINDOW MANAGEMENT ---
        
        function getWindowElement(id) {
            return document.getElementById(id);
        }

        function toggleWindow(id, event) {
            if (event) event.stopPropagation(); 
            
            const win = getWindowElement(id);
            if (!win) return;
            
            if (startMenu.style.display === 'flex') {
                toggleStartMenu();
            }

            if (win.style.display === 'block') {
                // If already visible, just focus it (bring to front)
                focusWindow(id);
            } else {
                // Open and focus
                win.style.display = 'block';
                focusWindow(id);
            }
        }

        function focusWindow(id) {
            const win = getWindowElement(id);
            if (!win) return;

            // Increment Z-index and apply it
            highestZIndex++;
            win.style.zIndex = highestZIndex;

            // Remove and re-add click listener to ensure window is focused on interaction
            win.removeEventListener('mousedown', () => focusWindow(id));
            win.addEventListener('mousedown', () => focusWindow(id));
        }

        function toggleMaximize(id, maxBtnId) {
            const win = getWindowElement(id);
            const state = windowBounds[id];
            const maxBtn = document.getElementById(maxBtnId);

            if (state.isMaximized) {
                // RESTORE
                win.classList.remove('maximized');
                win.style.width = state.width + 'px';
                win.style.height = state.height + 'px';
                win.style.left = state.left;
                win.style.top = state.top;
                win.style.transform = state.transform;
                maxBtn.textContent = '□'; 
                getWindowElement(id + '-titlebar').style.cursor = 'grab';
            } else {
                // MAXIMIZE
                // 1. Save current bounds for restore
                state.width = win.offsetWidth;
                state.height = win.offsetHeight;
                state.left = win.style.left;
                state.top = win.style.top;
                state.transform = win.style.transform;
                
                // 2. Apply maximized class
                win.classList.add('maximized');
                maxBtn.textContent = '▣'; 
                getWindowElement(id + '-titlebar').style.cursor = 'default';
            }
            state.isMaximized = !state.isMaximized;
        }
        
        // --- WINDOW DRAGGING LOGIC ---
        let isDragging = false;
        let activeWindow = null;
        let offset = { x: 0, y: 0 };

        function handleMouseDown(e) {
            const titlebar = e.currentTarget;
            const win = titlebar.parentElement;
            const id = win.id;
            const state = windowBounds[id];

            // If maximized or clicking a control button, do not drag
            if (state.isMaximized || e.target.classList.contains('win7-control-btn') || e.target.closest('.win7-controls')) return;
            
            isDragging = true;
            activeWindow = win;
            titlebar.style.cursor = 'grabbing';
            focusWindow(id);
            
            offset.x = e.clientX - win.offsetLeft;
            offset.y = e.clientY - win.offsetTop;
            
            // Remove transform for accurate offset math
            win.style.transform = 'none'; 
            win.style.left = win.offsetLeft + 'px';
            win.style.top = win.offsetTop + 'px';
        }

        function handleMouseMove(e) {
            if (!isDragging || !activeWindow) return;
            
            let newX = e.clientX - offset.x;
            let newY = e.clientY - offset.y;

            const desktopHeight = document.getElementById('desktop-bg').offsetHeight;
            const desktopWidth = document.getElementById('desktop-bg').offsetWidth;
            
            // Constrain window movement
            newX = Math.min(Math.max(0, newX), desktopWidth - activeWindow.offsetWidth);
            newY = Math.min(Math.max(0, newY), desktopHeight - activeWindow.offsetHeight);

            activeWindow.style.left = newX + 'px';
            activeWindow.style.top = newY + 'px';
        }

        function handleMouseUp() {
            if (activeWindow) {
                activeWindow.querySelector('.win7-titlebar').style.cursor = 'grab';
            }
            isDragging = false;
            activeWindow = null;
        }

        // Attach dragging events to all titlebars
        document.getElementById('ie-titlebar').addEventListener('mousedown', handleMouseDown);
        document.getElementById('explorer-titlebar').addEventListener('mousedown', handleMouseDown);
        document.getElementById('notepad-titlebar').addEventListener('mousedown', handleMouseDown);
        document.addEventListener('mousemove', handleMouseMove);
        document.addEventListener('mouseup', handleMouseUp);
        
        
        // --- BROWSER NAVIGATION LOGIC ---
        
        function handleFrameLoad() {
            // Cannot access frame content due to Cross-Origin restrictions
            ieTitle.textContent = `Internet Explorer 9 - Untitled Page`; 
        }

        function navigateTo(action) {
            // ... (Browser navigation logic remains the same)
            if (action === 'go') {
                let url = urlInput.value.trim();
                if (!url) {
                    url = 'https://wikipedia.org/'; 
                    urlInput.value = url;
                }
                
                if (!url.match(/^https?:\/\//i)) {
                    url = 'http://' + url;
                    urlInput.value = url;
                }
                browserFrame.src = url;
                ieTitle.textContent = `Internet Explorer 9 - Loading: ${url}`;
            } else if (action === 'back') {
                try { 
                    browserFrame.contentWindow.history.back(); 
                    ieTitle.textContent = `Internet Explorer 9 - Navigating Back...`;
                } catch(e) { 
                    window.alert("Browser navigation is restricted due to security (Same-Origin Policy)."); 
                    console.error("History Back Failed (Security):", e); 
                    ieTitle.textContent = `Internet Explorer 9 - Navigation Failed`;
                }
            } else if (action === 'forward') {
                try { 
                    browserFrame.contentWindow.history.forward(); 
                    ieTitle.textContent = `Internet Explorer 9 - Navigating Forward...`;
                } catch(e) { 
                    window.alert("Browser navigation is restricted due to security (Same-Origin Policy)."); 
                    console.error("History Forward Failed (Security):", e); 
                    ieTitle.textContent = `Internet Explorer 9 - Navigation Failed`;
                }
            }
        }

        urlInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                navigateTo('go');
            }
        });

        // Attach load handler to prevent cross-origin errors
        browserFrame.addEventListener('load', handleFrameLoad);


        // --- FILE EXPLORER LOGIC ---

        // Function to find the full file object from a path string
        function findFileObject(path) {
            // Top-level paths are handled in loadFolder, but this handles sub-paths starting with C:/
            
            let parts = path.split('/');
            let current = VFS;
            
            // Handle C:/
            if (path.startsWith('C:/')) {
                current = VFS['C:/'].contents;
                parts = parts.slice(1);
            } else if (VFS[parts[0]]) {
                // If it starts with a root folder name (e.g., Documents/Work)
                current = VFS[parts[0]].contents;
                parts = parts.slice(1);
            } else {
                 // Should not happen with current VFS structure
                 return null;
            }

            for (let i = 0; i < parts.length; i++) {
                const part = parts[i];
                if (current[part]) {
                    if (i === parts.length - 1) {
                        return current[part];
                    }
                    current = current[part].contents || {};
                } else {
                    return null;
                }
            }
            return null;
        }

        // Action to open a file using the Notepad application
        function openFile(fullPath, name) {
            const file = findFileObject(fullPath);
            
            if (!file) {
                return window.alert(`Error: File "${name}" not found at path: ${fullPath}`);
            }

            if (file.type === 'file') {
                if (file.content) {
                    notepadTitle.textContent = `${name} - Notepad`;
                    notepadTextarea.value = file.content;
                } else {
                    notepadTitle.textContent = `${name} - Notepad`;
                    notepadTextarea.value = `*** ATTENTION: SYSTEM FILE ACCESS GRANTED ***\n\nThis is the content of the file: ${name}\n\nSince this file was previously marked as "protected" or "system" and did not contain viewable text, we are showing a placeholder message.\n\nYou have successfully bypassed the simulated system restriction.`;
                }
                // Focus and show the Notepad window
                toggleWindow('notepad-window');
            } else {
                window.alert(`The file "${name}" is an unrecognized type and cannot be opened.`);
            }
        }


        function loadFolder(path, event) {
            if (event) event.stopPropagation();

            // Ensure the explorer window is open/focused when navigating via the Nav Pane
            if (document.getElementById('explorer-window').style.display !== 'block') {
                toggleWindow('explorer-window');
            } else {
                focusWindow('explorer-window');
            }

            // FIX START: Use findFileObject for all non-root paths to allow deep navigation
            let folder;
            if (VFS[path]) {
                folder = VFS[path]; // Top-level paths like 'C:/'
            } else {
                folder = findFileObject(path); // Sub-paths like 'C:/Program Files (x86)'
            }
            
            if (!folder) {
                return window.alert(`Error: Path "${path}" not found in VFS.`);
            }
            // FIX END

            // 1. Update global state and history
            currentPath = path;
            const historyLength = explorerHistory.length;
            if (explorerHistoryIndex < historyLength - 1) {
                explorerHistory.splice(explorerHistoryIndex + 1); // Truncate forward history
            }
            if (explorerHistory[historyLength - 1] !== path) {
                explorerHistory.push(path);
                explorerHistoryIndex = explorerHistory.length - 1;
            }

            // 2. Update UI
            explorerTitle.textContent = path === 'Computer' ? 'Computer' : path;
            explorerAddressBar.value = path;
            fileView.innerHTML = '';
            
            // Deselect all nav items and select the current one
            navPane.querySelectorAll('.nav-item').forEach(item => {
                item.classList.remove('selected');
                if (item.getAttribute('data-path') === path) {
                    item.classList.add('selected');
                }
            });

            // 3. Render contents
            if (folder.contents) {
                // If it's the Computer view, don't show the "Computer" icon itself.
                const itemsToRender = folder.type === 'system' && path === 'Computer' ? folder.contents : folder.contents;
                
                let contentFound = false;

                for (const name in itemsToRender) {
                    const item = itemsToRender[name];
                    const iconType = item.type; // 'folder', 'file', 'drive', 'system'
                    
                    let iconEmoji = '';
                    let clickAction = '';
                    let isFolder = false;

                    if (iconType === 'folder' || iconType === 'drive' || iconType === 'system') {
                        isFolder = true;
                        
                        // FIX: Use double-click (ondblclick) for folders in the file view
                        if (item.path === 'ie-window') { // Special link for IE
                             clickAction = `toggleWindow('${item.path}')`;
                             iconEmoji = 'e';
                        } else {
                            clickAction = `loadFolder('${item.path}')`;
                            iconEmoji = iconType === 'drive' ? '🖴' : iconType === 'system' ? '💻' : '📁';
                        }

                    } else if (iconType === 'file') {
                        // Files also use double-click (ondblclick)
                        clickAction = `openFile('${path === 'C:/' || path === 'Documents' || path === 'Desktop' ? path : path + '/'}${name}', '${name}')`;
                        
                        // Set appropriate emoji based on file extension
                        if (name.endsWith('.txt')) {
                            iconEmoji = '📄';
                        } else if (name.endsWith('.docx')) {
                            iconEmoji = '✍️';
                        } else if (name.endsWith('.jpg') || name.endsWith('.png')) {
                            iconEmoji = '🖼️';
                        } else if (name.endsWith('.sys') || name.endsWith('.ini')) {
                            iconEmoji = '⚙️'; // System file icon
                        } else {
                            iconEmoji = '❓';
                        }
                    }

                    const element = document.createElement('div');
                    element.className = 'file-icon';
                    element.setAttribute('ondblclick', clickAction); // Use dblclick for ALL items in file-view
                    element.setAttribute('onclick', `focusWindow('explorer-window')`); // Click to focus explorer
                    element.innerHTML = `
                        <span class="file-icon-img text-3xl ${isFolder ? 'folder-icon' : ''}">${iconEmoji}</span>
                        <span>${name}</span>
                    `;
                    fileView.appendChild(element);
                    contentFound = true;
                }
                
                if (!contentFound && Object.keys(folder.contents).length === 0) {
                    fileView.innerHTML = '<p class="text-gray-500 p-2">This folder is empty.</p>';
                }

            } else {
                fileView.innerHTML = '<p class="text-gray-500 p-2">This folder is empty or not accessible.</p>';
            }
        }

        function navigateExplorer(direction) {
            let newIndex = explorerHistoryIndex;
            if (direction === 'back' && explorerHistoryIndex > 0) {
                newIndex--;
            } else if (direction === 'forward' && explorerHistoryIndex < explorerHistory.length - 1) {
                newIndex++;
            } else {
                return; // No change
            }

            explorerHistoryIndex = newIndex;
            const newPath = explorerHistory[explorerHistoryIndex];
            
            // Re-load the folder without adding to history (since it's already in history)
            loadFolder(newPath);
            // Manually update the visual address bar and title
            explorerTitle.textContent = newPath === 'Computer' ? 'Computer' : newPath;
            explorerAddressBar.value = newPath;
        }

        // Initialize Explorer on load
        window.addEventListener('load', () => {
             // Initial load of the "Computer" folder
            loadFolder('Computer'); 
        });

    </script>
</body>
</html>