```html
<!DOCTYPE html>
<html lang="ur" dir="ltr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI DP & Poster Studio</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">

  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['"Plus Jakarta Sans"', 'sans-serif'],
          },
          colors: {
            brand: {
              50: '#f5f3ff',
              100: '#ede9fe',
              500: '#8b5cf6',
              600: '#7c3aed',
              700: '#6d28d9',
            }
          }
        }
      }
    }
  </script>

  <style>
    /* Custom subtle animations */
    @keyframes pulseGlow {
      0%, 100% { opacity: 0.4; transform: scale(1); }
      50% { opacity: 0.8; transform: scale(1.05); }
    }
    .animate-glow {
      animation: pulseGlow 4s ease-in-out infinite;
    }
    .glass-card {
      background: rgba(255, 255, 255, 0.04);
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      border: 1px solid rgba(255, 255, 255, 0.08);
    }
    .custom-scrollbar::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    .custom-scrollbar::-webkit-scrollbar-track {
      background: rgba(15, 23, 42, 0.6);
    }
    .custom-scrollbar::-webkit-scrollbar-thumb {
      background: rgba(139, 92, 246, 0.3);
      border-radius: 9999px;
    }
    .custom-scrollbar::-webkit-scrollbar-thumb:hover {
      background: rgba(139, 92, 246, 0.6);
    }
  </style>
</head>
<body class="bg-[#090d16] text-slate-100 min-h-screen font-sans selection:bg-purple-500/30 selection:text-purple-200 custom-scrollbar flex flex-col justify-between">

  <!-- Background decorative glows -->
  <div class="fixed inset-0 pointer-events-none overflow-hidden z-0">
    <div class="absolute -top-40 -left-40 w-96 h-96 bg-purple-600/20 rounded-full blur-[128px] animate-glow"></div>
    <div class="absolute top-1/3 -right-40 w-[30rem] h-[30rem] bg-indigo-600/15 rounded-full blur-[140px] animate-glow" style="animation-delay: 2s;"></div>
    <div class="absolute -bottom-40 left-1/3 w-96 h-96 bg-pink-600/15 rounded-full blur-[128px]"></div>
  </div>

  <!-- Notification Toast -->
  <div id="toast" class="fixed top-5 right-5 z-50 flex items-center gap-3 px-4 py-3 rounded-xl shadow-2xl transition-all duration-300 transform translate-y-[-100px] opacity-0 pointer-events-none">
    <i id="toast-icon" data-lucide="info" class="w-5 h-5 flex-shrink-0"></i>
    <span id="toast-message" class="text-sm font-medium">Notification</span>
  </div>

  <!-- Main Content Wrap -->
  <div class="relative z-10 flex-1 flex flex-col">
    <!-- Navbar -->
    <header class="border-b border-white/5 bg-[#0b101c]/80 backdrop-blur-md sticky top-0 z-40">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 py-4 flex items-center justify-between">
        <div class="flex items-center gap-3">
          <div class="h-10 w-10 rounded-xl bg-gradient-to-tr from-purple-600 via-indigo-600 to-pink-500 flex items-center justify-center shadow-lg shadow-purple-500/20">
            <i data-lucide="sparkles" class="w-5 h-5 text-white"></i>
          </div>
          <div>
            <h1 class="font-bold text-lg leading-tight bg-gradient-to-r from-white via-slate-200 to-purple-300 bg-clip-text text-transparent">AI DP & Poster Studio</h1>
            <p class="text-xs text-slate-400">Apna prompt likhein, DP ya Poster banayein</p>
          </div>
        </div>

        <div class="flex items-center gap-3">
          <span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-medium bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">
            <span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse"></span>
            Gemini Flash 3.1 Ready
          </span>
        </div>
      </div>
    </header>

    <!-- Main Workspace -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 py-6 sm:py-8 flex-1 grid grid-cols-1 lg:grid-cols-12 gap-8">
      
      <!-- Left Column: Controls & Prompt Input -->
      <section class="lg:col-span-5 flex flex-col gap-6">
        
        <!-- Prompt Box Card -->
        <div class="glass-card rounded-2xl p-5 sm:p-6 shadow-xl relative overflow-hidden">
          <div class="flex items-center justify-between mb-3">
            <label for="promptInput" class="text-sm font-semibold text-slate-200 flex items-center gap-2">
              <i data-lucide="message-square" class="w-4 h-4 text-purple-400"></i>
              Aapka Prompt (Idea)
            </label>
            <button id="surpriseBtn" class="text-xs text-purple-400 hover:text-purple-300 transition-colors flex items-center gap-1 bg-purple-500/10 hover:bg-purple-500/20 px-2.5 py-1 rounded-lg border border-purple-500/20">
              <i data-lucide="dices" class="w-3.5 h-3.5"></i>
              Surprise Me
            </button>
          </div>

          <div class="relative">
            <textarea 
              id="promptInput" 
              rows="4" 
              placeholder="Jaise: 'A futuristic lion in cyberpunk neon city with glowing golden crown, 8k cinematic lighting' ya 'Vintage travel poster of Hunza Valley with bold typography'..."
              class="w-full bg-slate-900/80 text-sm text-slate-100 rounded-xl p-3.5 border border-slate-700/60 focus:border-purple-500 focus:ring-2 focus:ring-purple-500/20 outline-none transition-all resize-none placeholder:text-slate-500"
            ></textarea>
          </div>

          <!-- Quick enhancement tags -->
          <div class="mt-3 flex items-center gap-2 flex-wrap">
            <span class="text-xs text-slate-400 flex items-center gap-1"><i data-lucide="wand-2" class="w-3 h-3 text-amber-400"></i> Add:</span>
            <button class="quick-tag text-xs bg-slate-800/80 hover:bg-slate-700 text-slate-300 px-2.5 py-0.5 rounded-md border border-slate-700/60 transition">+ Ultra HD</button>
            <button class="quick-tag text-xs bg-slate-800/80 hover:bg-slate-700 text-slate-300 px-2.5 py-0.5 rounded-md border border-slate-700/60 transition">+ Cinematic Light</button>
            <button class="quick-tag text-xs bg-slate-800/80 hover:bg-slate-700 text-slate-300 px-2.5 py-0.5 rounded-md border border-slate-700/60 transition">+ Vibrant Neon</button>
            <button class="quick-tag text-xs bg-slate-800/80 hover:bg-slate-700 text-slate-300 px-2.5 py-0.5 rounded-md border border-slate-700/60 transition">+ Clean Aesthetic</button>
          </div>
        </div>

        <!-- Format & Ratio Selection -->
        <div class="glass-card rounded-2xl p-5 sm:p-6 shadow-xl">
          <label class="text-sm font-semibold text-slate-200 flex items-center gap-2 mb-3">
            <i data-lucide="layout-grid" class="w-4 h-4 text-purple-400"></i>
            Design Format (Select Aspect Ratio)
          </label>

          <div class="grid grid-cols-2 sm:grid-cols-4 gap-2.5" id="ratioSelector">
            <!-- 1:1 DP / Avatar -->
            <button type="button" data-ratio="1:1" data-label="DP / Profile Pic" data-sub="WhatsApp, Insta DP" class="ratio-btn active group relative p-3 rounded-xl border border-purple-500 bg-purple-500/15 text-left transition-all">
              <div class="flex items-center justify-between mb-2">
                <span class="text-xs font-bold text-white group-hover:text-purple-300">1:1</span>
                <div class="w-5 h-5 rounded-md border-2 border-purple-400 flex items-center justify-center">
                  <div class="w-2.5 h-2.5 bg-purple-400 rounded-sm"></div>
                </div>
              </div>
              <div class="text-xs font-semibold text-slate-200">DP / Profile</div>
              <div class="text-[10px] text-slate-400 truncate">Square Avatar</div>
            </button>

            <!-- 3:4 Poster -->
            <button type="button" data-ratio="3:4" data-label="Vertical Poster" data-sub="Flyer & Print" class="ratio-btn group relative p-3 rounded-xl border border-slate-800 bg-slate-900/60 hover:bg-slate-800/50 text-left transition-all">
              <div class="flex items-center justify-between mb-2">
                <span class="text-xs font-bold text-white group-hover:text-purple-300">3:4</span>
                <div class="w-4 h-5 rounded-sm border-2 border-slate-500 flex items-center justify-center">
                  <div class="w-2 h-3 bg-slate-500 rounded-sm"></div>
                </div>
              </div>
              <div class="text-xs font-semibold text-slate-200">Classic Poster</div>
              <div class="text-[10px] text-slate-400 truncate">Event, Promo</div>
            </button>

            <!-- 9:16 Story / Reel -->
            <button type="button" data-ratio="9:16" data-label="Full Story Poster" data-sub="TikTok, Reels, Status" class="ratio-btn group relative p-3 rounded-xl border border-slate-800 bg-slate-900/60 hover:bg-slate-800/50 text-left transition-all">
              <div class="flex items-center justify-between mb-2">
                <span class="text-xs font-bold text-white group-hover:text-purple-300">9:16</span>
                <div class="w-3.5 h-6 rounded border-2 border-slate-500 flex items-center justify-center">
                  <div class="w-1.5 h-4 bg-slate-500 rounded-xs"></div>
                </div>
              </div>
              <div class="text-xs font-semibold text-slate-200">Story / Reel</div>
              <div class="text-[10px] text-slate-400 truncate">Vertical Screen</div>
            </button>

            <!-- 16:9 Banner -->
            <button type="button" data-ratio="16:9" data-label="Horizontal Banner" data-sub="YouTube / Web" class="ratio-btn group relative p-3 rounded-xl border border-slate-800 bg-slate-900/60 hover:bg-slate-800/50 text-left transition-all">
              <div class="flex items-center justify-between mb-2">
                <span class="text-xs font-bold text-white group-hover:text-purple-300">16:9</span>
                <div class="w-6 h-3.5 rounded-sm border-2 border-slate-500 flex items-center justify-center">
                  <div class="w-3.5 h-1.5 bg-slate-500 rounded-xs"></div>
                </div>
              </div>
              <div class="text-xs font-semibold text-slate-200">Banner / Land</div>
              <div class="text-[10px] text-slate-400 truncate">Header, Wall</div>
            </button>
          </div>
        </div>

        <!-- Style Themes -->
        <div class="glass-card rounded-2xl p-5 sm:p-6 shadow-xl">
          <label class="text-sm font-semibold text-slate-200 flex items-center justify-between mb-3">
            <span class="flex items-center gap-2">
              <i data-lucide="palette" class="w-4 h-4 text-purple-400"></i>
              Visual Style
            </span>
            <span id="currentStyleLabel" class="text-xs font-normal text-purple-300">Photorealistic</span>
          </label>

          <div class="grid grid-cols-3 gap-2" id="styleOptions">
            <button type="button" data-style="Photorealistic, hyper-detailed, 8k resolution, cinematic lighting, sharp focus" class="style-btn active text-xs font-medium py-2 px-3 rounded-xl border border-purple-500 bg-purple-500/20 text-purple-200 text-center transition">
              📸 Realistic
            </button>
            <button type="button" data-style="Bold Graphic Design Poster with expressive typography, minimal layout, flat colors, Swiss poster design style" class="style-btn text-xs font-medium py-2 px-3 rounded-xl border border-slate-800 bg-slate-900/60 text-slate-400 hover:text-slate-200 text-center transition">
              🎨 Graphic Poster
            </button>
            <button type="button" data-style="Cyberpunk futuristic aesthetic, vibrant neon glow lights, dark moody atmosphere, volumetric shadows" class="style-btn text-xs font-medium py-2 px-3 rounded-xl border border-slate-800 bg-slate-900/60 text-slate-400 hover:text-slate-200 text-center transition">
              ⚡ Cyber Neon
            </button>
            <button type="button" data-style="Japanese Anime art style, Makoto Shinkai aesthetic, vibrant sky, highly detailed illustration" class="style-btn text-xs font-medium py-2 px-3 rounded-xl border border-slate-800 bg-slate-900/60 text-slate-400 hover:text-slate-200 text-center transition">
              🎌 Anime Art
            </button>
            <button type="button" data-style="3D Pixar & Disney style render, soft volumetric lighting, cute smooth character design, Octane Render" class="style-btn text-xs font-medium py-2 px-3 rounded-xl border border-slate-800 bg-slate-900/60 text-slate-400 hover:text-slate-200 text-center transition">
              🧸 3D Render
            </button>
            <button type="button" data-style="Retro 80s vintage poster, distressed texture, muted retro color palette, synthwave vibes" class="style-btn text-xs font-medium py-2 px-3 rounded-xl border border-slate-800 bg-slate-900/60 text-slate-400 hover:text-slate-200 text-center transition">
              📻 Retro Vintage
            </button>
          </div>
        </div>

        <!-- Action Generate Button -->
        <button id="generateBtn" class="group relative w-full overflow-hidden rounded-2xl bg-gradient-to-r from-purple-600 via-indigo-600 to-pink-600 p-[1px] font-semibold text-white shadow-xl shadow-purple-600/25 transition-all duration-300 hover:shadow-purple-600/40 hover:scale-[1.01] active:scale-[0.99] disabled:opacity-60 disabled:cursor-not-allowed">
          <div class="relative flex items-center justify-center gap-2 rounded-2xl bg-[#0d1222]/90 py-4 px-6 transition duration-300 group-hover:bg-opacity-0">
            <i id="btnIcon" data-lucide="sparkle" class="w-5 h-5 transition-transform group-hover:rotate-12"></i>
            <span id="btnText" class="text-base tracking-wide font-bold">Design Create Karein</span>
          </div>
        </button>

      </section>

      <!-- Right Column: Canvas Preview, Actions, and History -->
      <section class="lg:col-span-7 flex flex-col gap-6">
        
        <!-- Preview Box -->
        <div class="glass-card rounded-2xl p-6 shadow-xl flex flex-col justify-between min-h-[480px]">
          <!-- Header Bar of Preview -->
          <div class="flex items-center justify-between border-b border-white/5 pb-4 mb-4">
            <div class="flex items-center gap-2">
              <span id="previewTag" class="px-2.5 py-1 text-xs rounded-lg font-semibold bg-purple-500/15 text-purple-300 border border-purple-500/20">
                1:1 DP Format
              </span>
              <span id="statusIndicator" class="text-xs text-slate-400">Ready</span>
            </div>

            <!-- Download & Share Controls -->
            <div class="flex items-center gap-2" id="actionControls" style="display: none;">
              <button id="dpCircleToggle" title="DP Circle Mask Preview" class="p-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 transition">
                <i data-lucide="circle" class="w-4 h-4"></i>
              </button>
              <button id="copyPromptBtn" title="Prompt Copy Karein" class="p-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 transition">
                <i data-lucide="copy" class="w-4 h-4"></i>
              </button>
              <a id="downloadBtn" download="ai-graphic.png" class="flex items-center gap-1.5 px-3.5 py-2 rounded-xl bg-purple-600 hover:bg-purple-500 text-white font-medium text-xs shadow-lg shadow-purple-600/30 transition">
                <i data-lucide="download" class="w-4 h-4"></i>
                Download
              </a>
            </div>
          </div>

          <!-- Preview Stage -->
          <div class="flex-1 flex items-center justify-center p-2 relative">
            
            <!-- Empty State -->
            <div id="emptyState" class="text-center max-w-sm py-12 flex flex-col items-center">
              <div class="w-20 h-20 rounded-3xl bg-slate-900/90 border border-white/10 flex items-center justify-center mb-4 shadow-inner">
                <i data-lucide="image" class="w-10 h-10 text-slate-500"></i>
              </div>
              <h3 class="font-bold text-base text-slate-200 mb-1">Koi Artwork Generate Nahi Hua</h3>
              <p class="text-xs text-slate-400 leading-relaxed">
                Left side par apna idea likhein, format (DP ya Poster) select karein aur "Design Create Karein" par click karein!
              </p>
            </div>

            <!-- Loading State -->
            <div id="loadingState" class="hidden flex-col items-center justify-center py-16 text-center">
              <div class="relative w-20 h-20 mb-6">
                <div class="absolute inset-0 rounded-full border-4 border-purple-500/20 border-t-purple-500 animate-spin"></div>
                <div class="absolute inset-2 rounded-full border-4 border-pink-500/20 border-b-pink-500 animate-spin" style="animation-direction: reverse; animation-duration: 1.5s;"></div>
                <div class="absolute inset-0 flex items-center justify-center">
                  <i data-lucide="sparkles" class="w-7 h-7 text-purple-400 animate-pulse"></i>
                </div>
              </div>
              <h4 class="font-bold text-slate-200 text-sm mb-1" id="loadingStageText">AI Canvas Par Paint Kar Raha Hai...</h4>
              <p class="text-xs text-slate-400 max-w-xs">High-definition graphics aur lighting ko polish kiya ja raha hai (Approx 4-6 seconds)</p>
            </div>

            <!-- Image Result Container -->
            <div id="resultContainer" class="hidden relative max-w-full flex items-center justify-center overflow-hidden transition-all duration-300">
              <img 
                id="generatedImage" 
                src="" 
                alt="Generated Output" 
                class="rounded-xl object-contain max-h-[500px] w-auto shadow-2xl border border-white/10 transition-all duration-300"
              />
              
              <!-- Circle Mask Overlay for DP testing -->
              <div id="circleMaskOverlay" class="absolute inset-0 pointer-events-none hidden items-center justify-center">
                <div class="w-full h-full max-w-[360px] max-h-[360px] rounded-full border-4 border-dashed border-purple-400/80 shadow-[0_0_0_9999px_rgba(0,0,0,0.6)]"></div>
              </div>
            </div>

          </div>

          <!-- Bottom Prompt Echo & Meta Info -->
          <div id="resultMeta" class="mt-4 pt-3 border-t border-white/5 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-2 text-xs text-slate-400" style="display: none;">
            <p class="truncate max-w-md"><span class="font-semibold text-slate-300">Prompt:</span> <span id="metaPromptText">...</span></p>
            <span class="flex items-center gap-1 text-[11px] bg-slate-800/80 px-2 py-1 rounded-md text-slate-400">
              <i data-lucide="check-check" class="w-3 h-3 text-emerald-400"></i> High Resolution
            </span>
          </div>

        </div>

        <!-- History Strip -->
        <div class="glass-card rounded-2xl p-5 shadow-xl">
          <div class="flex items-center justify-between mb-3">
            <h4 class="text-xs font-bold text-slate-300 uppercase tracking-wider flex items-center gap-1.5">
              <i data-lucide="history" class="w-3.5 h-3.5 text-purple-400"></i>
              Recent Creations
            </h4>
            <span id="hist# Bgrndr
