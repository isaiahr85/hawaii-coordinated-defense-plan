# hawaii-coordinated-defense-plan
A speculative AI-generated dashboard concept for Hawaiʻi Island’s defense plan (HI-DEFCON), designed to counter extremist threats like wildfires and sabotage. Features include threat briefings, vulnerability charts, and a risk-response matrix. Not affiliated with official agencies.
<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hawaiʻi Island Coordinated Defense Plan</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Chosen Palette: Dark theme with glowing red/amber accents -->
    <!-- Application Structure Plan: The application is designed as a multi-section, single-page dashboard. This structure was chosen over a linear document format to allow users (e.g., agency leads, first responders) to quickly access specific, mission-critical information. The user flow is non-linear, guided by a sticky navigation bar: Situation Dashboard (high-level overview), Threat Analysis (enemy details), Operational Plan (phased execution), Forces (personnel), and Resources (logistics). This thematic segmentation breaks down the complex plan into digestible modules. Key interactions, like tabbed content for operational phases and courses of action, prevent information overload and allow for focused analysis, making the dense report far more usable in a high-stakes environment. -->
    <!-- Visualization & Content Choices: 
         - Report Info: Key Risks & Mitigations -> Goal: Organize/Inform -> Viz/Presentation: HTML/CSS Grid -> Interaction: Static view -> Justification: Provides a clear, at-a-glance matrix of threats and responses, superior to a simple list. -> Library/Method: HTML/Tailwind.
         - Report Info: Island Vulnerabilities (WUI, Comms, Power) -> Goal: Compare/Inform -> Viz/Presentation: Bar Chart -> Interaction: Hover tooltips for context -> Justification: Visually quantifies and prioritizes abstract vulnerabilities mentioned in the text. -> Library/Method: Chart.js.
         - Report Info: Enemy Capabilities (Arson, Sabotage, Info Ops) -> Goal: Compare -> Viz/Presentation: Radar Chart -> Interaction: Hover tooltips -> Justification: Effectively displays the multi-faceted nature of the adversary's threat profile in a single graphic. -> Library/Method: Chart.js.
         - Report Info: Multi-phase operational plan & Courses of Action -> Goal: Organize/Navigate -> Viz/Presentation: Interactive Tabbed Sections -> Interaction: Click to reveal details -> Justification: Manages complexity and prevents a long wall of text, allowing users to focus on one phase/COA at a time. -> Library/Method: Vanilla JS.
         - Report Info: Friendly Forces Structure -> Goal: Organize -> Viz/Presentation: Hierarchical Diagram -> Interaction: Static view -> Justification: Clarifies command structure and agency roles more effectively than a list. -> Library/Method: HTML/Tailwind. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #111827; /* Dark Gray-900 */
            background-image: url('https://images.unsplash.com/photo-1593179449458-e455ab18b62b?q=80&w=2940&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            color: #E5E7EB; /* Gray-200 */
        }
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(17, 24, 39, 0.6);
            z-index: -1;
        }
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&family=Roboto+Slab:wght@700&display=swap');
        h1, h2, h3 {
            font-family: 'Roboto Slab', serif;
            color: #F87171; /* Red-400 */
        }
        .nav-link {
            transition: color 0.3s, border-bottom-color 0.3s;
            border-bottom: 2px solid transparent;
        }
        .nav-link.active, .nav-link:hover {
            color: #FBBF24; /* Amber-400 */
            border-bottom-color: #FBBF24;
        }
        .mobile-nav-link.active {
            background-color: #4B5563; /* Gray-600 */
            color: #FBBF24; /* Amber-400 */
        }
        .tab-button {
            transition: background-color 0.3s, color 0.3s;
            background-color: #374151; /* Gray-700 */
            color: #D1D5DB; /* Gray-300 */
        }
        .tab-button:hover {
            background-color: #4B5563; /* Gray-600 */
        }
        .tab-button.active {
            background-color: #DC2626; /* Red-600 */
            color: #FFFFFF;
            box-shadow: 0 0 10px rgba(239, 68, 68, 0.7);
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        .content-card {
            background-color: rgba(31, 41, 55, 0.8); /* Gray-800 with opacity */
            backdrop-filter: blur(10px);
            border: 1px solid rgba(75, 85, 99, 0.5); /* Gray-600 with opacity */
        }
        
        .loader {
            border-top-color: #8B5CF6; /* Violet-500 */
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        .modal-overlay {
            background-color: rgba(17, 24, 39, 0.8);
            backdrop-filter: blur(5px);
        }

        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }
    </style>
</head>
<body class="antialiased">

    <header class="bg-gray-900/80 backdrop-blur-md shadow-lg shadow-red-900/20 sticky top-0 z-50 border-b border-gray-700">
        <nav class="container mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <div class="flex items-center">
                    <div class="flex-shrink-0">
                        <h1 class="text-xl font-bold text-red-500">HI-DEFCON</h1>
                    </div>
                    <div class="hidden md:block">
                        <div class="ml-10 flex items-baseline space-x-4">
                            <a href="#dashboard" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300">Dashboard</a>
                            <a href="#threat" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300">Threat Analysis</a>
                            <a href="#plan" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300">Operational Plan</a>
                            <a href="#forces" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300">Forces</a>
                             <a href="#signal" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300">Command & Signal</a>
                            <a href="#resources" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300">Resources</a>
                        </div>
                    </div>
                </div>
                <div class="hidden md:block text-right">
                    <p id="current-time-header" class="text-sm font-medium text-gray-300">Loading...</p>
                    <p class="text-xs text-gray-400">Orchidlands Estates, HI</p>
                </div>
                <div class="-mr-2 flex md:hidden">
                    <!-- Mobile menu button -->
                    <button type="button" id="mobile-menu-button" class="bg-gray-800 inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-white hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-800 focus:ring-red-500" aria-controls="mobile-menu" aria-expanded="false">
                        <span class="sr-only">Open main menu</span>
                        <svg id="hamburger-icon" class="block h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                        </svg>
                        <svg id="close-icon" class="hidden h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                </div>
            </div>
             <!-- Mobile menu, show/hide based on menu state. -->
            <div class="md:hidden hidden" id="mobile-menu">
                <div class="px-2 pt-2 pb-3 space-y-1 sm:px-3">
                    <a href="#dashboard" class="mobile-nav-link block px-3 py-2 rounded-md text-base font-medium text-gray-300">Dashboard</a>
                    <a href="#threat" class="mobile-nav-link block px-3 py-2 rounded-md text-base font-medium text-gray-300">Threat Analysis</a>
                    <a href="#plan" class="mobile-nav-link block px-3 py-2 rounded-md text-base font-medium text-gray-300">Operational Plan</a>
                    <a href="#forces" class="mobile-nav-link block px-3 py-2 rounded-md text-base font-medium text-gray-300">Forces</a>
                    <a href="#signal" class="mobile-nav-link block px-3 py-2 rounded-md text-base font-medium text-gray-300">Command & Signal</a>
                    <a href="#resources" class="mobile-nav-link block px-3 py-2 rounded-md text-base font-medium text-gray-300">Resources</a>
                </div>
            </div>
        </nav>
    </header>

    <main class="container mx-auto p-4 sm:p-6 lg:p-8">
        
        <section id="disclosure" class="mb-12">
            <div class="bg-yellow-900/70 border-2 border-yellow-500 text-yellow-200 p-6 rounded-lg shadow-lg shadow-yellow-500/20" role="alert">
                <h3 class="text-xl font-bold text-yellow-300">Disclosure Statement</h3>
                <p class="mt-2 text-yellow-200">This proposal is a conceptual work generated with the assistance of artificial intelligence and reflects the independent ideas and research of the author. It does not represent, imply, or suggest any formal collaboration, endorsement, or partnership with the Hawaiʻi Fire Department, Hawaiʻi first responders, or any agency or entity of the State of Hawaiʻi. All references to fire prevention strategies, technological applications, or emergency response planning are speculative and intended solely for illustrative or exploratory purposes. Any resemblance to actual programs, policies, or initiatives is coincidental.</p>
            </div>
        </section>

        <section id="dashboard" class="pt-16 -mt-16 mb-12">
            <div class="text-center mb-8">
                <h2 class="text-3xl font-bold text-red-400">Hawaiʻi Island Coordinated Defense Plan</h2>
                <p class="mt-2 text-lg text-gray-300">An interactive overview of the joint agency response to extremist threats.</p>
            </div>
            
            <div class="bg-red-900/60 border-l-4 border-red-500 text-red-200 p-4 rounded-r-lg mb-8 content-card" role="alert">
                <p class="font-bold text-red-300">Mission Statement</p>
                <p>Detect, deter, and defeat extremist-initiated wildfire and infrastructure sabotage on Hawaiʻi Island to preserve life, protect critical infrastructure, maintain public order, and sustain community trust.</p>
            </div>

            <!-- Enhanced AI Threat Briefing Section -->
            <div class="content-card p-6 rounded-lg shadow-md mb-8">
                <h3 class="text-2xl font-bold text-violet-400 mb-4 text-center">AI Real-Time Threat Briefing</h3>
                <div class="text-center mb-6">
                    <button id="generate-briefing-btn" class="bg-violet-600 hover:bg-violet-700 text-white font-bold py-3 px-6 rounded-lg shadow-lg transition-transform transform hover:scale-105 focus:outline-none focus:ring-4 focus:ring-violet-500 focus:ring-opacity-50">
                        ✨ Generate Briefing from Verified Data
                    </button>
                    <div class="mt-4">
                         <button id="show-policy-btn" class="text-sm text-gray-400 hover:text-white underline focus:outline-none focus:ring-2 focus:ring-gray-500 rounded">
                            Data & API Usage Disclosure
                        </button>
                    </div>
                </div>
                 <!-- Loading Indicator -->
                <div id="loader" class="text-center hidden my-6">
                    <div class="loader ease-linear rounded-full border-4 border-t-4 border-gray-600 h-12 w-12 mx-auto mb-4"></div>
                    <p class="text-lg text-gray-400">Fetching real-time data and generating AI analysis... Please wait.</p>
                </div>
                 <!-- Error Message Display -->
                <div id="error-box" class="hidden bg-red-900/80 border border-red-700 text-red-200 px-4 py-3 rounded-lg relative my-4" role="alert">
                    <strong class="font-bold">Error:</strong>
                    <span class="block sm:inline" id="error-message"></span>
                </div>
                 <!-- Results Container -->
                <div id="results-container" class="space-y-6 hidden">
                    <!-- AI Briefing Output -->
                    <div>
                        <h4 class="text-xl font-semibold text-white border-b-2 border-violet-500 pb-2 mb-4">Threat Assessment</h4>
                        <div id="briefing-output" class="bg-gray-900/50 p-4 rounded-lg prose prose-invert max-w-none prose-p:text-gray-300 prose-strong:text-white">
                            <p>Generating briefing...</p>
                        </div>
                    </div>
                    <!-- Raw Data Sources -->
                    <div>
                        <details class="bg-gray-900/50 rounded-lg overflow-hidden border border-gray-700">
                            <summary class="cursor-pointer p-4 text-lg font-semibold text-white hover:bg-gray-700/50">
                                View Raw Data Sources
                            </summary>
                            <div id="raw-data-output" class="p-4 border-t border-gray-700 space-y-4">
                                <p>Fetching data...</p>
                            </div>
                        </details>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-8">
                <div class="p-6 rounded-lg shadow-md flex flex-col justify-between content-card">
                    <h3 class="font-bold text-gray-300 mb-2">Current Op Phase</h3>
                    <p class="text-3xl font-bold text-amber-400">Phase 0</p>
                    <p class="text-sm text-gray-400">Prevention & Posture</p>
                </div>
                <div class="p-6 rounded-lg shadow-md flex flex-col justify-between content-card">
                    <h3 class="font-bold text-gray-300 mb-2">Threat Condition</h3>
                    <p class="text-3xl font-bold text-cyan-400">Normal</p>
                    <p class="text-sm text-gray-400">No Red-Flag Conditions</p>
                </div>
                <div class="p-6 rounded-lg shadow-md flex flex-col justify-between content-card">
                    <h3 class="font-bold text-gray-300 mb-2">End State Goal</h3>
                    <p class="text-3xl font-bold text-blue-400">Secure</p>
                    <p class="text-sm text-gray-400">No successful attacks; public confidence maintained.</p>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="p-6 rounded-lg shadow-md content-card">
                    <h3 class="text-xl font-bold text-red-400 mb-4 text-center">Island Vulnerability Hotspots</h3>
                    <p class="text-center text-sm text-gray-400 mb-4">This chart highlights the primary areas of vulnerability across the island that require hardening and increased vigilance. These are the weak points the adversary is most likely to exploit.</p>
                    <div class="chart-container">
                        <canvas id="vulnerabilityChart"></canvas>
                    </div>
                </div>
                <div class="p-6 rounded-lg shadow-md content-card">
                    <h3 class="text-xl font-bold text-red-400 mb-4 text-center">Risk & Mitigation Matrix</h3>
                     <p class="text-center text-sm text-gray-400 mb-4">The following matrix outlines the most critical operational risks and the corresponding mitigation strategies developed to counter them, ensuring response continuity and effectiveness.</p>
                    <div class="space-y-4">
                        <div class="p-4 bg-red-900/50 rounded-lg border border-red-700">
                            <p class="font-semibold text-red-300">Risk: Multi-point ignitions overwhelm resources.</p>
                            <p class="text-sm text-red-200"><strong>Mitigation:</strong> Pre-stage assets, activate mutual aid compacts, dynamic tasking via Common Operating Picture.</p>
                        </div>
                        <div class="p-4 bg-yellow-900/50 rounded-lg border border-yellow-700">
                            <p class="font-semibold text-yellow-300">Risk: Comms blackout cripples response.</p>
                            <p class="text-sm text-yellow-200"><strong>Mitigation:</strong> Deploy portable nodes (SATCOM, repeaters), and utilize trained radio operators.</p>
                        </div>
                        <div class="p-4 bg-blue-900/50 rounded-lg border border-blue-700">
                            <p class="font-semibold text-blue-300">Risk: Public panic from disinformation.</p>
                            <p class="text-sm text-blue-200"><strong>Mitigation:</strong> Rapid, verified messaging through a single channel and a dedicated rumor control team.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="threat" class="pt-20 -mt-16 mb-12">
            <h2 class="text-3xl font-bold text-red-400 text-center mb-8">Threat Analysis: "Anti-Noah's Ark" Coalition</h2>
            <p class="text-center max-w-3xl mx-auto text-gray-300 mb-8">This section details the adversary, a locally-based, ideologically motivated extremist group. Understanding their composition, capabilities, and probable courses of action is critical for effective counter-operations.</p>
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 items-start">
                <div class="p-6 rounded-lg shadow-md content-card">
                    <h3 class="text-xl font-bold text-red-400 mb-4">Adversary Profile</h3>
                    <ul class="space-y-3 text-gray-300">
                        <li><strong>Identity:</strong> Loosely organized, socially isolated cells.</li>
                        <li><strong>Composition:</strong> Small, decentralized, no formal command structure.</li>
                        <li><strong>Ideology:</strong> Rejects community authority; seeks to "remove happiness" from perceived aggressors.</li>
                        <li><strong>Limitations:</strong> Limited logistics and sustainment capacity; vulnerable to community tip-offs.</li>
                    </ul>
                     <h3 class="text-xl font-bold text-red-400 mt-6 mb-4">Adversary Capabilities</h3>
                    <div class="chart-container h-80 md:h-96">
                        <canvas id="capabilitiesChart"></canvas>
                    </div>
                </div>
                <div class="p-6 rounded-lg shadow-md 
