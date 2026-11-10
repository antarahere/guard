[aegis_crypto_guard.html](https://github.com/user-attachments/files/34626131/aegis_crypto_guard.html)
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aegis Crypto Guard | Anti-Pump & Dump Protection</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Josefin Sans -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;1,400&display=swap" rel="stylesheet">
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        sage: {
                            50: '#F4F7F4',
                            100: '#E8EFEA',
                            200: '#D1DFC9',
                            300: '#B2CCA6',
                            400: '#87A96B', /* Main Sage */
                            500: '#688A4C',
                            600: '#4A6B53', /* Dark Sage */
                            700: '#38523F',
                            800: '#2A3D30',
                            900: '#1D2A21',
                        },
                        charcoal: '#2C3E35',
                        cream: '#F9FBF9',
                    },
                    fontFamily: {
                        sans: ['"Josefin Sans"', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Josefin Sans', sans-serif;
            background-color: #F4F7F4;
            color: #2C3E35;
        }

        /* Custom Scrollbars */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #E8EFEA;
        }
        ::-webkit-scrollbar-thumb {
            background: #87A96B;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #4A6B53;
        }

        /* Subtle Pulse Animations */
        @keyframes pulse-sage {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.85; transform: scale(1.02); }
        }
        .pulse-alert {
            animation: pulse-sage 2s infinite ease-in-out;
        }

        /* Circular Progress styling */
        .circle-bg {
            fill: none;
            stroke: #E8EFEA;
            stroke-width: 3.8;
        }
        .circle-progress {
            fill: none;
            stroke-width: 3.8;
            stroke-linecap: round;
            transition: stroke-dasharray 1s ease, stroke 0.5s ease;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col antialiased selection:bg-sage-300 selection:text-sage-900">

    <!-- Header Navigation -->
    <header class="sticky top-0 z-40 bg-white/90 backdrop-blur-md border-b border-sage-200 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-20">
                
                <!-- Logo -->
                <div class="flex items-center space-x-3 cursor-pointer" onclick="switchTab('dashboard')">
                    <div class="w-11 h-11 rounded-xl bg-sage-400 text-white flex items-center justify-center shadow-md shadow-sage-400/30 transform transition hover:scale-105">
                        <i class="fa-solid fa-shield-halved text-2xl"></i>
                    </div>
                    <div>
                        <span class="text-2xl font-bold tracking-tight text-sage-800 block leading-none">AEGIS</span>
                        <span class="text-xs font-semibold tracking-widest text-sage-500 uppercase">Crypto Guard</span>
                    </div>
                </div>

                <!-- Global Search Bar -->
                <div class="hidden md:flex flex-1 max-w-md mx-8 relative">
                    <div class="relative w-full">
                        <input type="text" id="global-search-input" placeholder="Search token name, ticker, or contract address (0x...)..."
                            class="w-full bg-sage-50 border border-sage-200 text-sage-900 rounded-xl py-2.5 pl-10 pr-10 text-sm focus:outline-none focus:ring-2 focus:ring-sage-400 focus:bg-white transition-all">
                        <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-3 text-sage-400 text-sm"></i>
                        <button onclick="handleGlobalSearch()" class="absolute right-2 top-1.5 bg-sage-400 text-white hover:bg-sage-600 text-xs px-3 py-1.5 rounded-lg transition-colors font-semibold">
                            Scan
                        </button>
                    </div>
                </div>

                <!-- Status Indicator & Live Feed -->
                <div class="hidden lg:flex items-center space-x-3 bg-sage-50 px-3.5 py-1.5 rounded-full border border-sage-200 text-xs">
                    <span class="relative flex h-2.5 w-2.5">
                      <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                      <span class="relative inline-flex rounded-full h-2.5 w-2.5 bg-emerald-500"></span>
                    </span>
                    <span class="font-semibold text-sage-700">DEX Sentinel Active</span>
                    <span class="text-sage-400">|</span>
                    <span class="text-sage-600" id="live-block-height">Block #19,428,102</span>
                </div>

                <!-- Mobile Menu Button -->
                <div class="md:hidden flex items-center">
                    <button id="mobile-menu-btn" onclick="toggleMobileMenu()" class="text-sage-700 hover:text-sage-900 p-2 focus:outline-none">
                        <i class="fa-solid fa-bars text-xl"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Navigation Bar Tabs -->
        <nav class="bg-sage-50 border-t border-sage-100 hidden md:block">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex space-x-1 py-1.5 overflow-x-auto">
                    <button id="tab-dashboard" onclick="switchTab('dashboard')" class="tab-btn px-4 py-2 rounded-lg text-sm font-semibold transition-all flex items-center space-x-2 text-sage-800 bg-white shadow-sm border border-sage-200">
                        <i class="fa-solid fa-chart-line text-sage-500"></i>
                        <span>Live Pump Detector</span>
                    </button>
                    <button id="tab-scanner" onclick="switchTab('scanner')" class="tab-btn px-4 py-2 rounded-lg text-sm font-semibold transition-all flex items-center space-x-2 text-sage-600 hover:text-sage-800 hover:bg-white/60">
                        <i class="fa-solid fa-microscope text-sage-500"></i>
                        <span>Token Risk Scanner</span>
                    </button>
                    <button id="tab-simulator" onclick="switchTab('simulator')" class="tab-btn px-4 py-2 rounded-lg text-sm font-semibold transition-all flex items-center space-x-2 text-sage-600 hover:text-sage-800 hover:bg-white/60">
                        <i class="fa-solid fa-gamepad text-sage-500"></i>
                        <span>Pump Simulator</span>
                    </button>
                    <button id="tab-telegram" onclick="switchTab('telegram')" class="tab-btn px-4 py-2 rounded-lg text-sm font-semibold transition-all flex items-center space-x-2 text-sage-600 hover:text-sage-800 hover:bg-white/60">
                        <i class="fa-paper-plane text-sage-500"></i>
                        <span>Social Signal Monitor</span>
                        <span class="bg-amber-100 text-amber-800 text-[10px] px-1.5 py-0.5 rounded-full font-bold">LIVE</span>
                    </button>
                    <button id="tab-education" onclick="switchTab('education')" class="tab-btn px-4 py-2 rounded-lg text-sm font-semibold transition-all flex items-center space-x-2 text-sage-600 hover:text-sage-800 hover:bg-white/60">
                        <i class="fa-solid fa-graduation-cap text-sage-500"></i>
                        <span>Prevention Guide</span>
                    </button>
                </div>
            </div>
        </nav>

        <!-- Mobile Drawer -->
        <div id="mobile-menu" class="hidden md:hidden bg-white border-b border-sage-200 px-4 pt-2 pb-4 space-y-2">
            <div class="mb-3">
                <input type="text" id="mobile-search-input" placeholder="Search token..." class="w-full bg-sage-50 border border-sage-200 rounded-lg p-2 text-sm">
            </div>
            <button onclick="switchTab('dashboard'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-sage-800 hover:bg-sage-50 flex items-center space-x-2">
                <i class="fa-solid fa-chart-line text-sage-500"></i> <span>Live Pump Detector</span>
            </button>
            <button onclick="switchTab('scanner'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-sage-800 hover:bg-sage-50 flex items-center space-x-2">
                <i class="fa-solid fa-microscope text-sage-500"></i> <span>Token Risk Scanner</span>
            </button>
            <button onclick="switchTab('simulator'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-sage-800 hover:bg-sage-50 flex items-center space-x-2">
                <i class="fa-solid fa-gamepad text-sage-500"></i> <span>Pump Simulator</span>
            </button>
            <button onclick="switchTab('telegram'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-sage-800 hover:bg-sage-50 flex items-center space-x-2">
                <i class="fa-paper-plane text-sage-500"></i> <span>Social Signal Monitor</span>
            </button>
            <button onclick="switchTab('education'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-sage-800 hover:bg-sage-50 flex items-center space-x-2">
                <i class="fa-solid fa-graduation-cap text-sage-500"></i> <span>Prevention Guide</span>
            </button>
        </div>
    </header>

    <!-- Toast Notification Container -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col space-y-3 pointer-events-none max-w-sm w-full"></div>

    <!-- Main Content Body -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6">

        <!-- TAB 1: LIVE PUMP DETECTOR DASHBOARD -->
        <section id="sec-dashboard" class="space-y-6">
            
            <!-- Hero Stat Bar -->
            <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                <div class="bg-white p-5 rounded-2xl border border-sage-200 shadow-sm flex items-center justify-between">
                    <div>
                        <p class="text-xs font-semibold text-sage-500 uppercase tracking-wider">Scanned 24h Tokens</p>
                        <h3 class="text-2xl font-bold text-sage-900 mt-1" id="stat-scanned">14,289</h3>
                        <span class="text-xs text-emerald-600 font-medium"><i class="fa-solid fa-arrow-trend-up"></i> +12% from yesterday</span>
                    </div>
                    <div class="w-12 h-12 bg-sage-100 rounded-xl flex items-center justify-center text-sage-600">
                        <i class="fa-solid fa-layer-group text-xl"></i>
                    </div>
                </div>

                <div class="bg-white p-5 rounded-2xl border border-sage-200 shadow-sm flex items-center justify-between">
                    <div>
                        <p class="text-xs font-semibold text-sage-500 uppercase tracking-wider">Active Pump Alerts</p>
                        <h3 class="text-2xl font-bold text-amber-600 mt-1" id="stat-active-alerts">18</h3>
                        <span class="text-xs text-amber-700 font-medium"><i class="fa-solid fa-triangle-exclamation"></i> High Volatility</span>
                    </div>
                    <div class="w-12 h-12 bg-amber-50 rounded-xl flex items-center justify-center text-amber-600">
                        <i class="fa-solid fa-bolt text-xl"></i>
                    </div>
                </div>

                <div class="bg-white p-5 rounded-2xl border border-sage-200 shadow-sm flex items-center justify-between">
                    <div>
                        <p class="text-xs font-semibold text-sage-500 uppercase tracking-wider">Active Dumps Flagged</p>
                        <h3 class="text-2xl font-bold text-rose-600 mt-1" id="stat-dumps">7</h3>
                        <span class="text-xs text-rose-600 font-medium"><i class="fa-solid fa-arrow-down-long"></i> Pullout in progress</span>
                    </div>
                    <div class="w-12 h-12 bg-rose-50 rounded-xl flex items-center justify-center text-rose-600">
                        <i class="fa-solid fa-chart-line-down text-xl"></i>
                    </div>
                </div>

                <div class="bg-white p-5 rounded-2xl border border-sage-200 shadow-sm flex items-center justify-between">
                    <div>
                        <p class="text-xs font-semibold text-sage-500 uppercase tracking-wider">Liquidity Protected</p>
                        <h3 class="text-2xl font-bold text-sage-700 mt-1">$4.82M</h3>
                        <span class="text-xs text-sage-600 font-medium"><i class="fa-solid fa-shield-check"></i> Aegis Auto-Shield</span>
                    </div>
                    <div class="w-12 h-12 bg-sage-400 text-white rounded-xl flex items-center justify-center shadow-sm">
                        <i class="fa-solid fa-lock text-xl"></i>
                    </div>
                </div>
            </div>

            <!-- Charts Section: Live Telemetry & Anomaly Radar -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- Interactive Chart Canvas -->
                <div class="lg:col-span-2 bg-white p-6 rounded-2xl border border-sage-200 shadow-sm flex flex-col justify-between">
                    <div class="flex flex-col sm:flex-row sm:items-center justify-between pb-4 border-b border-sage-100 gap-2">
                        <div>
                            <div class="flex items-center space-x-2">
                                <span class="bg-rose-100 text-rose-800 text-xs px-2.5 py-0.5 rounded-full font-bold uppercase tracking-wider">Active Spike Telemetry</span>
                                <span class="text-xs font-bold text-sage-600">$PUMP / WETH (Uniswap v3)</span>
                            </div>
                            <h2 class="text-lg font-bold text-sage-900 mt-1">Volume vs Open Interest Spike Detection</h2>
                        </div>
                        <div class="flex space-x-2">
                            <button onclick="changeChartTimespan('5m')" class="chart-time-btn text-xs px-3 py-1.5 rounded-lg bg-sage-50 text-sage-700 hover:bg-sage-200 font-semibold active">5m</button>
                            <button onclick="changeChartTimespan('15m')" class="chart-time-btn text-xs px-3 py-1.5 rounded-lg bg-sage-50 text-sage-700 hover:bg-sage-200 font-semibold">15m</button>
                            <button onclick="changeChartTimespan('1h')" class="chart-time-btn text-xs px-3 py-1.5 rounded-lg bg-sage-50 text-sage-700 hover:bg-sage-200 font-semibold">1h</button>
                        </div>
                    </div>
                    
                    <div class="py-4 relative h-64 sm:h-72 w-full">
                        <canvas id="liveTelemetryCanvas" class="w-full h-full rounded-xl"></canvas>
                        <div id="anomaly-badge" class="absolute top-4 left-4 bg-rose-500/90 text-white text-xs font-bold px-3 py-1 rounded-lg backdrop-blur-sm shadow flex items-center space-x-1 animate-bounce">
                            <i class="fa-solid fa-triangle-exclamation"></i>
                            <span>Artificial Surge Divergence Detected</span>
                        </div>
                    </div>

                    <div class="grid grid-cols-3 gap-3 pt-3 border-t border-sage-100 text-center">
                        <div class="bg-sage-50 p-2.5 rounded-xl">
                            <span class="text-xs text-sage-500 block">5m Volume Spike</span>
                            <span class="font-bold text-rose-600 text-sm sm:text-base">+1,420%</span>
                        </div>
                        <div class="bg-sage-50 p-2.5 rounded-xl">
                            <span class="text-xs text-sage-500 block">Whale Wallet Inflow</span>
                            <span class="font-bold text-sage-800 text-sm sm:text-base">78.4% of Pool</span>
                        </div>
                        <div class="bg-sage-50 p-2.5 rounded-xl">
                            <span class="text-xs text-sage-500 block">Predicted Peak</span>
                            <span class="font-bold text-amber-600 text-sm sm:text-base">~2.5 Mins Left</span>
                        </div>
                    </div>
                </div>

                <!-- Threat Detection Radar Feed -->
                <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between pb-3 border-b border-sage-100">
                            <h3 class="font-bold text-sage-900 flex items-center space-x-2">
                                <i class="fa-solid fa-radar text-sage-500"></i>
                                <span>Risk Signals Feed</span>
                            </h3>
                            <span class="text-xs bg-sage-100 text-sage-700 font-semibold px-2 py-0.5 rounded-md">Realtime</span>
                        </div>
                        
                        <div class="mt-4 space-y-3 overflow-y-auto max-h-[300px] pr-1" id="radar-signals-list">
                            <!-- Signal Items dynamically loaded -->
                        </div>
                    </div>

                    <div class="mt-4 pt-4 border-t border-sage-100 text-center">
                        <button onclick="triggerSampleAlert()" class="w-full bg-sage-100 hover:bg-sage-200 text-sage-800 text-xs font-bold py-2.5 rounded-xl transition flex items-center justify-center space-x-2">
                            <i class="fa-solid fa-vial"></i>
                            <span>Simulate Live Anomaly Warning</span>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Live Monitoring Filterable Table -->
            <div class="bg-white rounded-2xl border border-sage-200 shadow-sm overflow-hidden">
                <div class="p-6 border-b border-sage-100 flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                    <div>
                        <h2 class="text-lg font-bold text-sage-900">Live Token Surveillance Desk</h2>
                        <p class="text-xs text-sage-500">Continuous liquidity, contract owner action, and buy/sell ratio tracking</p>
                    </div>
                    
                    <!-- Filters -->
                    <div class="flex flex-wrap items-center gap-2">
                        <span class="text-xs font-semibold text-sage-600 mr-1">Filter Risk:</span>
                        <button onclick="filterTable('ALL')" class="tbl-filter-btn text-xs px-3 py-1.5 rounded-lg bg-sage-600 text-white font-semibold">All</button>
                        <button onclick="filterTable('HIGH')" class="tbl-filter-btn text-xs px-3 py-1.5 rounded-lg bg-sage-100 text-sage-700 hover:bg-sage-200 font-semibold">High Pump Risk</button>
                        <button onclick="filterTable('DUMP')" class="tbl-filter-btn text-xs px-3 py-1.5 rounded-lg bg-sage-100 text-sage-700 hover:bg-sage-200 font-semibold">Active Dump</button>
                        <button onclick="filterTable('SAFE')" class="tbl-filter-btn text-xs px-3 py-1.5 rounded-lg bg-sage-100 text-sage-700 hover:bg-sage-200 font-semibold">Safe / Low Vol</button>
                    </div>
                </div>

                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-sage-50/70 border-b border-sage-100 text-xs font-bold text-sage-600 uppercase tracking-wider">
                                <th class="py-3.5 px-6">Token</th>
                                <th class="py-3.5 px-4">Risk Status</th>
                                <th class="py-3.5 px-4">5m Vol Spike</th>
                                <th class="py-3.5 px-4">Holder Conc.</th>
                                <th class="py-3.5 px-4">Liquidity Lock</th>
                                <th class="py-3.5 px-4">Risk Score</th>
                                <th class="py-3.5 px-6 text-right">Action</th>
                            </tr>
                        </thead>
                        <tbody id="token-table-body" class="divide-y divide-sage-100 text-sm font-medium">
                            <!-- Table Rows injected via JS -->
                        </tbody>
                    </table>
                </div>
            </div>

        </section>

        <!-- TAB 2: TOKEN RISK SCANNER -->
        <section id="sec-scanner" class="space-y-6 hidden">
            <!-- Search & Banner -->
            <div class="bg-gradient-to-r from-sage-700 to-sage-900 rounded-2xl p-6 text-white shadow-lg relative overflow-hidden">
                <div class="relative z-10 max-w-2xl">
                    <span class="bg-sage-400/30 text-sage-200 text-xs px-3 py-1 rounded-full uppercase tracking-widest font-semibold">Institutional Grade Audit</span>
                    <h2 class="text-2xl sm:text-3xl font-bold mt-2">Deep Contract & Liquidity Scanner</h2>
                    <p class="text-sage-100 text-sm mt-1">Paste any ERC-20, BEP-20, or Solana token address to inspect ownership distribution, honeypot code, and artificial volume signatures.</p>

                    <div class="mt-5 flex flex-col sm:flex-row gap-2">
                        <input type="text" id="scanner-input" placeholder="Enter Token Address (e.g., 0x71c... or $PEPE)" 
                            class="flex-1 px-4 py-3 rounded-xl text-sage-900 bg-white text-sm focus:outline-none focus:ring-2 focus:ring-sage-300 font-mono">
                        <button onclick="runDeepScan()" class="bg-sage-400 hover:bg-sage-500 text-white font-bold px-6 py-3 rounded-xl transition shadow-md flex items-center justify-center space-x-2">
                            <i class="fa-solid fa-radar"></i>
                            <span>Run Security Audit</span>
                        </button>
                    </div>
                </div>
                <div class="absolute -right-10 -bottom-10 opacity-10 text-white pointer-events-none">
                    <i class="fa-solid fa-shield-virus text-[240px]"></i>
                </div>
            </div>

            <!-- Scan Output Dashboard -->
            <div id="scan-results-container" class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- Left Column: Safety Gauge & Verdict -->
                <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm flex flex-col justify-between">
                    <div class="text-center">
                        <h3 class="text-xs font-bold text-sage-500 uppercase tracking-widest">Overall Safety Score</h3>
                        <div id="scanned-token-name" class="text-xl font-bold text-sage-900 mt-1">$SAFEINU (Safe Inu)</div>
                        <p class="text-xs text-sage-500 font-mono" id="scanned-token-address">0x71c2b84931a...390e</p>

                        <!-- Circular Meter -->
                        <div class="relative w-44 h-44 mx-auto my-4 flex items-center justify-center">
                            <svg class="w-full h-full transform -rotate-90" viewBox="0 0 36 36">
                                <path class="circle-bg" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                                <path id="score-circle" class="circle-progress text-sage-400" stroke-dasharray="82, 100" stroke="currentColor" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                            </svg>
                            <div class="absolute flex flex-col items-center">
                                <span class="text-4xl font-extrabold text-sage-900" id="score-text">82</span>
                                <span class="text-xs font-bold text-sage-500 uppercase tracking-wider">/ 100</span>
                            </div>
                        </div>

                        <div id="verdict-badge" class="inline-block bg-emerald-100 text-emerald-800 px-4 py-1.5 rounded-full text-xs font-bold uppercase tracking-wider">
                            LOW PUMP RISK - VERIFIED
                        </div>
                    </div>

                    <div class="mt-6 pt-4 border-t border-sage-100 space-y-2 text-xs">
                        <div class="flex justify-between">
                            <span class="text-sage-500">Contract Creator:</span>
                            <span class="font-mono text-sage-800">0x89...11b2 (Clean)</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-sage-500">Mint Function:</span>
                            <span class="font-bold text-emerald-600">Disabled / Revoked</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-sage-500">Honeypot Test:</span>
                            <span class="font-bold text-emerald-600">PASSED (0% Tax)</span>
                        </div>
                    </div>
                </div>

                <!-- Middle Column: Dynamic Breakdown Cards -->
                <div class="lg:col-span-2 space-y-6">
                    <!-- Breakdown Grid -->
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        
                        <!-- Metric 1: Liquidity Lock -->
                        <div class="bg-white p-5 rounded-2xl border border-sage-200 shadow-sm">
                            <div class="flex justify-between items-start">
                                <div>
                                    <span class="text-xs font-bold text-sage-500 uppercase">Liquidity Lock Status</span>
                                    <h4 class="text-lg font-bold text-sage-900 mt-1" id="scan-lp-status">Locked for 365 Days</h4>
                                </div>
                                <div class="w-9 h-9 rounded-lg bg-emerald-50 text-emerald-600 flex items-center justify-center">
                                    <i class="fa-solid fa-lock text-sm"></i>
                                </div>
                            </div>
                            <div class="w-full bg-sage-100 h-2 rounded-full mt-3 overflow-hidden">
                                <div class="bg-emerald-500 h-full rounded-full" id="scan-lp-bar" style="width: 95%"></div>
                            </div>
                            <p class="text-[11px] text-sage-500 mt-2" id="scan-lp-desc">95% of initial Uniswap V2 LP tokens locked via Unicrypt.</p>
                        </div>

                        <!-- Metric 2: Top Holder Concentration -->
                        <div class="bg-white p-5 rounded-2xl border border-sage-200 shadow-sm">
                            <div class="flex justify-between items-start">
                                <div>
                                    <span class="text-xs font-bold text-sage-500 uppercase">Top 10 Holder Conc.</span>
                                    <h4 class="text-lg font-bold text-sage-900 mt-1" id="scan-holder-conc">18.4% Total Supply</h4>
                                </div>
                                <div class="w-9 h-9 rounded-lg bg-sage-100 text-sage-600 flex items-center justify-center">
                                    <i class="fa-solid fa-chart-pie text-sm"></i>
                                </div>
                            </div>
                            <div class="w-full bg-sage-100 h-2 rounded-full mt-3 overflow-hidden">
                                <div class="bg-sage-400 h-full rounded-full" id="scan-holder-bar" style="width: 18.4%"></div>
                            </div>
                            <p class="text-[11px] text-sage-500 mt-2" id="scan-holder-desc">Healthy distribution. Low risk of coordinated dev dumping.</p>
                        </div>

                        <!-- Metric 3: Social Volume Divergence -->
                        <div class="bg-white p-5 rounded-2xl border border-sage-200 shadow-sm">
                            <div class="flex justify-between items-start">
                                <div>
                                    <span class="text-xs font-bold text-sage-500 uppercase">Telegram / X Hype Surge</span>
                                    <h4 class="text-lg font-bold text-sage-900 mt-1" id="scan-social-surge">Normal Activity</h4>
                                </div>
                                <div class="w-9 h-9 rounded-lg bg-sage-100 text-sage-600 flex items-center justify-center">
                                    <i class="fa-solid fa-hashtag text-sm"></i>
                                </div>
                            </div>
                            <div class="w-full bg-sage-100 h-2 rounded-full mt-3 overflow-hidden">
                                <div class="bg-sage-400 h-full rounded-full" id="scan-social-bar" style="width: 25%"></div>
                            </div>
                            <p class="text-[11px] text-sage-500 mt-2" id="scan-social-desc">No artificial Telegram bot spam detected in last 2 hours.</p>
                        </div>

                        <!-- Metric 4: Sell Slippage / Tax Risk -->
                        <div class="bg-white p-5 rounded-2xl border border-sage-200 shadow-sm">
                            <div class="flex justify-between items-start">
                                <div>
                                    <span class="text-xs font-bold text-sage-500 uppercase">Contract Buy/Sell Tax</span>
                                    <h4 class="text-lg font-bold text-sage-900 mt-1" id="scan-tax">0% Buy / 0% Sell</h4>
                                </div>
                                <div class="w-9 h-9 rounded-lg bg-emerald-50 text-emerald-600 flex items-center justify-center">
                                    <i class="fa-solid fa-percent text-sm"></i>
                                </div>
                            </div>
                            <div class="w-full bg-sage-100 h-2 rounded-full mt-3 overflow-hidden">
                                <div class="bg-emerald-500 h-full rounded-full" id="scan-tax-bar" style="width: 100%"></div>
                            </div>
                            <p class="text-[11px] text-sage-500 mt-2" id="scan-tax-desc">Standard ERC-20 contract without hidden fee modifiers.</p>
                        </div>
                    </div>

                    <!-- Contract Vulnerability & Flags -->
                    <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm">
                        <h4 class="font-bold text-sage-900 mb-3 text-sm uppercase tracking-wider">Detailed Security Checklist</h4>
                        <div class="space-y-3" id="scan-checklist font-medium">
                            <div class="flex items-center justify-between p-3 rounded-xl bg-sage-50">
                                <div class="flex items-center space-x-3">
                                    <i class="fa-solid fa-circle-check text-emerald-600 text-base"></i>
                                    <span class="text-xs text-sage-800 font-semibold">Proxy / Upgradable Implementation</span>
                                </div>
                                <span class="text-xs font-bold text-emerald-700">Immutable Code</span>
                            </div>
                            <div class="flex items-center justify-between p-3 rounded-xl bg-sage-50">
                                <div class="flex items-center space-x-3">
                                    <i class="fa-solid fa-circle-check text-emerald-600 text-base"></i>
                                    <span class="text-xs text-sage-800 font-semibold">Blacklist Capability</span>
                                </div>
                                <span class="text-xs font-bold text-emerald-700">No Blacklist Logic Found</span>
                            </div>
                            <div class="flex items-center justify-between p-3 rounded-xl bg-sage-50">
                                <div class="flex items-center space-x-3">
                                    <i class="fa-solid fa-circle-check text-emerald-600 text-base"></i>
                                    <span class="text-xs text-sage-800 font-semibold">Whale Wallet Cluster Analysis</span>
                                </div>
                                <span class="text-xs font-bold text-emerald-700">Wallets Dispersed</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 3: INTERACTIVE PUMP SIMULATOR & PROTECTION TOOL -->
        <section id="sec-simulator" class="space-y-6 hidden">
            <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm">
                <div class="max-w-3xl">
                    <span class="bg-sage-100 text-sage-800 text-xs px-3 py-1 rounded-full font-bold uppercase tracking-wider">Educational Laboratory</span>
                    <h2 class="text-2xl font-bold text-sage-900 mt-2">Pump & Dump Execution Simulator</h2>
                    <p class="text-sage-600 text-sm mt-1">
                        Experience how micro-cap pumps work in real time. Watch how fast market makers and dev insiders dump liquidity on retail traders, and see how the Aegis Auto-Stop Protection saves capital.
                    </p>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- Control Panel -->
                <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm flex flex-col justify-between">
                    <div>
                        <h3 class="font-bold text-sage-900 pb-3 border-b border-sage-100 flex items-center space-x-2">
                            <i class="fa-solid fa-sliders text-sage-500"></i>
                            <span>Simulation Parameters</span>
                        </h3>

                        <div class="mt-4 space-y-4 text-xs font-medium">
                            <div>
                                <label class="text-sage-600 block mb-1">Simulated Initial Investment</label>
                                <div class="flex items-center space-x-2">
                                    <input type="number" id="sim-amount" value="1000" class="w-full bg-sage-50 border border-sage-200 rounded-lg p-2 text-sage-900 font-bold focus:outline-none focus:ring-1 focus:ring-sage-400">
                                    <span class="font-bold text-sage-700">USD</span>
                                </div>
                            </div>

                            <div>
                                <label class="text-sage-600 block mb-1">Entry Stage</label>
                                <select id="sim-entry-stage" class="w-full bg-sage-50 border border-sage-200 rounded-lg p-2 text-sage-900 focus:outline-none">
                                    <option value="early">Phase 1: Insider Accumulation (Pre-Pump)</option>
                                    <option value="mid" selected>Phase 2: Signal Call Broadcast (+300% In)</option>
                                    <option value="peak">Phase 3: FOMO Peak (+1200% In)</option>
                                </select>
                            </div>

                            <div>
                                <div class="flex justify-between items-center mb-1">
                                    <label class="text-sage-600">Dev Wallet Dump Severity</label>
                                    <span class="font-bold text-rose-600" id="dump-severity-val">85% Pull</span>
                                </div>
                                <input type="range" id="sim-severity" min="40" max="99" value="85" class="w-full accent-sage-600" oninput="document.getElementById('dump-severity-val').innerText = this.value + '% Pull'">
                            </div>

                            <div class="pt-2 border-t border-sage-100">
                                <label class="flex items-center space-x-2 cursor-pointer">
                                    <input type="checkbox" id="sim-aegis-toggle" checked class="w-4 h-4 text-sage-600 rounded border-sage-300 focus:ring-sage-500">
                                    <span class="font-bold text-sage-800">Enable Aegis Auto-Stop Guard</span>
                                </label>
                                <p class="text-[11px] text-sage-500 mt-1 pl-6">Automatically triggers sell order on detecting dev wallet mempool transaction.</p>
                            </div>
                        </div>
                    </div>

                    <div class="mt-6 pt-4 border-t border-sage-100 space-y-2">
                        <button onclick="runPumpSimulation()" id="start-sim-btn" class="w-full bg-sage-400 hover:bg-sage-600 text-white font-bold py-3 rounded-xl transition shadow-md flex items-center justify-center space-x-2">
                            <i class="fa-solid fa-play"></i>
                            <span>Launch Simulation</span>
                        </button>
                        <button onclick="resetSimulation()" class="w-full bg-sage-100 hover:bg-sage-200 text-sage-700 text-xs font-semibold py-2 rounded-lg transition">
                            Reset Chart
                        </button>
                    </div>
                </div>

                <!-- Simulation Canvas & Live PnL Output -->
                <div class="lg:col-span-2 bg-white p-6 rounded-2xl border border-sage-200 shadow-sm flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between pb-3 border-b border-sage-100">
                            <div>
                                <h3 class="font-bold text-sage-900">Simulated Price Curve</h3>
                                <span class="text-xs text-sage-500" id="sim-status-text">Ready to run simulation...</span>
                            </div>
                            <div class="text-right">
                                <span class="text-xs text-sage-500 block">Portfolio Balance</span>
                                <span class="text-xl font-bold text-sage-900" id="sim-current-val">$1,000.00</span>
                            </div>
                        </div>

                        <!-- Canvas -->
                        <div class="relative h-64 sm:h-72 w-full mt-4">
                            <canvas id="simCanvas" class="w-full h-full rounded-xl bg-sage-50/50 border border-sage-100"></canvas>
                        </div>
                    </div>

                    <!-- Post-Simulation Results Callout -->
                    <div id="sim-results-box" class="mt-4 p-4 rounded-xl bg-sage-50 border border-sage-200 grid grid-cols-1 sm:grid-cols-3 gap-3 text-center">
                        <div>
                            <span class="text-xs text-sage-500 block">Initial Value</span>
                            <span class="font-bold text-sage-800 text-sm" id="res-initial">$1,000.00</span>
                        </div>
                        <div>
                            <span class="text-xs text-sage-500 block">Final Value (Unprotected)</span>
                            <span class="font-bold text-rose-600 text-sm" id="res-unprotected">$150.00 (-85%)</span>
                        </div>
                        <div>
                            <span class="text-xs text-sage-500 block">Final Value (With Aegis Guard)</span>
                            <span class="font-bold text-emerald-600 text-sm" id="res-protected">$940.00 (-6%)</span>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 4: TELEGRAM & SOCIAL SIGNAL MONITOR -->
        <section id="sec-telegram" class="space-y-6 hidden">
            <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                <div>
                    <span class="bg-amber-100 text-amber-800 text-xs px-2.5 py-0.5 rounded-full font-bold uppercase tracking-wider">Social Radar</span>
                    <h2 class="text-2xl font-bold text-sage-900 mt-1">Coordinated Telegram Channel Detector</h2>
                    <p class="text-sage-600 text-sm">Real-time NLP analysis of public VIP crypto pump groups, message velocity, and token countdown calls.</p>
                </div>
                <div class="flex items-center space-x-2 bg-sage-50 px-4 py-2 rounded-xl border border-sage-200">
                    <i class="fa-solid fa-rss text-sage-500 animate-pulse"></i>
                    <span class="text-xs font-bold text-sage-700">Monitoring 412 Telegram Channels</span>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- Group Feed -->
                <div class="lg:col-span-2 space-y-4" id="telegram-feed-list">
                    <!-- Telegram Cards Injected via JS -->
                </div>

                <!-- Channel Risk Insights Sidebar -->
                <div class="space-y-6">
                    <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm">
                        <h3 class="font-bold text-sage-900 pb-3 border-b border-sage-100 text-sm uppercase tracking-wider">Top Flagged Groups Today</h3>
                        <div class="mt-4 space-y-3">
                            <div class="flex items-center justify-between p-2.5 rounded-xl bg-sage-50">
                                <div>
                                    <h4 class="font-bold text-xs text-sage-900">Mega Pump Signals VIP</h4>
                                    <span class="text-[11px] text-sage-500">142,000 members</span>
                                </div>
                                <span class="bg-rose-100 text-rose-800 text-xs px-2 py-0.5 rounded font-bold">98% Risk</span>
                            </div>

                            <div class="flex items-center justify-between p-2.5 rounded-xl bg-sage-50">
                                <div>
                                    <h4 class="font-bold text-xs text-sage-900">Binance Whale Pump Crew</h4>
                                    <span class="text-[11px] text-sage-500">88,400 members</span>
                                </div>
                                <span class="bg-rose-100 text-rose-800 text-xs px-2 py-0.5 rounded font-bold">95% Risk</span>
                            </div>

                            <div class="flex items-center justify-between p-2.5 rounded-xl bg-sage-50">
                                <div>
                                    <h4 class="font-bold text-xs text-sage-900">Gem Hunter Rocket Calls</h4>
                                    <span class="text-[11px] text-sage-500">34,100 members</span>
                                </div>
                                <span class="bg-amber-100 text-amber-800 text-xs px-2 py-0.5 rounded font-bold">78% Risk</span>
                            </div>
                        </div>
                    </div>

                    <div class="bg-gradient-to-br from-sage-800 to-sage-900 text-white p-6 rounded-2xl shadow-sm">
                        <i class="fa-solid fa-lightbulb text-sage-300 text-2xl mb-2"></i>
                        <h4 class="font-bold text-sm">How Group Pumps Work</h4>
                        <p class="text-xs text-sage-200 mt-2 leading-relaxed">
                            Organizers buy cheap low-liquidity micro-caps *before* broadcasting to their group. By the time 50,000 members get the signal, the owners are already executing sell orders into the buying rush.
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 5: EDUCATIONAL & PREVENTION GUIDE -->
        <section id="sec-education" class="space-y-6 hidden">
            <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm">
                <span class="bg-sage-100 text-sage-800 text-xs px-3 py-1 rounded-full font-bold uppercase tracking-wider">Masterclass & Prevention</span>
                <h2 class="text-2xl font-bold text-sage-900 mt-2">How to Identify & Prevent Pump and Dump Losses</h2>
                <p class="text-sage-600 text-sm mt-1">Essential knowledge for navigating decentralized exchanges and avoiding predatory liquidity traps.</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- Guide Card 1 -->
                <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm flex flex-col justify-between hover:shadow-md transition">
                    <div>
                        <div class="w-12 h-12 bg-sage-100 text-sage-700 rounded-xl flex items-center justify-center mb-4 text-xl">
                            <i class="fa-solid fa-magnifying-glass-chart"></i>
                        </div>
                        <h3 class="font-bold text-sage-900 text-base">1. Detect Volume Divergence</h3>
                        <p class="text-xs text-sage-600 mt-2 leading-relaxed">
                            A classic pump signature occurs when 5-minute trading volume explodes by over +500% without any real GitHub updates, press releases, or utility developments. 
                        </p>
                    </div>
                    <div class="mt-4 pt-4 border-t border-sage-100">
                        <span class="text-[11px] font-bold text-sage-500 uppercase tracking-wider">Aegis Rule #1: Check Buy/Sell Ratio</span>
                    </div>
                </div>

                <!-- Guide Card 2 -->
                <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm flex flex-col justify-between hover:shadow-md transition">
                    <div>
                        <div class="w-12 h-12 bg-sage-100 text-sage-700 rounded-xl flex items-center justify-center mb-4 text-xl">
                            <i class="fa-solid fa-user-lock"></i>
                        </div>
                        <h3 class="font-bold text-sage-900 text-base">2. Verify Liquidity Pool Locks</h3>
                        <p class="text-xs text-sage-600 mt-2 leading-relaxed">
                            Never buy a token if the liquidity pool (LP) tokens are held directly in developer personal wallets. Always check for Unicrypt, PinkSale, or Burned LP receipts.
                        </p>
                    </div>
                    <div class="mt-4 pt-4 border-t border-sage-100">
                        <span class="text-[11px] font-bold text-sage-500 uppercase tracking-wider">Aegis Rule #2: Min 6 Month Lock</span>
                    </div>
                </div>

                <!-- Guide Card 3 -->
                <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm flex flex-col justify-between hover:shadow-md transition">
                    <div>
                        <div class="w-12 h-12 bg-sage-100 text-sage-700 rounded-xl flex items-center justify-center mb-4 text-xl">
                            <i class="fa-solid fa-code text-sage-700"></i>
                        </div>
                        <h3 class="font-bold text-sage-900 text-base">3. Beware Honeypot Contracts</h3>
                        <p class="text-xs text-sage-600 mt-2 leading-relaxed">
                            Some malicious code allows users to buy tokens, but modifies sell permissions so only dev whitelisted wallets can dump. Always run dry sell simulations first.
                        </p>
                    </div>
                    <div class="mt-4 pt-4 border-t border-sage-100">
                        <span class="text-[11px] font-bold text-sage-500 uppercase tracking-wider">Aegis Rule #3: Verify Sell Tax Limits</span>
                    </div>
                </div>
            </div>

            <!-- FAQ Accordion -->
            <div class="bg-white p-6 rounded-2xl border border-sage-200 shadow-sm">
                <h3 class="font-bold text-sage-900 text-lg mb-4">Frequently Asked Questions</h3>
                <div class="space-y-3" id="faq-list">
                    <div class="border border-sage-100 rounded-xl p-4 cursor-pointer hover:bg-sage-50/50 transition" onclick="toggleFaq(this)">
                        <div class="flex justify-between items-center font-bold text-sm text-sage-800">
                            <span>How does Aegis detect a pump before the peak?</span>
                            <i class="fa-solid fa-chevron-down text-sage-400"></i>
                        </div>
                        <p class="text-xs text-sage-600 mt-2 hidden leading-relaxed">
                            Aegis monitors mempool pending transactions, sudden surges in DEX gas priority fees from cluster wallets, and bot group activity signals before the public buying surge reflects on standard price charts.
                        </p>
                    </div>

                    <div class="border border-sage-100 rounded-xl p-4 cursor-pointer hover:bg-sage-50/50 transition" onclick="toggleFaq(this)">
                        <div class="flex justify-between items-center font-bold text-sm text-sage-800">
                            <span>What is a Liquidity Pull vs a Rug Pull?</span>
                            <i class="fa-solid fa-chevron-down text-sage-400"></i>
                        </div>
                        <p class="text-xs text-sage-600 mt-2 hidden leading-relaxed">
                            A Liquidity Pull happens when token creators remove ETH/SOL locked in the liquidity pool, leaving holders unable to exchange tokens. A Rug Pull is a broader term that includes liquidating developer reserve wallets directly onto open DEX order books.
                        </p>
                    </div>

                    <div class="border border-sage-100 rounded-xl p-4 cursor-pointer hover:bg-sage-50/50 transition" onclick="toggleFaq(this)">
                        <div class="flex justify-between items-center font-bold text-sm text-sage-800">
                            <span>Can Aegis automatically stop me from buying high risk tokens?</span>
                            <i class="fa-solid fa-chevron-down text-sage-400"></i>
                        </div>
                        <p class="text-xs text-sage-600 mt-2 hidden leading-relaxed">
                            Yes. By connecting the Aegis Web3 Auto-Shield extension or RPC endpoint, trade executions will automatically fail if the destination token fails minimum security scores (e.g. unverified LP lock or 95%+ dev wallet concentration).
                        </p>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-white border-t border-sage-200 py-8 mt-12 text-sage-600">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col sm:flex-row justify-between items-center space-y-4 sm:space-y-0">
            <div class="flex items-center space-x-2">
                <div class="w-6 h-6 rounded-md bg-sage-400 text-white flex items-center justify-center text-xs font-bold">
                    <i class="fa-solid fa-shield-halved"></i>
                </div>
                <span class="font-bold text-sage-800 text-sm">AEGIS Crypto Guard</span>
                <span class="text-xs text-sage-400">| Anti-Manipulation Engine</span>
            </div>
            <div class="text-xs text-sage-500">
                Designed with Sage Green & White aesthetic. Built for crypto investor safety.
            </div>
        </div>
    </footer>

    <!-- JavaScript Application Logic -->
    <script>
        // Sample Mock Tokens Data
        const initialTokens = [
            { name: "PUMP IT DOGE", ticker: "$PUMP", risk: "HIGH", surge: "+1,420%", concentration: "78.4%", lock: "Unlocked", score: 14, address: "0x9a23...110e" },
            { name: "SAFE INU", ticker: "$SAFEINU", risk: "SAFE", surge: "+12.4%", concentration: "18.4%", lock: "Locked (365d)", score: 82, address: "0x71c2...390e" },
            { name: "MOON ROCKET", ticker: "$ROCKET", risk: "DUMP", surge: "-88.2%", concentration: "92.0%", lock: "Unlocked", score: 08, address: "0x44b1...88ab" },
            { name: "NEO ETHEREUM", ticker: "$NETH", risk: "HIGH", surge: "+890%", concentration: "65.1%", lock: "Locked (7d)", score: 29, address: "0x00c4...9911" },
            { name: "PEPE CLASSIC", ticker: "$PEPEC", risk: "SAFE", surge: "+4.1%", concentration: "12.3%", lock: "Burned LP", score: 91, address: "0x3310...77fa" },
            { name: "SHIBA GOLD", ticker: "$SHIBG", risk: "HIGH", surge: "+630%", concentration: "81.0%", lock: "Unlocked", score: 22, address: "0x88f2...4412" }
        ];

        // Sample Telegram Feed Mock Data
        const telegramFeeds = [
            { group: "VIP Crypto Pump Signals", members: "142,000", msg: "🚨 NEXT TARGET CALL IN 5 MINUTES! Prepare your BNB on PancakeSwap!", time: "1 min ago", status: "HIGH RISK - COUNTDOWN ACTIVE" },
            { group: "Binance Whales Club", members: "88,400", msg: "BUY NOW! $PUMP IS GOING TO THE MOON 🚀 TARGET 1000%!", time: "4 mins ago", status: "COORDINATED BUYING DETECTED" },
            { group: "DeFi Gem Leaks", members: "34,100", msg: "Liquidity lock confirmed for $SAFEINU. Organic growth pattern noted.", time: "12 mins ago", status: "SAFE PATTERN" }
        ];

        let currentTab = 'dashboard';
        let telemetryChartInstance = null;
        let simChartInstance = null;

        // Initialize Application on Window Load
        window.onload = function() {
            renderTokenTable(initialTokens);
            renderTelegramFeed();
            initTelemetryChart();
            initSimChart();
            startMockLiveBlockCounter();
            startRandomToastTrigger();
        };

        // Navigation Tab Switching Logic
        function switchTab(tabName) {
            currentTab = tabName;
            
            // Hide all sections
            document.querySelectorAll('main > section').forEach(sec => sec.classList.add('hidden'));
            
            // Show target section
            const targetSec = document.getElementById(`sec-${tabName}`);
            if (targetSec) targetSec.classList.remove('hidden');

            // Reset tab button styles
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('bg-white', 'shadow-sm', 'border', 'border-sage-200', 'text-sage-800');
                btn.classList.add('text-sage-600', 'hover:bg-white/60');
            });

            // Active Tab Style
            const activeBtn = document.getElementById(`tab-${tabName}`);
            if (activeBtn) {
                activeBtn.classList.remove('text-sage-600', 'hover:bg-white/60');
                activeBtn.classList.add('bg-white', 'shadow-sm', 'border', 'border-sage-200', 'text-sage-800');
            }

            // Scroll top
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // Toggle Mobile Menu
        function toggleMobileMenu() {
            const menu = document.getElementById('mobile-menu');
            menu.classList.toggle('hidden');
        }

        // Global Search Handler
        function handleGlobalSearch() {
            const query = document.getElementById('global-search-input').value.trim();
            if (!query) return;

            switchTab('scanner');
            document.getElementById('scanner-input').value = query;
            runDeepScan();
        }

        // Table Rendering Function
        function renderTokenTable(tokens) {
            const tbody = document.getElementById('token-table-body');
            tbody.innerHTML = '';

            tokens.forEach(t => {
                let riskBadge = '';
                if (t.risk === 'HIGH') {
                    riskBadge = `<span class="bg-amber-100 text-amber-800 text-xs px-2.5 py-1 rounded-full font-bold flex items-center w-max space-x-1"><i class="fa-solid fa-triangle-exclamation text-[10px]"></i> <span>High Pump Risk</span></span>`;
                } else if (t.risk === 'DUMP') {
                    riskBadge = `<span class="bg-rose-100 text-rose-800 text-xs px-2.5 py-1 rounded-full font-bold flex items-center w-max space-x-1 animate-pulse"><i class="fa-solid fa-arrow-down-long text-[10px]"></i> <span>Active Dump</span></span>`;
                } else {
                    riskBadge = `<span class="bg-emerald-100 text-emerald-800 text-xs px-2.5 py-1 rounded-full font-bold flex items-center w-max space-x-1"><i class="fa-solid fa-shield-check text-[10px]"></i> <span>Safe</span></span>`;
                }

                let scoreColor = t.score > 70 ? 'text-emerald-600' : (t.score > 30 ? 'text-amber-600' : 'text-rose-600');

                const tr = document.createElement('tr');
                tr.className = "hover:bg-sage-50/60 transition";
                tr.innerHTML = `
                    <td class="py-3.5 px-6">
                        <div class="font-bold text-sage-900">${t.name}</div>
                        <div class="text-xs text-sage-500 font-mono">${t.ticker} • ${t.address}</div>
                    </td>
                    <td class="py-3.5 px-4">${riskBadge}</td>
                    <td class="py-3.5 px-4 font-bold ${t.surge.startsWith('+') && parseInt(t.surge) > 100 ? 'text-rose-600' : 'text-sage-800'}">${t.surge}</td>
                    <td class="py-3.5 px-4 text-sage-700">${t.concentration}</td>
                    <td class="py-3.5 px-4">
                        <span class="text-xs font-semibold ${t.lock.includes('Unlocked') ? 'text-rose-600' : 'text-emerald-700'}">${t.lock}</span>
                    </td>
                    <td class="py-3.5 px-4 font-bold ${scoreColor}">${t.score} / 100</td>
                    <td class="py-3.5 px-6 text-right">
                        <button onclick="inspectToken('${t.ticker}', '${t.address}')" class="bg-sage-100 hover:bg-sage-400 hover:text-white text-sage-800 text-xs font-bold px-3 py-1.5 rounded-lg transition">
                            Inspect
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        // Filter Table by Risk
        function filterTable(type) {
            document.querySelectorAll('.tbl-filter-btn').forEach(b => {
                b.classList.remove('bg-sage-600', 'text-white');
                b.classList.add('bg-sage-100', 'text-sage-700');
            });
            event.target.classList.remove('bg-sage-100', 'text-sage-700');
            event.target.classList.add('bg-sage-600', 'text-white');

            if (type === 'ALL') {
                renderTokenTable(initialTokens);
            } else {
                const filtered = initialTokens.filter(t => t.risk === type);
                renderTokenTable(filtered);
            }
        }

        // Inspect Specific Token (Navigates to Scanner)
        function inspectToken(ticker, address) {
            switchTab('scanner');
            document.getElementById('scanner-input').value = address;
            runDeepScan(ticker);
        }

        // Render Telegram Radar Feed
        function renderTelegramFeed() {
            const feedList = document.getElementById('telegram-feed-list');
            const radarSignals = document.getElementById('radar-signals-list');

            feedList.innerHTML = '';
            radarSignals.innerHTML = '';

            telegramFeeds.forEach(item => {
                // Telegram Card
                const card = document.createElement('div');
                card.className = "bg-white p-5 rounded-2xl border border-sage-200 shadow-sm flex flex-col sm:flex-row justify-between items-start gap-4";
                card.innerHTML = `
                    <div class="space-y-2 flex-1">
                        <div class="flex items-center space-x-2">
                            <i class="fa-paper-plane text-sage-500 text-sm"></i>
                            <span class="font-bold text-sage-900 text-sm">${item.group}</span>
                            <span class="text-xs text-sage-400">• ${item.members}</span>
                        </div>
                        <p class="text-xs font-mono bg-sage-50 p-3 rounded-xl border border-sage-100 text-sage-800">${item.msg}</p>
                        <span class="text-[11px] text-sage-500 block">${item.time}</span>
                    </div>
                    <div class="sm:text-right">
                        <span class="bg-rose-100 text-rose-800 text-xs px-2.5 py-1 rounded-full font-bold uppercase tracking-wider inline-block">${item.status}</span>
                    </div>
                `;
                feedList.appendChild(card);

                // Sidebar Radar Item
                const radarItem = document.createElement('div');
                radarItem.className = "p-3 rounded-xl bg-sage-50 border border-sage-100 text-xs space-y-1";
                radarItem.innerHTML = `
                    <div class="flex justify-between font-bold text-sage-900">
                        <span>${item.group}</span>
                        <span class="text-rose-600">Surge Alert</span>
                    </div>
                    <p class="text-[11px] text-sage-600 truncate">${item.msg}</p>
                `;
                radarSignals.appendChild(radarItem);
            });
        }

        // Interactive Token Risk Scanner Deep Audit
        function runDeepScan(tickerOverride) {
            const input = document.getElementById('scanner-input').value.trim() || "$PUMP";
            const scoreText = document.getElementById('score-text');
            const scoreCircle = document.getElementById('score-circle');
            const tokenName = document.getElementById('scanned-token-name');
            const verdictBadge = document.getElementById('verdict-badge');

            // Generate deterministic mock stats based on query length/characters
            const isHighRisk = input.toLowerCase().includes('pump') || input.toLowerCase().includes('doge') || input.length < 5;
            
            tokenName.innerText = tickerOverride || (input.startsWith('0x') ? `Token (${input.substring(0,6)}...)` : input.toUpperCase());
            
            let targetScore = isHighRisk ? 18 : 88;
            scoreText.innerText = targetScore;

            // Update Progress Circle Stroke
            const strokeVal = `${targetScore}, 100`;
            scoreCircle.setAttribute('stroke-dasharray', strokeVal);

            if (targetScore < 40) {
                scoreCircle.setAttribute('class', 'circle-progress text-rose-500');
                verdictBadge.className = 'inline-block bg-rose-100 text-rose-800 px-4 py-1.5 rounded-full text-xs font-bold uppercase tracking-wider animate-pulse';
                verdictBadge.innerText = 'HIGH PUMP & RUG RISK';

                document.getElementById('scan-lp-status').innerText = 'UNLOCKED / Dev Wallet';
                document.getElementById('scan-lp-status').className = 'text-lg font-bold text-rose-600 mt-1';
                document.getElementById('scan-lp-bar').style.width = '10%';
                document.getElementById('scan-lp-bar').className = 'bg-rose-500 h-full rounded-full';

                document.getElementById('scan-holder-conc').innerText = '82.5% in Top 5 Wallets';
                document.getElementById('scan-holder-bar').style.width = '82.5%';
                document.getElementById('scan-holder-bar').className = 'bg-rose-500 h-full rounded-full';

                showToast("High Risk Token Detected! Dev liquidity unlocked.", "warning");
            } else {
                scoreCircle.setAttribute('class', 'circle-progress text-emerald-500');
                verdictBadge.className = 'inline-block bg-emerald-100 text-emerald-800 px-4 py-1.5 rounded-full text-xs font-bold uppercase tracking-wider';
                verdictBadge.innerText = 'LOW PUMP RISK - VERIFIED';

                document.getElementById('scan-lp-status').innerText = 'Locked for 365 Days';
                document.getElementById('scan-lp-status').className = 'text-lg font-bold text-sage-900 mt-1';
                document.getElementById('scan-lp-bar').style.width = '95%';
                document.getElementById('scan-lp-bar').className = 'bg-emerald-500 h-full rounded-full';

                document.getElementById('scan-holder-conc').innerText = '18.4% Total Supply';
                document.getElementById('scan-holder-bar').style.width = '18.4%';
                document.getElementById('scan-holder-bar').className = 'bg-sage-400 h-full rounded-full';

                showToast("Audit completed. Token contract verified clean.", "success");
            }
        }

        // Live Telemetry Canvas Drawing
        function initTelemetryChart() {
            const canvas = document.getElementById('liveTelemetryCanvas');
            if (!canvas) return;
            const ctx = canvas.getContext('2d');

            function resizeCanvas() {
                canvas.width = canvas.parentElement.clientWidth;
                canvas.height = canvas.parentElement.clientHeight;
                drawChart();
            }
            window.addEventListener('resize', resizeCanvas);
            resizeCanvas();

            function drawChart() {
                const w = canvas.width;
                const h = canvas.height;
                ctx.clearRect(0, 0, w, h);

                // Draw Grid
                ctx.strokeStyle = '#E8EFEA';
                ctx.lineWidth = 1;
                for (let x = 0; x < w; x += 40) {
                    ctx.beginPath();
                    ctx.moveTo(x, 0);
                    ctx.lineTo(x, h);
                    ctx.stroke();
                }
                for (let y = 0; y < h; y += 35) {
                    ctx.beginPath();
                    ctx.moveTo(0, y);
                    ctx.lineTo(w, y);
                    ctx.stroke();
                }

                // Volume Surge Gradient Line (Red Spike)
                ctx.beginPath();
                ctx.moveTo(20, h - 30);
                ctx.lineTo(w * 0.3, h - 35);
                ctx.lineTo(w * 0.5, h - 45);
                ctx.lineTo(w * 0.75, h * 0.15); // Sudden Spike
                ctx.lineTo(w - 20, h * 0.75); // Steep Dump

                ctx.strokeStyle = '#E11D48'; // Rose Red
                ctx.lineWidth = 3;
                ctx.stroke();

                // Gradient Fill under spike
                const grad = ctx.createLinearGradient(0, 0, 0, h);
                grad.addColorStop(0, 'rgba(225, 29, 72, 0.25)');
                grad.addColorStop(1, 'rgba(225, 29, 72, 0.0)');
                ctx.lineTo(w - 20, h);
                ctx.lineTo(20, h);
                ctx.fillStyle = grad;
                ctx.fill();

                // Safe Organic Baseline Line (Sage Green)
                ctx.beginPath();
                ctx.moveTo(20, h - 25);
                ctx.lineTo(w * 0.3, h - 30);
                ctx.lineTo(w * 0.6, h - 40);
                ctx.lineTo(w - 20, h - 60);

                ctx.strokeStyle = '#87A96B'; // Sage
                ctx.lineWidth = 2;
                ctx.setLineDash([5, 5]);
                ctx.stroke();
                ctx.setLineDash([]);
            }
        }

        // Pump Simulator Logic & Animation
        let simInterval = null;
        function runPumpSimulation() {
            const canvas = document.getElementById('simCanvas');
            const ctx = canvas.getContext('2d');
            const initialAmt = parseFloat(document.getElementById('sim-amount').value) || 1000;
            const useProtection = document.getElementById('sim-aegis-toggle').checked;
            const severityPct = parseInt(document.getElementById('sim-severity').value) / 100;

            const statusText = document.getElementById('sim-status-text');
            const currentValText = document.getElementById('sim-current-val');
            const btn = document.getElementById('start-sim-btn');

            btn.disabled = true;
            btn.classList.add('opacity-50');

            let points = [];
            let frame = 0;
            const totalFrames = 100;

            if (simInterval) clearInterval(simInterval);

            simInterval = setInterval(() => {
                frame++;
                const w = canvas.width;
                const h = canvas.height;
                ctx.clearRect(0, 0, w, h);

                // Generate Price Curve (Exponential Pump then Instant Dump)
                let priceMultiplier = 1;
                if (frame < 60) {
                    priceMultiplier = 1 + Math.pow(frame / 20, 2.2); // Explosive pump
                } else if (frame < 70) {
                    // Insider Dump Phase
                    if (useProtection) {
                        priceMultiplier = 2.8; // Aegis triggers exit near top
                    } else {
                        priceMultiplier = 3.5 - ((frame - 60) * (3.5 * severityPct / 10)); // Total collapse
                    }
                } else {
                    priceMultiplier = useProtection ? 2.8 : Math.max(0.05, 3.5 * (1 - severityPct));
                }

                const currentPortfolio = initialAmt * priceMultiplier;
                currentValText.innerText = `$${currentPortfolio.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2})}`;

                // Status updates
                if (frame < 60) {
                    statusText.innerText = "🚀 Insiders artificially inflating price...";
                    statusText.className = "text-xs text-amber-600 font-bold animate-pulse";
                } else if (frame < 70) {
                    if (useProtection) {
                        statusText.innerText = "🛡️ Aegis Shield Auto-Stop Triggered! Liquidity exit secured.";
                        statusText.className = "text-xs text-emerald-600 font-bold";
                    } else {
                        statusText.innerText = "🚨 Dev Wallet Dump Executing! Liquidity Pulled!";
                        statusText.className = "text-xs text-rose-600 font-bold animate-bounce";
                    }
                }

                // Render Chart Line
                points.push({ x: (frame / totalFrames) * w, y: h - (priceMultiplier * (h / 5)) });
                
                ctx.beginPath();
                ctx.moveTo(points[0].x, points[0].y);
                for (let i = 1; i < points.length; i++) {
                    ctx.lineTo(points[i].x, points[i].y);
                }
                ctx.strokeStyle = (frame >= 60 && !useProtection) ? '#E11D48' : '#87A96B';
                ctx.lineWidth = 3;
                ctx.stroke();

                if (frame >= totalFrames) {
                    clearInterval(simInterval);
                    btn.disabled = false;
                    btn.classList.remove('opacity-50');

                    // Update comparison boxes
                    document.getElementById('res-initial').innerText = `$${initialAmt.toFixed(2)}`;
                    document.getElementById('res-unprotected').innerText = `$${(initialAmt * (1 - severityPct)).toFixed(2)} (-${(severityPct * 100).toFixed(0)}%)`;
                    document.getElementById('res-protected').innerText = `$${(initialAmt * 0.94).toFixed(2)} (-6.0%)`;

                    showToast(useProtection ? "Protection success! Aegis saved 94% of capital." : "Unprotected trade lost liquidity to dev dump.", useProtection ? "success" : "warning");
                }
            }, 30);
        }

        function resetSimulation() {
            if (simInterval) clearInterval(simInterval);
            const canvas = document.getElementById('simCanvas');
            const ctx = canvas.getContext('2d');
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            document.getElementById('sim-current-val').innerText = "$1,000.00";
            document.getElementById('sim-status-text').innerText = "Ready to run simulation...";
            document.getElementById('sim-status-text').className = "text-xs text-sage-500";
        }

        function initSimChart() {
            const canvas = document.getElementById('simCanvas');
            if (!canvas) return;
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;
        }

        // Timespan switch simulation
        function changeChartTimespan(span) {
            document.querySelectorAll('.chart-time-btn').forEach(b => {
                b.classList.remove('bg-sage-600', 'text-white');
                b.classList.add('bg-sage-50', 'text-sage-700');
            });
            event.target.classList.remove('bg-sage-50', 'text-sage-700');
            event.target.classList.add('bg-sage-600', 'text-white');
            initTelemetryChart();
        }

        // Live Block Height Simulation
        function startMockLiveBlockCounter() {
            let block = 19428102;
            setInterval(() => {
                block += Math.floor(Math.random() * 2) + 1;
                const el = document.getElementById('live-block-height');
                if (el) el.innerText = `Block #${block.toLocaleString()}`;
            }, 12000);
        }

        // Toast Notification Helper
        function showToast(message, type = "info") {
            const container = document.getElementById('toast-container');
            if (!container) return;

            const toast = document.createElement('div');
            toast.className = `pointer-events-auto p-4 rounded-xl shadow-lg border text-xs font-bold flex items-center justify-between space-x-3 transition-all duration-300 transform translate-y-2 opacity-0 ${
                type === 'warning' ? 'bg-rose-50 border-rose-200 text-rose-900' :
                type === 'success' ? 'bg-sage-100 border-sage-300 text-sage-900' : 'bg-white border-sage-200 text-sage-900'
            }`;

            toast.innerHTML = `
                <div class="flex items-center space-x-2">
                    <i class="fa-solid ${type === 'warning' ? 'fa-triangle-exclamation text-rose-600' : 'fa-shield-check text-sage-600'} text-base"></i>
                    <span>${message}</span>
                </div>
                <button onclick="this.parentElement.remove()" class="text-sage-400 hover:text-sage-700">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            `;

            container.appendChild(toast);
            setTimeout(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            }, 50);

            setTimeout(() => {
                if (toast.parentElement) {
                    toast.classList.add('opacity-0', 'translate-y-2');
                    setTimeout(() => toast.remove(), 300);
                }
            }, 5000);
        }

        function triggerSampleAlert() {
            showToast("ANOMALY: $PUMP pool dev moved 45 ETH to Binance", "warning");
        }

        function startRandomToastTrigger() {
            setTimeout(() => {
                showToast("Aegis Sentinel blocked honeypot contract buy for 0x39a...001", "success");
            }, 4000);
        }

        // FAQ Toggle
        function toggleFaq(element) {
            const p = element.querySelector('p');
            const icon = element.querySelector('i');
            p.classList.toggle('hidden');
            icon.classList.toggle('rotate-180');
        }
    </script>
</body>
</html>
