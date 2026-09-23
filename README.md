<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tripzy AI â€” Plan Less. Travel More.</title>
  
  <!-- Tailwind CSS & Lucide Icons CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest"></script>
  
  <!-- Leaflet CSS & JS -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <!-- Google Fonts: Inter & Outfit -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Outfit:wght@500;600;700;800&display=swap" rel="stylesheet">

  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            brand: {
              50: '#fff7ed',
              100: '#ffedd5',
              400: '#fb923c',
              500: '#f97316', // Pitch deck signature orange
              600: '#ea580c',
              700: '#c2410c',
            },
            deck: {
              bg: '#070D1E',       // Pitch deck deep navy background
              card: '#0F1A34',     // Dark container surface
              cardBorder: '#1E2D52', // Border accent
              subtle: '#94A3B8'
            }
          },
          fontFamily: {
            sans: ['Inter', 'sans-serif'],
            display: ['Outfit', 'sans-serif'],
          }
        }
      }
    }
  </script>

  <style>
    body {
      background-color: #070D1E;
      color: #F8FAFC;
      font-family: 'Inter', sans-serif;
    }
    .glass-panel {
      background: rgba(15, 26, 52, 0.75);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border: 1px solid rgba(255, 255, 255, 0.08);
    }
    .glass-panel-orange {
      background: linear-gradient(135deg, rgba(249, 115, 22, 0.12), rgba(15, 26, 52, 0.8));
      border: 1px solid rgba(249, 115, 22, 0.3);
    }
    #trip-map {
      height: 380px;
      width: 100%;
      border-radius: 1rem;
      z-index: 10;
    }
    /* Custom scrollbars */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #070D1E;
    }
    ::-webkit-scrollbar-thumb {
      background: #1E2D52;
      border-radius: 4px;
    }
    @media print {
      body {
        background: #ffffff !important;
        color: #000000 !important;
      }
      .no-print {
        display: none !important;
      }
      .print-only {
        display: block !important;
      }
      .glass-panel {
        background: #ffffff !important;
        color: #000000 !important;
        border: 1px solid #e2e8f0 !important;
      }
    }
  </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-brand-500 selection:text-white">

  <!-- ================= TOP NAVIGATION ================= -->
  <header class="sticky top-0 z-40 border-b border-deck-cardBorder/60 bg-deck-bg/85 backdrop-blur-md no-print">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
      <div class="flex items-center space-x-3">
        <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-brand-600 to-amber-400 flex items-center justify-center shadow-lg shadow-brand-500/25">
          <i data-lucide="compass" class="w-6 h-6 text-white stroke-[2.2]"></i>
        </div>
        <div>
          <div class="flex items-center space-x-2">
            <span class="font-display font-extrabold text-2xl tracking-tight text-white">Tripzy<span class="text-brand-500">.AI</span></span>
            <span class="text-[10px] uppercase font-bold tracking-wider px-2 py-0.5 rounded-full bg-brand-500/20 text-brand-400 border border-brand-500/30">AI Manthon 2k26</span>
          </div>
          <p class="text-[11px] text-gray-400 leading-none">Plan less. Travel more.</p>
        </div>
      </div>

      <nav class="hidden md:flex items-center space-x-8 text-sm font-medium text-gray-300">
        <a href="#planner" class="hover:text-brand-400 transition-colors">AI Planner</a>
        <a href="#deck-comparison" class="hover:text-brand-400 transition-colors">Problem & Solution</a>
        <a href="#budget-matrix" class="hover:text-brand-400 transition-colors">Dynamic Budget</a>
        <a href="#partner-ecosystem" class="hover:text-brand-400 transition-colors">Partners</a>
        <a href="#business-model" class="hover:text-brand-400 transition-colors">Economics</a>
      </nav>

      <div class="flex items-center space-x-3">
        <button onclick="toggleConfigModal()" class="px-3.5 py-1.5 rounded-lg border border-deck-cardBorder bg-deck-card hover:bg-deck-cardBorder text-xs text-gray-300 flex items-center space-x-1.5 transition">
          <i data-lucide="key" class="w-3.5 h-3.5 text-brand-400"></i>
          <span>Gemini Key</span>
        </button>
        <a href="#planner" class="px-4 py-2 rounded-xl bg-brand-500 hover:bg-brand-600 text-white text-sm font-semibold shadow-lg shadow-brand-500/30 transition transform active:scale-95">
          Plan My Trip
        </a>
      </div>
    </div>
  </header>

  <!-- ================= HERO SECTION (Pitch Deck Slide 01) ================= -->
  <section class="relative pt-12 pb-14 px-4 sm:px-6 lg:px-8 overflow-hidden no-print">
    <div class="absolute inset-0 pointer-events-none -z-10 flex items-center justify-center">
      <div class="w-[600px] h-[600px] bg-brand-500/10 rounded-full blur-[140px] opacity-60"></div>
      <div class="w-[400px] h-[400px] bg-blue-500/10 rounded-full blur-[120px] opacity-40"></div>
    </div>

    <div class="max-w-4xl mx-auto text-center">
      <div class="inline-flex items-center space-x-2 px-3.5 py-1.5 rounded-full bg-deck-card border border-deck-cardBorder mb-6">
        <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
        <span class="text-xs font-medium text-gray-300">Live Hackathon Prototype: Slide 01 to Slide 10 Live Engine</span>
      </div>
      <h1 class="text-4xl sm:text-5xl lg:text-6xl font-display font-extrabold tracking-tight text-white leading-tight">
        Planning a <span class="text-brand-500 underline decoration-brand-500/40 decoration-wavy">â‚¹8,000</span> trip shouldnâ€™t feel like a project.
      </h1>
      <p class="mt-5 text-lg sm:text-xl text-gray-300 max-w-2xl mx-auto font-light leading-relaxed">
        Tripzy AI turns endless tabs, group chats, confusing routes, and price changes into one single guided, verified plan.
      </p>

      <!-- Quick Metrics from Slide 03 -->
      <div class="mt-8 grid grid-cols-3 gap-4 max-w-xl mx-auto pt-4 border-t border-deck-cardBorder/50">
        <div class="text-center">
          <div class="text-2xl sm:text-3xl font-bold font-display text-brand-400">89%</div>
          <div class="text-[11px] sm:text-xs text-gray-400 mt-1">Want AI in travel</div>
        </div>
        <div class="text-center border-x border-deck-cardBorder/50">
          <div class="text-2xl sm:text-3xl font-bold font-display text-sky-400">67%</div>
          <div class="text-[11px] sm:text-xs text-gray-400 mt-1">Already used AI tools</div>
        </div>
        <div class="text-center">
          <div class="text-2xl sm:text-3xl font-bold font-display text-emerald-400">1-Click</div>
          <div class="text-[11px] sm:text-xs text-gray-400 mt-1">Dynamic Re-planning</div>
        </div>
      </div>
    </div>
  </section>

  <!-- ================= MAIN TRIP PLANNER & ENGINE ================= -->
  <main id="planner" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 pb-16 w-full">
    
    <!-- Planner Configuration Bar -->
    <div class="glass-panel p-6 sm:p-8 rounded-2xl border border-deck-cardBorder shadow-2xl relative overflow-hidden mb-8 no-print">
      <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-6 pb-6 border-b border-deck-cardBorder/70">
        <div>
          <div class="text-xs uppercase font-bold tracking-wider text-brand-400">Intelligent Input Layer</div>
          <h2 class="text-2xl font-bold text-white mt-1">Set Your Trip Parameters</h2>
        </div>
        <!-- Quick Budget Presets (From Slide 04 & 06) -->
        <div class="flex items-center space-x-2 flex-wrap gap-y-2">
          <span class="text-xs text-gray-400 mr-1">Pitch Deck Presets:</span>
          <button onclick="setBudgetPreset(8000)" class="px-3 py-1.5 rounded-lg bg-brand-500/20 text-brand-400 hover:bg-brand-500 hover:text-white border border-brand-500/40 text-xs font-semibold transition">
            â‚¹8,000 (Slide 04 Demo)
          </button>
          <button onclick="setBudgetPreset(6000)" class="px-3 py-1.5 rounded-lg bg-deck-card hover:border-brand-500/50 border border-deck-cardBorder text-xs text-gray-300 font-semibold transition">
            â‚¹6,000 (Slide 06 Re-budget)
          </button>
          <button onclick="setBudgetPreset(12000)" class="px-3 py-1.5 rounded-lg bg-deck-card hover:border-brand-500/50 border border-deck-cardBorder text-xs text-gray-300 font-semibold transition">
            â‚¹12,000 (Comfort Tier)
          </button>
        </div>
      </div>

      <!-- Input Form -->
      <form id="trip-form" onsubmit="handleGeneratePlan(event)" class="mt-6 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-5">
        
        <!-- Origin -->
        <div>
          <label class="block text-xs font-semibold text-gray-300 uppercase tracking-wider mb-2">Starting Point</label>
          <div class="relative">
            <i data-lucide="map-pin" class="w-4 h-4 absolute left-3.5 top-3.5 text-gray-400"></i>
            <select id="origin-select" class="w-full bg-deck-card/90 border border-deck-cardBorder rounded-xl pl-10 pr-3 py-2.5 text-sm text-white focus:outline-none focus:border-brand-500 transition">
              <option value="Lucknow" selected>Lucknow (Origin in Deck)</option>
              <option value="Delhi">New Delhi</option>
              <option value="Kanpur">Kanpur</option>
              <option value="Mumbai">Mumbai</option>
              <option value="Chandigarh">Chandigarh</option>
            </select>
          </div>
        </div>

        <!-- Destination -->
        <div>
          <label class="block text-xs font-semibold text-gray-300 uppercase tracking-wider mb-2">Destination Cluster</label>
          <div class="relative">
            <i data-lucide="mountain-snow" class="w-4 h-4 absolute left-3.5 top-3.5 text-brand-400"></i>
            <select id="destination-select" onchange="handleDestinationChange()" class="w-full bg-deck-card/90 border border-deck-cardBorder rounded-xl pl-10 pr-3 py-2.5 text-sm text-white focus:outline-none focus:border-brand-500 transition">
              <option value="Uttarakhand" selected>Uttarakhand (Rishikesh / Mussoorie)</option>
              <option value="Himachal">Himachal Pradesh (Manali / Kasol)</option>
              <option value="Rajasthan">Rajasthan (Jaipur / Pushkar)</option>
              <option value="Goa">Goa (Beaches & Cafes)</option>
              <option value="NorthEast">North-East (Meghalaya / Shillong)</option>
            </select>
          </div>
        </div>

        <!-- Budget Slider Input -->
        <div>
          <div class="flex justify-between items-center mb-2">
            <label class="text-xs font-semibold text-gray-300 uppercase tracking-wider">Total Budget</label>
            <span id="budget-display" class="text-sm font-bold text-brand-400">â‚¹8,000</span>
          </div>
          <div class="pt-1">
            <input 
              type="range" 
              id="budget-slider" 
              min="3000" 
              max="25000" 
              step="500" 
              value="8000" 
              oninput="handleBudgetSlider(this.value)" 
              class="w-full h-2 bg-deck-cardBorder rounded-lg appearance-none cursor-pointer accent-brand-500"
            />
            <div class="flex justify-between text-[10px] text-gray-500 mt-1 font-mono">
              <span>â‚¹3k (Hostel)</span>
              <span>â‚¹8k (Deck)</span>
              <span>â‚¹25k (Luxury)</span>
            </div>
          </div>
        </div>

        <!-- Travelers & Days -->
        <div class="grid grid-cols-2 gap-2">
          <div>
            <label class="block text-xs font-semibold text-gray-300 uppercase tracking-wider mb-2">Travelers</label>
            <select id="travelers-select" class="w-full bg-deck-card/90 border border-deck-cardBorder rounded-xl px-3 py-2.5 text-sm text-white focus:outline-none focus:border-brand-500 transition">
              <option value="1">Solo</option>
              <option value="2" selected>2 Friends</option>
              <option value="3">3 Friends</option>
              <option value="4">4 Friends</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-300 uppercase tracking-wider mb-2">Trip Length</label>
            <select id="duration-select" class="w-full bg-deck-card/90 border border-deck-cardBorder rounded-xl px-3 py-2.5 text-sm text-white focus:outline-none focus:border-brand-500 transition">
              <option value="3">3 Days</option>
              <option value="4" selected>4 Days (Deck)</option>
              <option value="5">5 Days</option>
            </select>
          </div>
        </div>

        <!-- Submit Button -->
        <div class="flex items-end">
          <button 
            type="submit" 
            id="analyse-btn"
            class="w-full py-2.5 px-4 rounded-xl bg-gradient-to-r from-brand-500 to-amber-500 hover:from-brand-600 hover:to-amber-600 text-white font-bold text-sm shadow-lg shadow-brand-500/25 flex items-center justify-center space-x-2 transition transform active:scale-95">
            <i data-lucide="sparkles" class="w-4 h-4"></i>
            <span>Analyse & Plan</span>
          </button>
        </div>
      </form>

      <!-- Multi-Agent Loading Pipeline (Slide 05: LLM + Travel Data + Budget Checks) -->
      <div id="ai-loading-stepper" class="mt-6 hidden pt-4 border-t border-deck-cardBorder/60">
        <div class="flex items-center justify-between text-xs text-gray-300 mb-2">
          <span id="agent-current-status" class="font-medium text-brand-400 flex items-center space-x-2">
            <span class="w-2 h-2 rounded-full bg-brand-400 animate-ping"></span>
            <span>Agent 1: LLM understanding intent & style constraints...</span>
          </span>
          <span id="agent-percent" class="font-mono text-gray-400">25%</span>
        </div>
        <div class="w-full bg-deck-card rounded-full h-2 overflow-hidden border border-deck-cardBorder">
          <div id="agent-progress-bar" class="bg-gradient-to-r from-brand-500 to-sky-400 h-2 rounded-full transition-all duration-300" style="width: 25%"></div>
        </div>
      </div>
    </div>

    <!-- ================= PLANNER RESULTS CONTAINER ================= -->
    <div id="plan-results" class="grid grid-cols-1 lg:grid-cols-12 gap-8">
      
      <!-- LEFT COLUMN: Trip Summary, Budget Breakdown, Dynamic Replan & Group Split (7 Cols) -->
      <div class="lg:col-span-7 space-y-6">

        <!-- Top Route Badge & Live Disruption Switch -->
        <div class="glass-panel p-5 rounded-2xl flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4">
          <div>
            <div class="flex items-center space-x-2">
              <span class="text-xs uppercase font-extrabold tracking-wider text-brand-400">Active Itinerary</span>
              <span class="text-[10px] bg-emerald-500/10 text-emerald-400 px-2 py-0.5 rounded-full border border-emerald-500/20 font-medium">100% Feasible</span>
            </div>
            <h3 id="trip-header-title" class="text-xl font-bold text-white mt-0.5">Lucknow â†’ Uttarakhand (4 Days, 2 Friends)</h3>
            <p id="trip-header-subtitle" class="text-xs text-gray-400">Optimized route with Nature + 1 Adventure focus</p>
          </div>
          
          <div class="flex items-center space-x-2 w-full sm:w-auto justify-end">
            <!-- Trigger Rain / Disruption (Slide 05 Dynamic Re-plan) -->
            <button 
              id="replan-weather-btn" 
              onclick="toggleWeatherDisruption()" 
              class="px-3 py-2 rounded-xl bg-blue-500/10 hover:bg-blue-500/20 border border-blue-500/30 text-xs font-semibold text-blue-300 flex items-center space-x-1.5 transition">
              <i data-lucide="cloud-rain" class="w-3.5 h-3.5"></i>
              <span id="replan-btn-text">Simulate Heavy Rain</span>
            </button>
            <button 
              onclick="window.print()" 
              class="px-3 py-2 rounded-xl bg-deck-card hover:bg-deck-cardBorder border border-deck-cardBorder text-xs font-semibold text-gray-300 flex items-center space-x-1.5 transition"
              title="Print / Export Itinerary PDF">
              <i data-lucide="printer" class="w-3.5 h-3.5"></i>
              <span>Export</span>
            </button>
          </div>
        </div>

        <!-- Slide 06 Exact Mirror: Live Dynamic Budget Allocation Matrix -->
        <div id="budget-matrix" class="glass-panel p-6 rounded-2xl border border-deck-cardBorder relative">
          <div class="flex items-center justify-between mb-4">
            <div>
              <span class="text-[11px] font-bold uppercase tracking-wider text-brand-400">Slide 06 Demo Architecture</span>
              <h4 class="text-base font-bold text-white">Dynamic Budget Matrix</h4>
            </div>
            <div class="text-right">
              <div class="text-xs text-gray-400">Total Allocated:</div>
              <div id="budget-total-pill" class="text-lg font-extrabold text-brand-400 font-mono">â‚¹8,000</div>
            </div>
          </div>

          <!-- Dynamic Budget Progress Bars -->
          <div class="space-y-3.5">
            <!-- Transport -->
            <div>
              <div class="flex justify-between text-xs mb-1">
                <span class="text-gray-300 flex items-center"><i data-lucide="train" class="w-3.5 h-3.5 mr-1.5 text-sky-400"></i> Transport (Train / Bus)</span>
                <span id="alloc-transport-amt" class="font-semibold text-white font-mono">â‚¹2,400</span>
              </div>
              <div class="w-full bg-deck-card rounded-full h-2 overflow-hidden">
                <div id="alloc-transport-bar" class="bg-sky-400 h-2 rounded-full transition-all duration-500" style="width: 30%"></div>
              </div>
              <span id="alloc-transport-mode" class="text-[11px] text-gray-400 italic">AC 3-Tier Express or Overnight Volvo</span>
            </div>

            <!-- Stay -->
            <div>
              <div class="flex justify-between text-xs mb-1">
                <span class="text-gray-300 flex items-center"><i data-lucide="hotel" class="w-3.5 h-3.5 mr-1.5 text-amber-400"></i> Stay (Hostels / Homestays)</span>
                <span id="alloc-stay-amt" class="font-semibold text-white font-mono">â‚¹2,000</span>
              </div>
              <div class="w-full bg-deck-card rounded-full h-2 overflow-hidden">
                <div id="alloc-stay-bar" class="bg-amber-400 h-2 rounded-full transition-all duration-500" style="width: 25%"></div>
              </div>
              <span id="alloc-stay-mode" class="text-[11px] text-gray-400 italic">3 Nights: Riverview Hostel Dorm / Budget Homestay</span>
            </div>

            <!-- Food -->
            <div>
              <div class="flex justify-between text-xs mb-1">
                <span class="text-gray-300 flex items-center"><i data-lucide="utensils" class="w-3.5 h-3.5 mr-1.5 text-emerald-400"></i> Food & Dining</span>
                <span id="alloc-food-amt" class="font-semibold text-white font-mono">â‚¹1,600</span>
              </div>
              <div class="w-full bg-deck-card rounded-full h-2 overflow-hidden">
                <div id="alloc-food-bar" class="bg-emerald-400 h-2 rounded-full transition-all duration-500" style="width: 20%"></div>
              </div>
              <span id="alloc-food-mode" class="text-[11px] text-gray-400 italic">Local mountain cafes, Dhabas & breakfast thalis</span>
            </div>

            <!-- Activities -->
            <div>
              <div class="flex justify-between text-xs mb-1">
                <span class="text-gray-300 flex items-center"><i data-lucide="compass" class="w-3.5 h-3.5 mr-1.5 text-purple-400"></i> Activities + Local Travel</span>
                <span id="alloc-activity-amt" class="font-semibold text-white font-mono">â‚¹1,600</span>
              </div>
              <div class="w-full bg-deck-card rounded-full h-2 overflow-hidden">
                <div id="alloc-activity-bar" class="bg-purple-400 h-2 rounded-full transition-all duration-500" style="width: 20%"></div>
              </div>
              <span id="alloc-activity-mode" class="text-[11px] text-gray-400 italic">Rishikesh 16km White Water Rafting + Scooty rental</span>
            </div>

            <!-- Emergency Buffer -->
            <div>
              <div class="flex justify-between text-xs mb-1">
                <span class="text-gray-300 flex items-center"><i data-lucide="shield-check" class="w-3.5 h-3.5 mr-1.5 text-brand-400"></i> Emergency Buffer</span>
                <span id="alloc-buffer-amt" class="font-semibold text-white font-mono">â‚¹400</span>
              </div>
              <div class="w-full bg-deck-card rounded-full h-2 overflow-hidden">
                <div id="alloc-buffer-bar" class="bg-brand-500 h-2 rounded-full transition-all duration-500" style="width: 5%"></div>
              </div>
              <span class="text-[11px] text-gray-400 italic">First aid, local rickshaws, and incidental expenses</span>
            </div>
          </div>
        </div>

        <!-- Multi-Day Itinerary with Dynamic Day Tabs -->
        <div class="glass-panel p-6 rounded-2xl border border-deck-cardBorder">
          <div class="flex flex-col sm:flex-row sm:items-center justify-between pb-4 border-b border-deck-cardBorder gap-3">
            <div>
              <h4 class="text-base font-bold text-white">Day-by-Day Action Plan</h4>
              <p class="text-xs text-gray-400">Strictly mapped to budget and distance constraints</p>
            </div>
            
            <!-- Day Switcher Tabs -->
            <div id="day-tabs-container" class="flex space-x-1.5 bg-deck-card p-1 rounded-xl border border-deck-cardBorder">
              <button onclick="renderDayContent(1)" id="day-tab-1" class="px-3 py-1 rounded-lg text-xs font-semibold bg-brand-500 text-white transition">Day 1</button>
              <button onclick="renderDayContent(2)" id="day-tab-2" class="px-3 py-1 rounded-lg text-xs font-semibold text-gray-400 hover:text-white transition">Day 2</button>
              <button onclick="renderDayContent(3)" id="day-tab-3" class="px-3 py-1 rounded-lg text-xs font-semibold text-gray-400 hover:text-white transition">Day 3</button>
              <button onclick="renderDayContent(4)" id="day-tab-4" class="px-3 py-1 rounded-lg text-xs font-semibold text-gray-400 hover:text-white transition">Day 4</button>
            </div>
          </div>

          <!-- Dynamic Active Day Content -->
          <div id="day-itinerary-content" class="mt-5 space-y-4">
            <!-- Populated via JavaScript dynamically -->
          </div>
        </div>

        <!-- Group Split & Fair Settlement (Solves Problem Slide 02) -->
        <div class="glass-panel p-6 rounded-2xl border border-deck-cardBorder">
          <div class="flex items-center justify-between mb-3">
            <div class="flex items-center space-x-2">
              <i data-lucide="users" class="w-4 h-4 text-brand-400"></i>
              <h4 class="text-sm font-bold text-white uppercase tracking-wider">Group Expense Splitter</h4>
            </div>
            <span class="text-xs text-gray-400 font-mono">No Awkward Calculations</span>
          </div>
          
          <div class="grid grid-cols-1 sm:grid-cols-3 gap-3 text-center">
            <div class="bg-deck-card/70 p-3 rounded-xl border border-deck-cardBorder/60">
              <div class="text-[11px] text-gray-400">Per Person Total</div>
              <div id="split-per-person" class="text-lg font-bold text-emerald-400 font-mono mt-0.5">â‚¹4,000</div>
              <div class="text-[10px] text-gray-500">Based on 2 friends</div>
            </div>
            <div class="bg-deck-card/70 p-3 rounded-xl border border-deck-cardBorder/60">
              <div class="text-[11px] text-gray-400">Daily Per Head</div>
              <div id="split-daily-head" class="text-lg font-bold text-sky-400 font-mono mt-0.5">â‚¹1,000</div>
              <div class="text-[10px] text-gray-500">Includes stay + meals</div>
            </div>
            <div class="bg-deck-card/70 p-3 rounded-xl border border-deck-cardBorder/60">
              <div class="text-[11px] text-gray-400">Tripzy Service Fee</div>
              <div id="split-commission" class="text-lg font-bold text-amber-400 font-mono mt-0.5">â‚¹0 extra</div>
              <div class="text-[10px] text-gray-500">Free during hackathon</div>
            </div>
          </div>
        </div>
      </div>

      <!-- RIGHT COLUMN: Interactive Leaflet Map, Live Weather & Local Partner Marketplace (5 Cols) -->
      <div class="lg:col-span-5 space-y-6">

        <!-- Interactive Route Map -->
        <div class="glass-panel p-5 rounded-2xl border border-deck-cardBorder">
          <div class="flex items-center justify-between mb-3">
            <div class="flex items-center space-x-2">
              <i data-lucide="map" class="w-4 h-4 text-brand-400"></i>
              <span class="text-xs font-bold text-white uppercase tracking-wider">Optimized Route & Waypoints</span>
            </div>
            <span id="route-distance-pill" class="text-[11px] bg-brand-500/20 text-brand-400 px-2 py-0.5 rounded-full font-mono">510 km â€¢ ~9 hrs</span>
          </div>

          <!-- Leaflet Map Container -->
          <div id="trip-map" class="shadow-inner border border-deck-cardBorder"></div>

          <!-- Transit Waypoints Chips -->
          <div id="waypoints-container" class="mt-3 flex items-center justify-between text-xs text-gray-400 font-mono px-1 overflow-x-auto gap-2">
            <!-- Dynamic Waypoints populated via JS -->
          </div>
        </div>

        <!-- Live Destination Weather Check (Free Open-Meteo Integration) -->
        <div class="glass-panel p-5 rounded-2xl border border-deck-cardBorder">
          <div class="flex items-center justify-between mb-3">
            <div class="flex items-center space-x-2">
              <i data-lucide="cloud-sun" class="w-4 h-4 text-amber-400"></i>
              <span class="text-xs font-bold text-white uppercase tracking-wider">Live Weather Verification</span>
            </div>
            <span class="text-[10px] text-emerald-400 bg-emerald-500/10 px-2 py-0.5 rounded-full border border-emerald-500/20">Open-Meteo API</span>
          </div>

          <div class="flex items-center justify-between bg-deck-card/80 p-3.5 rounded-xl border border-deck-cardBorder/70">
            <div class="flex items-center space-x-3">
              <div id="weather-icon-box" class="w-10 h-10 rounded-lg bg-amber-500/10 flex items-center justify-center text-amber-400 text-xl font-bold">
                â˜€ï¸
              </div>
              <div>
                <div id="weather-temp" class="text-lg font-bold text-white">22Â°C â€” Sunny</div>
                <div id="weather-location-label" class="text-xs text-gray-400">Rishikesh, Uttarakhand</div>
              </div>
            </div>
            <div class="text-right text-[11px]">
              <div class="text-gray-400">Travel Suitability</div>
              <div id="weather-status-badge" class="font-bold text-emerald-400">Ideal for Rafting</div>
            </div>
          </div>
          
          <div id="packing-advice" class="mt-3 text-[11px] text-gray-400 bg-deck-card/40 p-2.5 rounded-lg border border-deck-cardBorder/40 flex items-start space-x-2">
            <i data-lucide="info" class="w-3.5 h-3.5 text-brand-400 mt-0.5 shrink-0"></i>
            <span>Recommended pack: Quick-dry shorts, waterproof phone pouch, comfortable hiking shoes, light evening windcheater.</span>
          </div>
        </div>

        <!-- Local Partner Ecosystem & Instant Checkout (Slides 07 & 08) -->
        <div id="partner-ecosystem" class="glass-panel p-5 rounded-2xl border border-deck-cardBorder">
          <div class="flex items-center justify-between mb-3">
            <div>
              <span class="text-[10px] uppercase font-bold text-brand-400 tracking-wider">Slide 07 & 08 Local Marketplace</span>
              <h4 class="text-sm font-bold text-white">Verified Local Partners</h4>
            </div>
            <span class="text-[10px] text-sky-400 bg-sky-500/10 px-2 py-0.5 rounded-full border border-sky-500/20">10% Take-Rate</span>
          </div>

          <!-- Partner Items with Book Button -->
          <div class="space-y-2.5">
            <!-- Stay Partner -->
            <div class="p-3 rounded-xl bg-deck-card border border-deck-cardBorder flex items-center justify-between hover:border-brand-500/50 transition">
              <div class="flex items-center space-x-3">
                <div class="w-9 h-9 rounded-lg bg-amber-500/10 flex items-center justify-center text-amber-400">
                  <i data-lucide="bed" class="w-4 h-4"></i>
                </div>
                <div>
                  <div class="text-xs font-bold text-white">Zostel / Ganga Valley Dorm</div>
                  <div class="text-[11px] text-gray-400">Tapovan, Rishikesh â€¢ â­ 4.8</div>
                </div>
              </div>
              <button onclick="openBookingModal('Zostel Tapovan River Hostel', 2000, 'Stay')" class="px-3 py-1.5 rounded-lg bg-brand-500 hover:bg-brand-600 text-white text-xs font-semibold transition">
                Book â‚¹2,000
              </button>
            </div>

            <!-- Activity Partner -->
            <div class="p-3 rounded-xl bg-deck-card border border-deck-cardBorder flex items-center justify-between hover:border-brand-500/50 transition">
              <div class="flex items-center space-x-3">
                <div class="w-9 h-9 rounded-lg bg-purple-500/10 flex items-center justify-center text-purple-400">
                  <i data-lucide="waves" class="w-4 h-4"></i>
                </div>
                <div>
                  <div class="text-xs font-bold text-white">Shivpuri 16km Grade-III Rafting</div>
                  <div class="text-[11px] text-gray-400">Certified Guides + Cliff Jump â€¢ â­ 4.9</div>
                </div>
              </div>
              <button onclick="openBookingModal('Shivpuri Grade-III Rafting', 1200, 'Activity')" class="px-3 py-1.5 rounded-lg bg-brand-500 hover:bg-brand-600 text-white text-xs font-semibold transition">
                Book â‚¹1,200
              </button>
            </div>

            <!-- Move / Scooter Rental -->
            <div class="p-3 rounded-xl bg-deck-card border border-deck-cardBorder flex items-center justify-between hover:border-brand-500/50 transition">
              <div class="flex items-center space-x-3">
                <div class="w-9 h-9 rounded-lg bg-sky-500/10 flex items-center justify-center text-sky-400">
                  <i data-lucide="bike" class="w-4 h-4"></i>
                </div>
                <div>
                  <div class="text-xs font-bold text-white">Activa 6G Rental (48 hrs)</div>
                  <div class="text-[11px] text-gray-400">Helmet + Free Delivery â€¢ â­ 4.7</div>
                </div>
              </div>
              <button onclick="openBookingModal('Activa 6G Scooter (2 Days)', 900, 'Transport')" class="px-3 py-1.5 rounded-lg bg-brand-500 hover:bg-brand-600 text-white text-xs font-semibold transition">
                Book â‚¹900
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ================= PITCH DECK EXHIBITION SECTIONS ================= -->
    
    <!-- SLIDE 02: THE OLD WAY vs TRIPZY GUIDED FLOW -->
    <section id="deck-comparison" class="mt-20 pt-10 border-t border-deck-cardBorder/60 no-print">
      <div class="text-center max-w-2xl mx-auto mb-12">
        <span class="text-xs uppercase font-extrabold tracking-widest text-brand-500">Slide 02 â€¢ The Core Problem</span>
        <h2 class="text-3xl font-bold font-display text-white mt-1">Travel Planning is Broken</h2>
        <p class="text-sm text-gray-400 mt-2">Too many tabs, shifting prices, broken group budgets, and conflicting choices.</p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
        <!-- The Old Way (Deck Slide 01 & 02) -->
        <div class="glass-panel p-6 rounded-2xl border border-red-500/20 bg-red-950/10">
          <div class="flex items-center space-x-2 text-red-400 mb-4">
            <i data-lucide="x-circle" class="w-5 h-5"></i>
            <span class="text-sm font-bold uppercase tracking-wider">The Old Way (5+ Exhausting Steps)</span>
          </div>
          <div class="grid grid-cols-2 gap-3 text-center">
            <div class="bg-deck-card/80 p-3 rounded-xl border border-deck-cardBorder">
              <i data-lucide="layers" class="w-5 h-5 text-gray-400 mx-auto mb-1"></i>
              <div class="text-xs font-bold text-gray-300">12+ Search Tabs</div>
              <div class="text-[10px] text-gray-500">OTAs, reviews, blogs</div>
            </div>
            <div class="bg-deck-card/80 p-3 rounded-xl border border-deck-cardBorder">
              <i data-lucide="message-square" class="w-5 h-5 text-gray-400 mx-auto mb-1"></i>
              <div class="text-xs font-bold text-gray-300">Group Chats</div>
              <div class="text-[10px] text-gray-500">Indecision & arguments</div>
            </div>
            <div class="bg-deck-card/80 p-3 rounded-xl border border-deck-cardBorder">
              <i data-lucide="trending-up" class="w-5 h-5 text-gray-400 mx-auto mb-1"></i>
              <div class="text-xs font-bold text-gray-300">Fluctuating Prices</div>
              <div class="text-[10px] text-gray-500">Budgets constantly break</div>
            </div>
            <div class="bg-deck-card/80 p-3 rounded-xl border border-deck-cardBorder">
              <i data-lucide="refresh-cw" class="w-5 h-5 text-gray-400 mx-auto mb-1"></i>
              <div class="text-xs font-bold text-gray-300">Search Again</div>
              <div class="text-[10px] text-gray-500">Hours wasted replanning</div>
            </div>
          </div>
        </div>

        <!-- The Tripzy Guided Flow -->
        <div class="glass-panel p-6 rounded-2xl border border-emerald-500/30 bg-emerald-950/10">
          <div class="flex items-center space-x-2 text-emerald-400 mb-4">
            <i data-lucide="check-circle" class="w-5 h-5"></i>
            <span class="text-sm font-bold uppercase tracking-wider">With Tripzy AI (One Guided Flow)</span>
          </div>
          <div class="space-y-2.5">
            <div class="flex items-center justify-between p-2.5 rounded-xl bg-deck-card/80 border border-deck-cardBorder">
              <div class="flex items-center space-x-3">
                <span class="w-6 h-6 rounded-full bg-brand-500 text-white flex items-center justify-center text-xs font-bold">1</span>
                <span class="text-xs text-gray-300">Budget, Destination & Travel style entered in seconds</span>
              </div>
              <i data-lucide="check" class="w-4 h-4 text-emerald-400"></i>
            </div>
            <div class="flex items-center justify-between p-2.5 rounded-xl bg-deck-card/80 border border-deck-cardBorder">
              <div class="flex items-center space-x-3">
                <span class="w-6 h-6 rounded-full bg-brand-500 text-white flex items-center justify-center text-xs font-bold">2</span>
                <span class="text-xs text-gray-300">Real-time feasibility check (Weather, distance, rates)</span>
              </div>
              <i data-lucide="check" class="w-4 h-4 text-emerald-400"></i>
            </div>
            <div class="flex items-center justify-between p-2.5 rounded-xl bg-deck-card/80 border border-deck-cardBorder">
              <div class="flex items-center space-x-3">
                <span class="w-6 h-6 rounded-full bg-brand-500 text-white flex items-center justify-center text-xs font-bold">3</span>
                <span class="text-xs text-gray-300">Live replanning automatically if budget or weather changes</span>
              </div>
              <i data-lucide="check" class="w-4 h-4 text-emerald-400"></i>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- SLIDE 08: BUSINESS MODEL & REVENUE CALCULATOR -->
    <section id="business-model" class="mt-20 pt-10 border-t border-deck-cardBorder/60 no-print">
      <div class="text-center max-w-2xl mx-auto mb-10">
        <span class="text-xs uppercase font-extrabold tracking-widest text-brand-500">Slide 08 â€¢ Monetization</span>
        <h2 class="text-3xl font-bold font-display text-white mt-1">We Earn When a Trip Gets Booked</h2>
        <p class="text-sm text-gray-400 mt-2">Targeting 8%â€“15% take-rate across transport, stays, dining, and activities.</p>
      </div>

      <div class="glass-panel p-6 sm:p-8 rounded-2xl border border-deck-cardBorder">
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 items-center">
          <div>
            <h4 class="text-lg font-bold text-white mb-2">Simulate Tripzy Platform Revenue</h4>
            <p class="text-xs text-gray-400 mb-6">Test how varying booking volumes and trip sizes translate into platform net revenues.</p>
            
            <div class="space-y-4">
              <div>
                <div class="flex justify-between text-xs font-medium text-gray-300 mb-1">
                  <span>Monthly Trip Bookings</span>
                  <span id="rev-trips-label" class="font-bold text-brand-400">1,500 Trips</span>
                </div>
                <input type="range" id="rev-trips-slider" min="100" max="10000" step="100" value="1500" oninput="calculatePlatformRevenue()" class="w-full h-2 bg-deck-cardBorder rounded-lg appearance-none cursor-pointer accent-brand-500">
              </div>

              <div>
                <div class="flex justify-between text-xs font-medium text-gray-300 mb-1">
                  <span>Average Trip Ticket Size</span>
                  <span id="rev-size-label" class="font-bold text-brand-400">â‚¹8,000</span>
                </div>
                <input type="range" id="rev-size-slider" min="4000" max="25000" step="500" value="8000" oninput="calculatePlatformRevenue()" class="w-full h-2 bg-deck-cardBorder rounded-lg appearance-none cursor-pointer accent-brand-500">
              </div>

              <div>
                <div class="flex justify-between text-xs font-medium text-gray-300 mb-1">
                  <span>Take-Rate Commission %</span>
                  <span id="rev-rate-label" class="font-bold text-brand-400">10%</span>
                </div>
                <input type="range" id="rev-rate-slider" min="8" max="15" step="1" value="10" oninput="calculatePlatformRevenue()" class="w-full h-2 bg-deck-cardBorder rounded-lg appearance-none cursor-pointer accent-brand-500">
              </div>
            </div>
          </div>

          <div class="bg-deck-card/90 p-6 rounded-2xl border border-deck-cardBorder text-center">
            <span class="text-xs uppercase font-extrabold tracking-wider text-brand-400">Projected Monthly Platform Revenue</span>
            <div id="projected-revenue-total" class="text-4xl font-extrabold text-white font-mono mt-3 mb-1">â‚¹12,00,000</div>
            <div id="projected-gmv-total" class="text-xs text-gray-400 font-mono">Gross Merchandise Value: â‚¹1.2 Crore / month</div>
            
            <div class="grid grid-cols-2 gap-3 mt-6 pt-6 border-t border-deck-cardBorder/70 text-left">
              <div class="p-3 rounded-xl bg-deck-bg/60 border border-deck-cardBorder/50">
                <div class="text-[10px] text-gray-400 uppercase font-semibold">Local Partner Payout</div>
                <div id="projected-partner-payout" class="text-base font-bold text-emerald-400 font-mono mt-0.5">â‚¹1.08 Cr</div>
              </div>
              <div class="p-3 rounded-xl bg-deck-bg/60 border border-deck-cardBorder/50">
                <div class="text-[10px] text-gray-400 uppercase font-semibold">Per Trip Commission</div>
                <div id="projected-per-trip" class="text-base font-bold text-amber-400 font-mono mt-0.5">â‚¹800 / trip</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- SLIDE 09: MARKET OPPORTUNITY & COMPETITION -->
    <section class="mt-20 pt-10 border-t border-deck-cardBorder/60 no-print">
      <div class="text-center max-w-2xl mx-auto mb-10">
        <span class="text-xs uppercase font-extrabold tracking-widest text-brand-500">Slide 09 â€¢ Market & Moat</span>
        <h2 class="text-3xl font-bold font-display text-white mt-1">Why Tripzy AI Wins</h2>
        <p class="text-sm text-gray-400 mt-2">Indian Online Travel Market: ~â‚¹2.6 Trillion (Redseer 2024)</p>
      </div>

      <div class="glass-panel p-6 rounded-2xl border border-deck-cardBorder overflow-x-auto">
        <table class="w-full text-left text-xs sm:text-sm">
          <thead>
            <tr class="border-b border-deck-cardBorder text-gray-400">
              <th class="py-3 px-4">Feature</th>
              <th class="py-3 px-4 text-brand-400 font-bold bg-brand-500/10 rounded-t-lg">Tripzy AI</th>
              <th class="py-3 px-4">Traditional OTAs</th>
              <th class="py-3 px-4">Travel Community</th>
              <th class="py-3 px-4">Generic LLMs</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-deck-cardBorder/50 text-gray-300">
            <tr>
              <td class="py-3.5 px-4 font-medium text-white">Full Trip AI Flow</td>
              <td class="py-3.5 px-4 font-bold text-emerald-400 bg-brand-500/10">âœ“ End-to-end</td>
              <td class="py-3.5 px-4 text-red-400">âœ— Fragmented</td>
              <td class="py-3.5 px-4 text-red-400">âœ— Manual</td>
              <td class="py-3.5 px-4 text-amber-400">~ Text Only</td>
            </tr>
            <tr>
              <td class="py-3.5 px-4 font-medium text-white">Dynamic Budget Matrix</td>
              <td class="py-3.5 px-4 font-bold text-emerald-400 bg-brand-500/10">âœ“ Auto-balances</td>
              <td class="py-3.5 px-4 text-red-400">âœ— Static filters</td>
              <td class="py-3.5 px-4 text-red-400">âœ— None</td>
              <td class="py-3.5 px-4 text-red-400">âœ— Hallucinates rates</td>
            </tr>
            <tr>
              <td class="py-3.5 px-4 font-medium text-white">Live Disruption Re-planning</td>
              <td class="py-3.5 px-4 font-bold text-emerald-400 bg-brand-500/10">âœ“ Real-time</td>
              <td class="py-3.5 px-4 text-red-400">âœ— Manual cancellation</td>
              <td class="py-3.5 px-4 text-red-400">âœ— None</td>
              <td class="py-3.5 px-4 text-amber-400">~ Needs manual prompt</td>
            </tr>
            <tr>
              <td class="py-3.5 px-4 font-medium text-white">Local Partner Micro-booking</td>
              <td class="py-3.5 px-4 font-bold text-emerald-400 bg-brand-500/10">âœ“ Instant checkout</td>
              <td class="py-3.5 px-4 text-amber-400">~ High commission OTAs</td>
              <td class="py-3.5 px-4 text-red-400">âœ— External links</td>
              <td class="py-3.5 px-4 text-red-400">âœ— No commerce layer</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </main>

  <!-- ================= BOOKING MODAL (Slide 07 & 08 Instant Checkout) ================= -->
  <div id="booking-modal" class="fixed inset-0 z-50 bg-black/80 backdrop-blur-sm hidden items-center justify-center p-4 no-print">
    <div class="glass-panel p-6 sm:p-8 rounded-2xl max-w-md w-full border border-brand-500/40 shadow-2xl relative">
      <button onclick="closeBookingModal()" class="absolute top-4 right-4 text-gray-400 hover:text-white">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>

      <div class="flex items-center space-x-3 mb-4">
        <div class="w-10 h-10 rounded-xl bg-brand-500/20 text-brand-400 flex items-center justify-center">
          <i data-lucide="shield-check" class="w-5 h-5"></i>
        </div>
        <div>
          <span class="text-[10px] uppercase font-bold text-brand-400">Instant Partner Checkout</span>
          <h3 class="text-lg font-bold text-white">Confirm Reservation</h3>
        </div>
      </div>

      <div class="bg-deck-card/80 p-4 rounded-xl border border-deck-cardBorder mb-5 space-y-2">
        <div class="flex justify-between text-xs">
          <span class="text-gray-400">Item:</span>
          <span id="modal-item-name" class="font-bold text-white">Zostel Tapovan</span>
        </div>
        <div class="flex justify-between text-xs">
          <span class="text-gray-400">Base Price:</span>
          <span id="modal-base-price" class="font-mono text-gray-200">â‚¹2,000</span>
        </div>
        <div class="flex justify-between text-xs">
          <span class="text-gray-400">Platform Take-rate (10%):</span>
          <span id="modal-platform-fee" class="font-mono text-brand-400">â‚¹200 (Inc.)</span>
        </div>
        <div class="pt-2 border-t border-deck-cardBorder/60 flex justify-between text-sm font-bold">
          <span class="text-white">Total Payable:</span>
          <span id="modal-total-price" class="text-emerald-400 font-mono">â‚¹2,000</span>
        </div>
      </div>

      <div id="booking-payment-state">
        <div class="space-y-3 mb-5">
          <label class="block text-xs font-semibold text-gray-300">Choose Dummy Payment Mode:</label>
          <div class="grid grid-cols-2 gap-2 text-xs">
            <button type="button" class="py-2 px-3 rounded-lg border border-brand-500 bg-brand-500/10 text-white font-medium text-center">
              UPI (GPay / PhonePe)
            </button>
            <button type="button" class="py-2 px-3 rounded-lg border border-deck-cardBorder bg-deck-card text-gray-400 text-center">
              Credit / Debit Card
            </button>
          </div>
        </div>

        <button onclick="executeDummyPayment()" id="modal-pay-btn" class="w-full py-3 rounded-xl bg-brand-500 hover:bg-brand-600 text-white font-bold text-sm shadow-lg shadow-brand-500/30 transition flex items-center justify-center space-x-2">
          <i data-lucide="lock" class="w-4 h-4"></i>
          <span>Pay & Confirm Reservation</span>
        </button>
      </div>

      <div id="booking-success-state" class="hidden text-center py-4">
        <div class="w-12 h-12 rounded-full bg-emerald-500/20 text-emerald-400 mx-auto flex items-center justify-center mb-3">
          <i data-lucide="check-circle-2" class="w-7 h-7"></i>
        </div>
        <h4 class="text-base font-bold text-white">Booking Confirmed!</h4>
        <p class="text-xs text-gray-300 mt-1">Voucher sent via WhatsApp & SMS. 10% platform commission credited to Tripzy Escrow.</p>
        <button onclick="closeBookingModal()" class="mt-4 px-5 py-2 rounded-xl bg-deck-card border border-deck-cardBorder text-xs font-semibold text-white">
          Done
        </button>
      </div>
    </div>
  </div>

  <!-- ================= GEMINI API KEY MODAL ================= -->
  <div id="config-modal" class="fixed inset-0 z-50 bg-black/80 backdrop-blur-sm hidden items-center justify-center p-4 no-print">
    <div class="glass-panel p-6 rounded-2xl max-w-md w-full border border-deck-cardBorder shadow-2xl relative">
      <button onclick="toggleConfigModal()" class="absolute top-4 right-4 text-gray-400 hover:text-white">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>

      <div class="flex items-center space-x-2.5 mb-3">
        <i data-lucide="key" class="w-5 h-5 text-brand-400"></i>
        <h3 class="text-base font-bold text-white">Gemini API Key (Optional)</h3>
      </div>
      <p class="text-xs text-gray-300 mb-4 leading-relaxed">
        Tripzy operates with built-in heuristic AI models by default. You can optionally paste your free Google Gemini API Key below for customized LLM trip prompts.
      </p>

      <input 
        type="password" 
        id="gemini-api-key" 
        placeholder="AIzaSy..." 
        class="w-full bg-deck-card border border-deck-cardBorder rounded-xl px-3.5 py-2 text-xs text-white mb-4 focus:outline-none focus:border-brand-500"
      />

      <div class="flex justify-end space-x-2">
        <button onclick="toggleConfigModal()" class="px-3.5 py-2 rounded-lg bg-deck-card text-xs text-gray-400">Cancel</button>
        <button onclick="saveApiKey()" class="px-4 py-2 rounded-lg bg-brand-500 hover:bg-brand-600 text-white text-xs font-bold">Save Key</button>
      </div>
    </div>
  </div>

  <!-- ================= FOOTER ================= -->
  <footer class="border-t border-deck-cardBorder/60 py-8 bg-deck-bg text-center text-xs text-gray-500 no-print">
    <div class="max-w-7xl mx-auto px-4 flex flex-col sm:flex-row items-center justify-between gap-4">
      <div class="flex items-center space-x-2">
        <span class="font-display font-bold text-white">Tripzy<span class="text-brand-500">.AI</span></span>
        <span>â€” AI Manthon 2k26 Pitch Deck Prototype</span>
      </div>
      <div class="text-gray-400">
        "Tripzy AI wants to become the simple decision layer before every trip."
      </div>
      <div class="flex space-x-4">
        <span class="hover:text-gray-300">Privacy</span>
        <span class="hover:text-gray-300">Terms</span>
        <span class="hover:text-gray-300">Pitch Deck</span>
      </div>
    </div>
  </footer>

  <!-- ================= SCRIPT & DYNAMIC LOGIC ================= -->
  <script>
    // --- Global Application State ---
    const appState = {
      origin: 'Lucknow',
      destination: 'Uttarakhand',
      budget: 8000,
      travelers: 2,
      days: 4,
      isRaining: false,
      activeDay: 1,
      geminiKey: localStorage.getItem('tripzy_gemini_key') || '',
      map: null,
      routePolyline: null,
      markers: []
    };

    // --- Predefined Destination Coordinates & Waypoints (Slides 04, 06, 09) ---
    const DESTINATIONS_DATA = {
      Uttarakhand: {
        name: 'Uttarakhand (Rishikesh / Mussoorie)',
        coords: [30.0869, 78.2676], // Rishikesh
        waypoints: [
          { name: 'Lucknow', coords: [26.8467, 80.9462], stop: 'Origin' },
          { name: 'Bareilly Junction', coords: [28.3670, 79.4304], stop: 'Transit' },
          { name: 'Haridwar', coords: [29.9457, 78.1642], stop: 'Transit' },
          { name: 'Rishikesh', coords: [30.0869, 78.2676], stop: 'Destination' }
        ],
        distance: '510 km â€¢ ~9 hrs',
        weatherQuery: { lat: 30.0869, lon: 78.2676, place: 'Rishikesh, Uttarakhand' }
      },
      Himachal: {
        name: 'Himachal Pradesh (Manali / Kasol)',
        coords: [32.2432, 77.1892],
        waypoints: [
          { name: 'Lucknow', coords: [26.8467, 80.9462], stop: 'Origin' },
          { name: 'Ambala Cantt', coords: [30.3782, 76.7767], stop: 'Transit' },
          { name: 'Mandi', coords: [31.5892, 76.9182], stop: 'Transit' },
          { name: 'Manali', coords: [32.2432, 77.1892], stop: 'Destination' }
        ],
        distance: '860 km â€¢ ~16 hrs',
        weatherQuery: { lat: 32.2432, lon: 77.1892, place: 'Manali, Himachal Pradesh' }
      },
      Rajasthan: {
        name: 'Rajasthan (Jaipur / Pushkar)',
        coords: [26.9124, 75.7873],
        waypoints: [
          { name: 'Lucknow', coords: [26.8467, 80.9462], stop: 'Origin' },
          { name: 'Agra Fort', coords: [27.1767, 78.0081], stop: 'Transit' },
          { name: 'Bharatpur', coords: [27.2152, 77.4930], stop: 'Transit' },
          { name: 'Jaipur', coords: [26.9124, 75.7873], stop: 'Destination' }
        ],
        distance: '570 km â€¢ ~8.5 hrs',
        weatherQuery: { lat: 26.9124, lon: 75.7873, place: 'Jaipur, Rajasthan' }
      },
      Goa: {
        name: 'Goa (Beaches & Cafes)',
        coords: [15.2993, 74.1240],
        waypoints: [
          { name: 'Lucknow', coords: [26.8467, 80.9462], stop: 'Origin' },
          { name: 'Bhopal', coords: [23.2599, 77.4126], stop: 'Transit' },
          { name: 'Pune', coords: [18.5204, 73.8567], stop: 'Transit' },
          { name: 'North Goa', coords: [15.5494, 73.7535], stop: 'Destination' }
        ],
        distance: '1,840 km â€¢ Train / Flight',
        weatherQuery: { lat: 15.2993, lon: 74.1240, place: 'Panaji, Goa' }
      },
      NorthEast: {
        name: 'North-East (Meghalaya / Shillong)',
        coords: [25.5788, 91.8933],
        waypoints: [
          { name: 'Lucknow', coords: [26.8467, 80.9462], stop: 'Origin' },
          { name: 'Patna', coords: [25.5941, 85.1376], stop: 'Transit' },
          { name: 'Guwahati', coords: [26.1445, 91.7362], stop: 'Transit' },
          { name: 'Shillong', coords: [25.5788, 91.8933], stop: 'Destination' }
        ],
        distance: '1,280 km â€¢ Train / Transit',
        weatherQuery: { lat: 25.5788, lon: 91.8933, place: 'Shillong, Meghalaya' }
      }
    };

    // --- Dynamic Itinerary Repository by Destination & Weather ---
    const ITINERARIES_DB = {
      Uttarakhand: {
        sunny: {
          1: [
            { time: '06:30 AM', title: 'Overnight Express Arrival at Haridwar / Rishikesh', desc: 'Arrive via sleeper/3AC train from Lucknow. Shared electric auto to Tapovan.', cost: 'â‚¹120' },
            { time: '09:00 AM', title: 'Check-in at Tapovan Riverside Hostel', desc: 'Freshen up and enjoy breakfast thali overlooking the Ganga.', cost: 'â‚¹250' },
            { time: '02:00 PM', title: 'Walk across Ram Jhula & Beatles Ashram', desc: 'Explore historic murals, quiet green groves, and cliff views.', cost: 'â‚¹150' },
            { time: '06:00 PM', title: 'Triveni Ghat Evening Ganga Aarti', desc: 'Witness the serene lamps and chanting ceremony by the river banks.', cost: 'Free' }
          ],
          2: [
            { time: '08:30 AM', title: 'Shivpuri 16km Grade-III River Rafting', desc: 'Thrilling white-water rapids through Roller Coaster & Golf Course rapids with cliff jump.', cost: 'â‚¹600' },
            { time: '01:30 PM', title: 'Local Garhwali Lunch at Chotiwala', desc: 'Kafli, Jhangora kheer, and hot tandoori rotis.', cost: 'â‚¹280' },
            { time: '04:30 PM', title: 'Scooter Ride to Neer Garh Waterfalls', desc: 'Trek up to the natural cold pools for breathtaking sunset views.', cost: 'â‚¹50' },
            { time: '08:30 PM', title: 'Bonfire & Acoustic Night at Hostel', desc: 'Connect with fellow budget backpackers.', cost: 'Free' }
          ],
          3: [
            { time: '05:00 AM', title: 'Sunrise Trek to Kunjapuri Devi Temple', desc: 'Breathtaking 360Â° views of snow-clad Himalayan peaks from 1,650 meters.', cost: 'â‚¹200' },
            { time: '11:00 AM', title: 'Cafe Hopping in Tapovan', desc: 'Visit Little Buddha Cafe for lemon ginger honey tea and pizza slices.', cost: 'â‚¹350' },
            { time: '04:00 PM', title: 'Beach Volleyball at Goa Beach Rishikesh', desc: 'White sand banks of the Ganga during golden hour.', cost: 'Free' },
            { time: '08:00 PM', title: 'Dinner & Live Music at Beatles Cafe', desc: 'Organic pasta and peaceful acoustic melodies.', cost: 'â‚¹400' }
          ],
          4: [
            { time: '08:00 AM', title: 'Morning Sound Bath & Yoga Session', desc: 'Relaxing sound meditation session near Lakshman Jhula.', cost: 'â‚¹300' },
            { time: '12:00 PM', title: 'Local Souvenir & Tea Shopping', desc: 'Buy Himalayan green tea, rudraksha beads, and wooden handicrafts.', cost: 'â‚¹250' },
            { time: '05:30 PM', title: 'Board Overnight Volvo / Train to Lucknow', desc: 'Smooth return journey within the budget envelope.', cost: 'â‚¹1,200' }
          ]
        },
        rainy: {
          1: [
            { time: '07:00 AM', title: 'Rain Protocol: Arrive & Cozy Check-in', desc: 'Direct cab transfer to hostel lounge. Complimentary ginger tea.', cost: 'â‚¹200' },
            { time: '10:30 AM', title: 'Indoor Sound Healing & Meditation', desc: 'Safe from weather in a covered studio overlooking rain-misted mountains.', cost: 'â‚¹300' },
            { time: '02:00 PM', title: 'Gourmet Cafe Exploration in Tapovan', desc: 'Enjoy wood-fired pizza and books at Nirvana Cafe.', cost: 'â‚¹350' },
            { time: '06:00 PM', title: 'Covered Ghat Aarti at Parmarth Niketan', desc: 'Sheltered seating for peaceful rituals during monsoon showers.', cost: 'Free' }
          ],
          2: [
            { time: '09:00 AM', title: 'Indoor Ayurvedic Cooking & Chai Workshop', desc: 'Replaced outdoor river rafting due to high water levels for safety.', cost: 'â‚¹350' },
            { time: '01:00 PM', title: 'Traditional Kumaoni Lunch', desc: 'Hot Bhatt ki Churkani and Mandua rotis at safe sheltered diner.', cost: 'â‚¹250' },
            { time: '04:00 PM', title: 'Beatles Ashram Indoor Gallery Exploration', desc: 'Covered photo exhibitions and spiritual architecture.', cost: 'â‚¹150' },
            { time: '07:30 PM', title: 'Board Games & Movie Night at Hostel Common Room', desc: 'Indoor networking and hot soup with travelers.', cost: 'Free' }
          ],
          3: [
            { time: '09:30 AM', title: 'Scenic Low-Altitude Rain Walk', desc: 'Misty pine forest trail with rain gear and umbrellas.', cost: 'Free' },
            { time: '01:30 PM', title: 'Cafe Hopping & Hot Chocolate Tasting', desc: 'Visit Beatles Cafe and Ganga View Bakery.', cost: 'â‚¹300' },
            { time: '05:00 PM', title: 'Evening Yoga & Breathwork Immersion', desc: 'Restorative indoor session.', cost: 'â‚¹250' }
          ],
          4: [
            { time: '10:00 AM', title: 'Local Handicrafts Emporium Visit', desc: 'Sheltered shopping for hill honey and woolens.', cost: 'â‚¹300' },
            { time: '04:00 PM', title: 'Early Departures via Sheltered Volvo', desc: 'Safe mountain roads descent prior to peak evening rains.', cost: 'â‚¹1,200' }
          ]
        }
      }
    };

    // --- Initialize Page & Leaflet Map ---
    document.addEventListener('DOMContentLoaded', () => {
      lucide.createIcons();
      initLeafletMap();
      fetchFreeWeather(DESTINATIONS_DATA.Uttarakhand.weatherQuery);
      updateBudgetAllocations(8000);
      renderDayContent(1);
      calculatePlatformRevenue();
    });

    // --- Leaflet Map Setup ---
    function initLeafletMap() {
      const dest = DESTINATIONS_DATA[appState.destination];
      appState.map = L.map('trip-map', {
        zoomControl: false,
        attributionControl: false
      }).setView([28.4668, 79.6069], 7);

      L.tileLayer('https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png', {
        maxZoom: 18,
      }).addTo(appState.map);

      L.control.zoom({ position: 'bottomright' }).addTo(appState.map);

      drawRouteWaypoints(dest);
    }

    function drawRouteWaypoints(destinationObj) {
      // Clear previous markers & line
      if (appState.routePolyline) appState.map.removeLayer(appState.routePolyline);
      appState.markers.forEach(m => appState.map.removeLayer(m));
      appState.markers = [];

      const waypoints = destinationObj.waypoints;
      const latlngs = waypoints.map(w => w.coords);

      // Draw Polyline
      appState.routePolyline = L.polyline(latlngs, {
        color: '#f97316',
        weight: 4,
        opacity: 0.9,
        dashArray: '6, 8',
        lineCap: 'round'
      }).addTo(appState.map);

      // Draw Markers with custom HTML icons
      waypoints.forEach((wp, index) => {
        const isEndpoint = index === 0 || index === waypoints.length - 1;
        const iconHtml = `
          <div style="background-color: ${isEndpoint ? '#f97316' : '#0F1A34'}; border: 2px solid ${isEndpoint ? '#fff' : '#f97316'}; color: white; width: 24px; height: 24px; border-radius: 50%; display: flex; items-center; justify-content: center; font-size: 11px; font-weight: bold; box-shadow: 0 0 10px rgba(0,0,0,0.5);">
            ${index + 1}
          </div>
        `;
        const customIcon = L.divIcon({
          className: 'custom-div-icon',
          html: iconHtml,
          iconSize: [24, 24],
          iconAnchor: [12, 12]
        });

        const marker = L.marker(wp.coords, { icon: customIcon })
          .bindPopup(`<b>${wp.name}</b><br><span style="font-size: 11px; color: #f97316;">${wp.stop}</span>`)
          .addTo(appState.map);
        appState.markers.push(marker);
      });

      appState.map.fitBounds(appState.routePolyline.getBounds(), { padding: [40, 40] });

      // Update Waypoints Strip UI
      const container = document.getElementById('waypoints-container');
      container.innerHTML = waypoints.map((w, idx) => `
        <div class="flex items-center space-x-1 whitespace-nowrap">
          <span class="w-4 h-4 rounded-full bg-brand-500/20 text-brand-400 border border-brand-500/40 text-[10px] flex items-center justify-center font-bold">${idx + 1}</span>
          <span class="text-white font-medium">${w.name}</span>
          ${idx < waypoints.length - 1 ? '<span class="text-gray-600">â†’</span>' : ''}
        </div>
      `).join('');

      document.getElementById('route-distance-pill').textContent = destinationObj.distance;
    }

    // --- Slide 06 Exact Dynamic Re-budgeting Engine ---
    function updateBudgetAllocations(totalBudget) {
      appState.budget = parseInt(totalBudget);
      document.getElementById('budget-display').textContent = `â‚¹${appState.budget.toLocaleString('en-IN')}`;
      document.getElementById('budget-total-pill').textContent = `â‚¹${appState.budget.toLocaleString('en-IN')}`;

      let transport, stay, food, activities, buffer;

      if (appState.budget <= 6000) {
        // Slide 06 â‚¹6,000 Demonstration Breakdown
        transport = 1700;
        stay = 1300;
        food = 1200;
        activities = 1200;
        buffer = 600;
        document.getElementById('alloc-transport-mode').textContent = 'Sleeper Class Express / Ordinary State Transport';
        document.getElementById('alloc-stay-mode').textContent = 'Budget Backpacker Dormitory (Tapovan)';
      } else if (appState.budget <= 9000) {
        // Slide 04 & 06 â‚¹8,000 Benchmark Breakdown
        transport = Math.round(appState.budget * 0.30);
        stay = Math.round(appState.budget * 0.25);
        food = Math.round(appState.budget * 0.20);
        activities = Math.round(appState.budget * 0.20);
        buffer = appState.budget - (transport + stay + food + activities);
        document.getElementById('alloc-transport-mode').textContent = 'AC 3-Tier Express or Overnight Volvo Bus';
        document.getElementById('alloc-stay-mode').textContent = 'Riverview Hostel Bed / Clean Mountain Homestay';
      } else {
        // Higher Comfort Tier
        transport = Math.round(appState.budget * 0.32);
        stay = Math.round(appState.budget * 0.30);
        food = Math.round(appState.budget * 0.18);
        activities = Math.round(appState.budget * 0.15);
        buffer = appState.budget - (transport + stay + food + activities);
        document.getElementById('alloc-transport-mode').textContent = 'Semi-Sleeper Volvo / 2AC Train Express';
        document.getElementById('alloc-stay-mode').textContent = 'Private Boutique Room or Premium Riverside Cottage';
      }

      // Render values
      document.getElementById('alloc-transport-amt').textContent = `â‚¹${transport.toLocaleString('en-IN')}`;
      document.getElementById('alloc-stay-amt').textContent = `â‚¹${stay.toLocaleString('en-IN')}`;
      document.getElementById('alloc-food-amt').textContent = `â‚¹${food.toLocaleString('en-IN')}`;
      document.getElementById('alloc-activity-amt').textContent = `â‚¹${activities.toLocaleString('en-IN')}`;
      document.getElementById('alloc-buffer-amt').textContent = `â‚¹${buffer.toLocaleString('en-IN')}`;

      // Progress bars %
      document.getElementById('alloc-transport-bar').style.width = `${(transport / appState.budget) * 100}%`;
      document.getElementById('alloc-stay-bar').style.width = `${(stay / appState.budget) * 100}%`;
      document.getElementById('alloc-food-bar').style.width = `${(food / appState.budget) * 100}%`;
      document.getElementById('alloc-activity-bar').style.width = `${(activities / appState.budget) * 100}%`;
      document.getElementById('alloc-buffer-bar').style.width = `${(buffer / appState.budget) * 100}%`;

      // Update Group Split Card (Slide 02)
      const perHead = Math.round(appState.budget / appState.travelers);
      const perDay = Math.round(perHead / appState.days);
      document.getElementById('split-per-person').textContent = `â‚¹${perHead.toLocaleString('en-IN')}`;
      document.getElementById('split-daily-head').textContent = `â‚¹${perDay.toLocaleString('en-IN')}`;
    }

    // --- Slider & Preset Handlers ---
    function handleBudgetSlider(val) {
      updateBudgetAllocations(val);
    }

    function setBudgetPreset(amt) {
      document.getElementById('budget-slider').value = amt;
      updateBudgetAllocations(amt);
    }

    // --- Free Open-Meteo Weather API Integration ---
    async function fetchFreeWeather(query) {
      try {
        const res = await fetch(`https://api.open-meteo.com/v1/forecast?latitude=${query.lat}&longitude=${query.lon}&current_weather=true`);
        const data = await res.json();
        if (data && data.current_weather) {
          const temp = Math.round(data.current_weather.temperature);
          const weatherCode = data.current_weather.weathercode;
          
          let condition = 'Clear Skies';
          let icon = 'â˜€ï¸';
          if (weatherCode > 50) {
            condition = 'Rainy / Overcast';
            icon = 'ðŸŒ§ï¸';
          } else if (weatherCode > 2) {
            condition = 'Partly Cloudy';
            icon = 'â›…';
          }

          document.getElementById('weather-temp').textContent = `${temp}Â°C â€” ${condition}`;
          document.getElementById('weather-location-label').textContent = query.place;
          document.getElementById('weather-icon-box').textContent = icon;
        }
      } catch (err) {
        console.warn('Weather fallback to default:', err);
      }
    }

    // --- Handle Destination Dropdown Change ---
    function handleDestinationChange() {
      const destKey = document.getElementById('destination-select').value;
      appState.destination = destKey;
      const target = DESTINATIONS_DATA[destKey];
      if (target) {
        drawRouteWaypoints(target);
        fetchFreeWeather(target.weatherQuery);
        document.getElementById('trip-header-title').textContent = `${appState.origin} â†’ ${target.name} (${appState.days} Days, ${appState.travelers} Friends)`;
      }
    }

    // --- Multi-Agent Simulation Pipeline (Slide 05) ---
    function handleGeneratePlan(e) {
      e.preventDefault();
      
      const stepper = document.getElementById('ai-loading-stepper');
      const progressBar = document.getElementById('agent-progress-bar');
      const statusText = document.getElementById('agent-current-status');
      const percentText = document.getElementById('agent-percent');
      const analyseBtn = document.getElementById('analyse-btn');

      appState.origin = document.getElementById('origin-select').value;
      appState.destination = document.getElementById('destination-select').value;
      appState.travelers = parseInt(document.getElementById('travelers-select').value);
      appState.days = parseInt(document.getElementById('duration-select').value);

      stepper.classList.remove('hidden');
      analyseBtn.disabled = true;

      // Pipeline stages from Slide 05: LLM -> Travel Data -> Budget Checks -> Complete
      const stages = [
        { pct: 30, text: 'Agent 1: LLM understanding travel constraints & budget caps...' },
        { pct: 60, text: 'Agent 2: Checking Open-Meteo weather & route optimization...' },
        { pct: 85, text: 'Agent 3: Validating stay & activity partner pricing...' },
        { pct: 100, text: 'Complete: Realistic Tripzy plan ready!' }
      ];

      let currentStep = 0;
      const interval = setInterval(() => {
        if (currentStep < stages.length) {
          const s = stages[currentStep];
          progressBar.style.width = `${s.pct}%`;
          percentText.textContent = `${s.pct}%`;
          statusText.innerHTML = `<span class="w-2 h-2 rounded-full bg-brand-400 animate-ping mr-2"></span>${s.text}`;
          currentStep++;
        } else {
          clearInterval(interval);
          setTimeout(() => {
            stepper.classList.add('hidden');
            analyseBtn.disabled = false;
            updateBudgetAllocations(appState.budget);
            handleDestinationChange();
            renderDayContent(1);
            lucide.createIcons();
          }, 400);
        }
      }, 350);
    }

    // --- Slide 05 Dynamic Re-plan / Rain Simulation ---
    function toggleWeatherDisruption() {
      appState.isRaining = !appState.isRaining;
      const btn = document.getElementById('replan-weather-btn');
      const btnText = document.getElementById('replan-btn-text');
      const statusBadge = document.getElementById('weather-status-badge');
      const packing = document.getElementById('packing-advice');

      if (appState.isRaining) {
        btn.classList.add('bg-amber-500/20', 'border-amber-500/40', 'text-amber-300');
        btnText.textContent = 'Active: Rain Disruption Mode';
        statusBadge.textContent = 'High Water â€¢ Rafting Replaced';
        statusBadge.className = 'font-bold text-amber-400';
        packing.innerHTML = `<i data-lucide="alert-triangle" class="w-3.5 h-3.5 text-amber-400 mt-0.5 shrink-0"></i><span>Tripzy Re-planner Triggered: Outdoor water rafting replaced with indoor sound healing & culinary workshops.</span>`;
      } else {
        btn.classList.remove('bg-amber-500/20', 'border-amber-500/40', 'text-amber-300');
        btnText.textContent = 'Simulate Heavy Rain';
        statusBadge.textContent = 'Ideal for Rafting';
        statusBadge.className = 'font-bold text-emerald-400';
        packing.innerHTML = `<i data-lucide="info" class="w-3.5 h-3.5 text-brand-400 mt-0.5 shrink-0"></i><span>Recommended pack: Quick-dry shorts, waterproof phone pouch, comfortable hiking shoes, light evening windcheater.</span>`;
      }
      renderDayContent(appState.activeDay);
      lucide.createIcons();
    }

    // --- Render Multi-Day Itinerary ---
    function renderDayContent(dayNum) {
      appState.activeDay = dayNum;
      
      // Update Day tab styles
      for (let i = 1; i <= 4; i++) {
        const tab = document.getElementById(`day-tab-${i}`);
        if (tab) {
          if (i === dayNum) {
            tab.className = 'px-3 py-1 rounded-lg text-xs font-semibold bg-brand-500 text-white transition';
          } else {
            tab.className = 'px-3 py-1 rounded-lg text-xs font-semibold text-gray-400 hover:text-white transition';
          }
        }
      }

      const weatherMode = appState.isRaining ? 'rainy' : 'sunny';
      const dayData = (ITINERARIES_DB.Uttarakhand[weatherMode] && ITINERARIES_DB.Uttarakhand[weatherMode][dayNum]) 
        ? ITINERARIES_DB.Uttarakhand[weatherMode][dayNum] 
        : ITINERARIES_DB.Uttarakhand.sunny[1];

      const container = document.getElementById('day-itinerary-content');
      container.innerHTML = dayData.map(item => `
        <div class="flex items-start space-x-3.5 p-3.5 rounded-xl bg-deck-card/70 border border-deck-cardBorder hover:border-brand-500/40 transition">
          <div class="w-16 shrink-0 font-mono text-[11px] font-bold text-brand-400 pt-0.5">${item.time}</div>
          <div class="flex-1">
            <div class="flex items-center justify-between">
              <h5 class="text-xs sm:text-sm font-bold text-white">${item.title}</h5>
              <span class="font-mono text-xs text-gray-300 font-semibold">${item.cost}</span>
            </div>
            <p class="text-[11px] text-gray-400 mt-1 leading-relaxed">${item.desc}</p>
          </div>
        </div>
      `).join('');
    }

    // --- Slide 08 Interactive Monetization Calculator ---
    function calculatePlatformRevenue() {
      const trips = parseInt(document.getElementById('rev-trips-slider').value);
      const size = parseInt(document.getElementById('rev-size-slider').value);
      const rate = parseInt(document.getElementById('rev-rate-slider').value);

      document.getElementById('rev-trips-label').textContent = `${trips.toLocaleString('en-IN')} Trips`;
      document.getElementById('rev-size-label').textContent = `â‚¹${size.toLocaleString('en-IN')}`;
      document.getElementById('rev-rate-label').textContent = `${rate}%`;

      const gmv = trips * size;
      const commission = Math.round(gmv * (rate / 100));
      const partnerPayout = gmv - commission;
      const perTripEarn = Math.round(size * (rate / 100));

      document.getElementById('projected-revenue-total').textContent = `â‚¹${commission.toLocaleString('en-IN')}`;
      document.getElementById('projected-gmv-total').textContent = `Gross Merchandise Value: â‚¹${(gmv / 10000000).toFixed(2)} Cr / month`;
      document.getElementById('projected-partner-payout').textContent = `â‚¹${(partnerPayout / 10000000).toFixed(2)} Cr`;
      document.getElementById('projected-per-trip').textContent = `â‚¹${perTripEarn} / trip`;
    }

    // --- Instant Partner Booking Modal Handlers ---
    function openBookingModal(itemName, price, type) {
      document.getElementById('modal-item-name').textContent = itemName;
      document.getElementById('modal-base-price').textContent = `â‚¹${price.toLocaleString('en-IN')}`;
      document.getElementById('modal-platform-fee').textContent = `â‚¹${Math.round(price * 0.1).toLocaleString('en-IN')} (10% Inc.)`;
      document.getElementById('modal-total-price').textContent = `â‚¹${price.toLocaleString('en-IN')}`;
      
      document.getElementById('booking-payment-state').classList.remove('hidden');
      document.getElementById('booking-success-state').classList.add('hidden');
      document.getElementById('booking-modal').classList.remove('hidden');
      document.getElementById('booking-modal').classList.add('flex');
    }

    function closeBookingModal() {
      document.getElementById('booking-modal').classList.add('hidden');
      document.getElementById('booking-modal').classList.remove('flex');
    }

    function executeDummyPayment() {
      const btn = document.getElementById('modal-pay-btn');
      btn.innerHTML = `<span class="w-4 h-4 rounded-full border-2 border-white border-t-transparent animate-spin mr-2"></span> Processing via Escrow...`;
      btn.disabled = true;

      setTimeout(() => {
        btn.disabled = false;
        btn.innerHTML = `<i data-lucide="lock" class="w-4 h-4 mr-2"></i><span>Pay & Confirm Reservation</span>`;
        document.getElementById('booking-payment-state').classList.add('hidden');
        document.getElementById('booking-success-state').classList.remove('hidden');
        lucide.createIcons();
      }, 1200);
    }

    // --- Gemini API Key Modal Handlers ---
    function toggleConfigModal() {
      const modal = document.getElementById('config-modal');
      modal.classList.toggle('hidden');
      modal.classList.toggle('flex');
      if (appState.geminiKey) {
        document.getElementById('gemini-api-key').value = appState.geminiKey;
      }
    }

    function saveApiKey() {
      const key = document.getElementById('gemini-api-key').value.trim();
      appState.geminiKey = key;
      localStorage.setItem('tripzy_gemini_key', key);
      toggleConfigModal();
    }
  </script>
</body>
</html>
