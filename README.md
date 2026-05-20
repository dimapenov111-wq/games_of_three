<!DOCTYPE html>
<html lang="ro">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Animal Co-Op Adventure</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Inter & Press Start 2P pentru aspect retro-modern -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&family=Press+Start+2P&display=swap" rel="stylesheet">
    <!-- FontAwesome pentru iconițe retro frumoase -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0b0f19;
            overflow-x: hidden;
            user-select: none;
        }
        .retro-font {
            font-family: 'Press Start 2P', monospace;
        }
        canvas {
            image-rendering: pixelated;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.7);
        }
        /* Stiluri custom pentru scrollbar retro */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #475569;
        }
    </style>
</head>
<body class="flex flex-col min-h-screen text-gray-100">

    <!-- Header / Nav bar -->
    <header class="bg-gray-950 border-b border-gray-900 py-4 px-6 shadow-md z-10">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row items-center justify-between gap-4">
            <div class="flex items-center gap-3">
                <div class="bg-indigo-600 p-2.5 rounded-xl shadow-lg shadow-indigo-500/30 animate-pulse">
                    <i class="fa-solid fa-users text-2xl text-white"></i>
                </div>
                <div>
                    <h1 class="text-xl font-extrabold tracking-wider bg-gradient-to-r from-emerald-400 via-teal-400 to-indigo-400 bg-clip-text text-transparent uppercase retro-font text-xs md:text-sm">
                        Animal Co-Op
                    </h1>
                    <p class="text-xs text-gray-400 mt-0.5 font-bold">Aventură Cooperativă, Parkour & Logică în Echipă</p>
                </div>
            </div>

            <!-- Setari Audio / Resetare -->
            <div class="flex items-center gap-3 text-xs md:text-sm">
                <button onclick="toggleAudio()" id="audioBtn" class="bg-gray-900 hover:bg-gray-800 text-gray-300 px-3 py-2 rounded-lg flex items-center gap-2 border border-gray-800 transition">
                    <i class="fa-solid fa-volume-high text-green-400" id="audioIcon"></i> Sunet: PORNIT
                </button>
                <button onclick="resetToMenu()" class="bg-red-950/40 hover:bg-red-950/60 border border-red-900 text-red-200 px-3 py-2 rounded-lg transition flex items-center gap-2">
                    <i class="fa-solid fa-rotate-left"></i> Meniu Principal
                </button>
            </div>
        </div>
    </header>

    <main class="flex-grow flex flex-col items-center justify-center p-4 max-w-7xl mx-auto w-full gap-6">
        
        <!-- Zona de Joc (Meniuri Selectie / Canvas) -->
        <div class="w-full flex flex-col items-center">
            
            <!-- MENU SCREEN (Player count) -->
            <div id="menuScreen" class="bg-gray-950/90 border border-gray-900 rounded-3xl p-8 max-w-2xl w-full text-center shadow-2xl backdrop-blur-md">
                <div class="mb-6">
                    <span class="px-3 py-1 bg-emerald-500/20 text-emerald-400 rounded-full text-xs font-semibold uppercase tracking-widest">Aventură Cooperativă Retro</span>
                </div>
                <h2 class="text-3xl md:text-5xl font-black mb-4 tracking-tight leading-none bg-gradient-to-b from-white to-gray-400 bg-clip-text text-transparent">
                    ALEGE JUCĂTORII
                </h2>
                <p class="text-gray-400 mb-8 max-w-md mx-auto text-sm md:text-base">
                    Fiecare jucător va controla un animal simpatic (Câine 🐶, Pisică 🐱 sau Rață 🦆) pe aceeași tastatură! Colaborați strâns pentru a depăși provocările.
                </p>

                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 mb-8">
                    <!-- 1 Jucător -->
                    <button onclick="selectPlayerCount(1)" class="group relative bg-gray-900 hover:bg-indigo-950/40 border-2 border-gray-800 hover:border-indigo-500 rounded-2xl p-6 transition-all duration-300 flex flex-col items-center gap-3 shadow-lg hover:-translate-y-1">
                        <div class="absolute -top-3 right-3 bg-indigo-500 text-white font-bold text-xs px-2 py-0.5 rounded-full shadow">Solo</div>
                        <div class="w-16 h-16 rounded-full bg-indigo-500/10 flex items-center justify-center text-indigo-400 group-hover:scale-110 transition-transform">
                            <i class="fa-solid fa-user text-3xl"></i>
                        </div>
                        <span class="font-bold text-lg text-white">1 Jucător</span>
                        <span class="text-xs text-gray-400">Antrenează-ți reflexele</span>
                    </button>

                    <!-- 2 Jucători -->
                    <button onclick="selectPlayerCount(2)" class="group relative bg-gray-900 hover:bg-pink-950/40 border-2 border-gray-800 hover:border-pink-500 rounded-2xl p-6 transition-all duration-300 flex flex-col items-center gap-3 shadow-lg hover:-translate-y-1">
                        <div class="absolute -top-3 right-3 bg-pink-500 text-white font-bold text-xs px-2 py-0.5 rounded-full shadow">Duo</div>
                        <div class="w-16 h-16 rounded-full bg-pink-500/10 flex items-center justify-center text-pink-400 group-hover:scale-110 transition-transform">
                            <i class="fa-solid fa-users text-3xl"></i>
                        </div>
                        <span class="font-bold text-lg text-white">2 Jucători</span>
                        <span class="text-xs text-gray-400">Dublu amuzament local</span>
                    </button>

                    <!-- 3 Jucători -->
                    <button onclick="selectPlayerCount(3)" class="group relative bg-gray-900 hover:bg-yellow-950/40 border-2 border-gray-800 hover:border-yellow-500 rounded-2xl p-6 transition-all duration-300 flex flex-col items-center gap-3 shadow-lg hover:-translate-y-1">
                        <div class="absolute -top-3 right-3 bg-yellow-500 text-gray-950 font-bold text-xs px-2 py-0.5 rounded-full shadow">Echipă</div>
                        <div class="w-16 h-16 rounded-full bg-yellow-500/10 flex items-center justify-center text-yellow-400 group-hover:scale-110 transition-transform">
                            <i class="fa-solid fa-users-rays text-3xl"></i>
                        </div>
                        <span class="font-bold text-lg text-white">3 Jucători</span>
                        <span class="text-xs text-gray-400">Cooperare maximă</span>
                    </button>
                </div>

                <!-- Panou info comenzi rapide -->
                <div class="bg-black/40 rounded-2xl p-4 border border-gray-900 text-left text-xs text-gray-400">
                    <h4 class="font-bold text-gray-200 mb-2 uppercase tracking-wider flex items-center gap-2">
                        <i class="fa-solid fa-keyboard text-emerald-400"></i> Setări Controale Tastatură:
                    </h4>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                        <div>
                            <span class="text-indigo-400 font-bold">Jucător 1 (Câine):</span>
                            <p class="mt-1"><kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">A</kbd> / <kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">D</kbd> - Mișcare</p>
                            <p><kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">W</kbd> / <kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">Spațiu</kbd> - Sări</p>
                        </div>
                        <div>
                            <span class="text-pink-400 font-bold">Jucător 2 (Pisică):</span>
                            <p class="mt-1"><kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">←</kbd> / <kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">→</kbd> - Mișcare</p>
                            <p><kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">↑</kbd> - Sări</p>
                        </div>
                        <div>
                            <span class="text-yellow-400 font-bold">Jucător 3 (Rață):</span>
                            <p class="mt-1"><kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">J</kbd> / <kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">L</kbd> - Mișcare</p>
                            <p><kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">I</kbd> - Sări</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- CHARACTER SELECTION SCREEN -->
            <div id="characterSelectScreen" class="hidden bg-gray-950/90 border border-gray-900 rounded-3xl p-8 max-w-4xl w-full text-center shadow-2xl backdrop-blur-md">
                <h2 class="text-3xl font-extrabold mb-2 tracking-tight text-white uppercase">Alegeți Personajele</h2>
                <p class="text-gray-400 mb-6 text-sm">Fiecare jucător activ trebuie să își confirme animalul preferat înainte de a porni.</p>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8" id="characterGrid">
                    <!-- Dinamic injectat de JS -->
                </div>

                <div class="flex flex-col sm:flex-row justify-center gap-4">
                    <button onclick="backToPlayers()" class="bg-gray-800 hover:bg-gray-700 text-gray-300 px-6 py-3 rounded-xl font-bold transition">
                        Înapoi
                    </button>
                    <button onclick="goToLevelSelect()" id="nextBtn" class="bg-gradient-to-r from-indigo-500 to-indigo-600 hover:from-indigo-400 hover:to-indigo-500 text-white px-8 py-3 rounded-xl font-extrabold shadow-lg shadow-indigo-500/25 transition transform hover:-translate-y-0.5">
                        SELECTEAZĂ NIVELUL <i class="fa-solid fa-arrow-right ml-2"></i>
                    </button>
                </div>
            </div>

            <!-- LEVEL SELECT MENU SCREEN -->
            <div id="levelSelectScreen" class="hidden bg-gray-950/90 border border-gray-900 rounded-3xl p-8 max-w-4xl w-full text-center shadow-2xl backdrop-blur-md">
                <span class="px-3 py-1 bg-indigo-500/20 text-indigo-400 rounded-full text-xs font-semibold uppercase tracking-widest mb-4 inline-block">Misiuni Disponibile</span>
                <h2 class="text-3xl font-extrabold mb-6 tracking-tight text-white uppercase">Alege Nivelul</h2>
                
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <!-- Nivelul 1 -->
                    <button onclick="selectLevelAndStart(1)" class="group bg-gray-900 hover:bg-indigo-950/40 border border-gray-800 hover:border-indigo-500 rounded-2xl p-5 transition-all duration-300 flex flex-col items-center text-center gap-3 shadow-lg hover:-translate-y-1">
                        <div class="w-12 h-12 rounded-xl bg-indigo-500/10 flex items-center justify-center text-indigo-400 font-extrabold text-xl">1</div>
                        <h4 class="font-bold text-white text-sm uppercase">Parkour Zig-Zag</h4>
                        <p class="text-xs text-gray-400">Apareți în dreapta-sus, ocoliți platforma centrală cu 3 țepi și urcați în zig-zag spre ușă.</p>
                    </button>
                    <!-- Nivelul 2 -->
                    <button onclick="selectLevelAndStart(2)" class="group bg-gray-900 hover:bg-pink-950/40 border border-gray-800 hover:border-pink-500 rounded-2xl p-5 transition-all duration-300 flex flex-col items-center text-center gap-3 shadow-lg hover:-translate-y-1">
                        <div class="w-12 h-12 rounded-xl bg-pink-500/10 flex items-center justify-center text-pink-400 font-extrabold text-xl">2</div>
                        <h4 class="font-bold text-white text-sm uppercase">Camera de Tortură</h4>
                        <p class="text-xs text-gray-400">Camere separate, cu câte un tun static ce trage spre stânga. Supraviețuiți 60 de secunde.</p>
                    </button>
                    <!-- Nivelul 3 -->
                    <button onclick="selectLevelAndStart(3)" class="group bg-gray-900 hover:bg-yellow-950/40 border border-gray-800 hover:border-yellow-500 rounded-2xl p-5 transition-all duration-300 flex flex-col items-center text-center gap-3 shadow-lg hover:-translate-y-1">
                        <div class="w-12 h-12 rounded-xl bg-yellow-500/10 flex items-center justify-center text-yellow-400 font-extrabold text-xl">3</div>
                        <h4 class="font-bold text-white text-sm uppercase">Labirint Pac-Man</h4>
                        <p class="text-xs text-gray-400">Labirint clasic. Fireballs apar pe rând din centru. Supraviețuiți 2 minute.</p>
                    </button>
                    <!-- Nivelul 4 -->
                    <button onclick="selectLevelAndStart(4)" class="group bg-gray-900 hover:bg-emerald-950/40 border border-gray-800 hover:border-emerald-500 rounded-2xl p-5 transition-all duration-300 flex flex-col items-center text-center gap-3 shadow-lg hover:-translate-y-1">
                        <div class="w-12 h-12 rounded-xl bg-emerald-500/10 flex items-center justify-center text-emerald-400 font-extrabold text-xl">4</div>
                        <h4 class="font-bold text-white text-sm uppercase">Recolta de Mere</h4>
                        <p class="text-xs text-gray-400">Strângeți mere în căldărușe și feriți-vă de ouăle alterate. Obiectiv: 100 de puncte.</p>
                    </button>
                </div>

                <div class="flex justify-center">
                    <button onclick="backToCharacters()" class="bg-gray-800 hover:bg-gray-700 text-gray-300 px-6 py-3 rounded-xl font-bold transition">
                        Înapoi la Personaje
                    </button>
                </div>
            </div>

            <!-- GAME PLAYING AREA -->
            <div id="gameArea" class="hidden w-full flex flex-col items-center">
                <!-- Info Level HUD -->
                <div class="w-full max-w-[960px] bg-gray-950 border border-gray-900 rounded-t-2xl p-4 flex flex-wrap justify-between items-center gap-4 shadow-lg">
                    <div class="flex items-center gap-3">
                        <span id="levelBadge" class="retro-font text-xs bg-indigo-600 text-white px-3 py-1.5 rounded-lg border border-indigo-400 shadow shadow-indigo-500/20">
                            NIVELUL 1
                        </span>
                        <div>
                            <h3 id="levelName" class="font-bold text-white text-base">Traseul Șerpuitor</h3>
                            <p id="levelInstructions" class="text-xs text-gray-400">Urcați prin sărituri precise, evitați țepii și sari în ușa de aur de sus!</p>
                        </div>
                    </div>

                    <!-- Indicator de stare cooperativ / Timer / Scor -->
                    <div class="flex items-center gap-6">
                        <div id="levelTimerContainer" class="hidden items-center gap-2 bg-black px-3 py-1.5 rounded-lg border border-gray-900">
                            <i class="fa-solid fa-stopwatch text-rose-500 animate-pulse text-lg"></i>
                            <span id="levelTimer" class="retro-font text-sm text-rose-400">60s</span>
                        </div>
                        <div id="levelScoreContainer" class="hidden items-center gap-2 bg-black px-3 py-1.5 rounded-lg border border-gray-900">
                            <i class="fa-solid fa-star text-yellow-400 text-lg"></i>
                            <span id="levelScore" class="retro-font text-sm text-yellow-400">0 / 100</span>
                        </div>
                        <!-- Contor Încercări -->
                        <div class="flex items-center gap-2 bg-black px-3 py-1.5 rounded-lg border border-gray-900">
                            <span class="text-xs text-gray-400 font-bold uppercase tracking-wider">Încercări:</span>
                            <div class="flex items-center gap-1">
                                <span id="triesCounter" class="retro-font text-sm text-rose-500 font-extrabold">0</span>
                                <i class="fa-solid fa-circle-info text-gray-500 ml-1 hover:text-gray-300 cursor-pointer" title="Nu aveți vieți limitate! Vă puteți juca de câte ori doriți."></i>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Canvas-ul de joc principal -->
                <div class="relative bg-gray-950 border-x border-b border-gray-900 rounded-b-2xl overflow-hidden shadow-2xl max-w-full">
                    <canvas id="gameCanvas" width="960" height="540" class="block w-full max-w-[960px] h-auto"></canvas>
                    
                    <!-- Overlay de Pauză / Succes Nivel -->
                    <div id="canvasOverlay" class="hidden absolute inset-0 bg-black/85 backdrop-blur-sm flex flex-col items-center justify-center p-6 text-center z-20">
                        <div id="overlayContent" class="max-w-md bg-gray-950 border border-gray-800 p-8 rounded-2xl shadow-2xl flex flex-col items-center gap-4">
                            <!-- Populat dinamic de JS -->
                        </div>
                    </div>
                </div>

                <div class="w-full max-w-[960px] mt-4 grid grid-cols-1 md:grid-cols-3 gap-3 bg-gray-950/60 p-4 rounded-xl border border-gray-900 text-center text-xs text-gray-400">
                    <p class="md:col-span-3 font-semibold text-gray-300">💡 Sfat: Puteți reporni nivelul curent oricând apăsând tasta <kbd class="px-1.5 py-0.5 bg-gray-800 rounded text-white border border-gray-700">R</kbd> sau alegeți alt nivel din meniu.</p>
                </div>
            </div>

        </div>

        <!-- Instrucțiuni detaliate despre nivele -->
        <div class="w-full bg-gray-950/60 border border-gray-900 rounded-2xl p-6 shadow-xl">
            <h3 class="font-extrabold text-lg text-white mb-4 flex items-center gap-2">
                <i class="fa-solid fa-map-location-dot text-indigo-400"></i> Ghidul Nivelelor de Joc (Animal Co-Op):
            </h3>
            <div class="grid grid-cols-1 md:grid-cols-4 gap-4 text-xs">
                <div class="bg-black/40 p-4 rounded-xl border border-gray-900">
                    <div class="flex items-center gap-2 mb-2">
                        <span class="w-5 h-5 rounded-full bg-indigo-500/20 text-indigo-400 font-bold flex items-center justify-center">1</span>
                        <h4 class="font-bold text-gray-200 uppercase">Parkour Zig-Zag</h4>
                    </div>
                    <p class="text-gray-400">Jucătorii încep pe un bloc în dreapta-sus. Coborâți, feriți-vă de platforma cu 3 ghimpi din centru și urcați în zig-zag până la ușă. Săriți în dreptul ușii pentru a intra!</p>
                </div>
                <div class="bg-black/40 p-4 rounded-xl border border-gray-900">
                    <div class="flex items-center gap-2 mb-2">
                        <span class="w-5 h-5 rounded-full bg-pink-500/20 text-pink-400 font-bold flex items-center justify-center">2</span>
                        <h4 class="font-bold text-gray-200 uppercase">Camera de Tortură</h4>
                    </div>
                    <p class="text-gray-400">Fiecare jucător are propria sa celulă. Tunurile statice din dreapta trag gloanțe la fiecare 2 secunde. Gloanțele dispar la coliziunea cu peretele stâng. Rezistați 60 de secunde!</p>
                </div>
                <div class="bg-black/40 p-4 rounded-xl border border-gray-900">
                    <div class="flex items-center gap-2 mb-2">
                        <span class="w-5 h-5 rounded-full bg-yellow-500/20 text-yellow-400 font-bold flex items-center justify-center">3</span>
                        <h4 class="font-bold text-gray-200 uppercase">Labirint Pac-Man</h4>
                    </div>
                    <p class="text-gray-400">Deplasați-vă liber într-un labirint Pac-Man retro. Bilele de foc (fireballs) apar una câte una direct din căsuța centrală a fantomelor. Colectați bananele pentru invincibilitate!</p>
                </div>
                <div class="bg-black/40 p-4 rounded-xl border border-gray-900">
                    <div class="flex items-center gap-2 mb-2">
                        <span class="w-5 h-5 rounded-full bg-emerald-500/20 text-emerald-400 font-bold flex items-center justify-center">4</span>
                        <h4 class="font-bold text-gray-200 uppercase">Recolta de Mere</h4>
                    </div>
                    <p class="text-gray-400">Prindeți merele în căldărușele voastre de pe cap pentru +1 punct. Feriți-vă de ouăle alterate care scad -5 puncte! Obiectiv: 100 de puncte în total.</p>
                </div>
            </div>
        </div>

    </main>

    <!-- Footer -->
    <footer class="bg-gray-950 border-t border-gray-900 py-6 text-center text-xs text-gray-600 mt-auto">
        <p>© 2026 Animal Co-Op. Creat în HTML5 Canvas & Tailwind.</p>
    </footer>

    <script>
        // CONFIGURARE JOC & STATE-URI
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // AUDIO GENERATION (Web Audio API)
        let audioCtx = null;
        let isAudioEnabled = true;

        function playSound(type) {
            if (!isAudioEnabled) return;
            try {
                if (!audioCtx) {
                    audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                }
                if (audioCtx.state === 'suspended') {
                    audioCtx.resume();
                }

                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);

                const now = audioCtx.currentTime;

                if (type === 'jump') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(150, now);
                    osc.frequency.exponentialRampToValueAtTime(600, now + 0.15);
                    gain.gain.setValueAtTime(0.12, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.15);
                    osc.start(now);
                    osc.stop(now + 0.15);
                } else if (type === 'hit') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(250, now);
                    osc.frequency.exponentialRampToValueAtTime(30, now + 0.25);
                    gain.gain.setValueAtTime(0.18, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.25);
                    osc.start(now);
                    osc.stop(now + 0.25);
                } else if (type === 'collect') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(523.25, now); // C5
                    osc.frequency.setValueAtTime(659.25, now + 0.08); // E5
                    osc.frequency.setValueAtTime(783.99, now + 0.16); // G5
                    gain.gain.setValueAtTime(0.08, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.3);
                    osc.start(now);
                    osc.stop(now + 0.3);
                } else if (type === 'banana_power') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(392.00, now);
                    osc.frequency.exponentialRampToValueAtTime(880.00, now + 0.4);
                    gain.gain.setValueAtTime(0.12, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.4);
                    osc.start(now);
                    osc.stop(now + 0.4);
                } else if (type === 'win') {
                    osc.type = 'sine';
                    const notes = [261.63, 329.63, 392.00, 523.25]; // C4, E4, G4, C5
                    notes.forEach((freq, idx) => {
                        osc.frequency.setValueAtTime(freq, now + idx * 0.1);
                    });
                    gain.gain.setValueAtTime(0.12, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.5);
                    osc.start(now);
                    osc.stop(now + 0.5);
                }
            } catch (err) {
                console.log("Audio Context Error: ", err);
            }
        }

        function toggleAudio() {
            isAudioEnabled = !isAudioEnabled;
            const icon = document.getElementById('audioIcon');
            const btn = document.getElementById('audioBtn');
            if (isAudioEnabled) {
                icon.className = 'fa-solid fa-volume-high text-green-400';
                btn.innerHTML = `<i class="fa-solid fa-volume-high text-green-400" id="audioIcon"></i> Sunet: PORNIT`;
                if (audioCtx) audioCtx.resume();
            } else {
                icon.className = 'fa-solid fa-volume-xmark text-rose-500';
                btn.innerHTML = `<i class="fa-solid fa-volume-xmark text-rose-500" id="audioIcon"></i> Sunet: OPRIT`;
            }
        }

        // DATE STARE JOC
        let numPlayers = 1;
        let players = [];
        let playerSelectionData = []; // Tipul ales pentru fiecare jucator
        let currentLevel = 1;
        let totalFails = 0; // Contor inlocuitor de vieti (fara limita)
        let isGameOver = false;
        let keysPressed = {};
        let levelTimerInterval = null;
        let secondsLeft = 0;
        let targetScore = 100;
        let level4Score = 0;

        // ENTITATI SPECIFICE NIVELELOR
        let platforms = [];
        let levelExit = { x: 80, y: 30, width: 50, height: 70, open: true };
        let obstacles = []; // Tepi, etc.
        
        // Nivel 2: Boxe, tunuri si proiectile
        let level2Boxes = []; 
        let level2Cannons = []; 
        let bullets = [];   

        // Nivel 3: Labirint, Fireballs si Banana
        let fireballs = []; 
        let bananaItem = null; 
        let bananaTimer = 0;   
        let teamInvincibleTime = 0; 
        let fireballSpawnTimer = 0; // Timer special pentru spawn secvential

        // Nivel 4: Mere si Ouă
        let fallingItems = []; 
        let treeSpawners = []; 

        // TIPURI DE PERSONAJE & CULORI
        const CHARACTER_TYPES = [
            { id: 'dog', name: 'Câine 🐶', color: '#D97706', secondary: '#F59E0B' },
            { id: 'cat', name: 'Pisică 🐱', color: '#EC4899', secondary: '#F472B6' },
            { id: 'duck', name: 'Rață 🦆', color: '#EAB308', secondary: '#FACC15' }
        ];

        // CONFIGURARE CONTROL TASTATURĂ PE JUCĂTORI
        const PLAYER_CONTROLS = {
            1: { left: 'KeyA', right: 'KeyD', jump: 'KeyW', altJump: 'Space' },
            2: { left: 'ArrowLeft', right: 'ArrowRight', jump: 'ArrowUp' },
            3: { left: 'KeyJ', right: 'KeyL', jump: 'KeyI' }
        };

        // ASCULTĂTORI EVENIMENTE TASTATURĂ
        window.addEventListener('keydown', (e) => {
            keysPressed[e.code] = true;
            
            // Repornire rapida cu R
            if (e.code === 'KeyR' && document.getElementById('gameArea').classList.contains('hidden') === false) {
                initLevel(currentLevel);
            }
        });

        window.addEventListener('keyup', (e) => {
            keysPressed[e.code] = false;
        });

        function selectPlayerCount(count) {
            numPlayers = count;
            document.getElementById('menuScreen').classList.add('hidden');
            document.getElementById('characterSelectScreen').classList.remove('hidden');
            playSound('collect');

            // Inițializăm selectiile
            playerSelectionData = [];
            for (let i = 1; i <= count; i++) {
                const defaultType = CHARACTER_TYPES[(i - 1) % CHARACTER_TYPES.length].id;
                playerSelectionData.push({
                    playerId: i,
                    type: defaultType
                });
            }

            renderCharacterSelectionGrid();
        }

        function backToPlayers() {
            document.getElementById('characterSelectScreen').classList.add('hidden');
            document.getElementById('menuScreen').classList.remove('hidden');
            playSound('hit');
        }

        function goToLevelSelect() {
            document.getElementById('characterSelectScreen').classList.add('hidden');
            document.getElementById('levelSelectScreen').classList.remove('hidden');
            playSound('collect');
        }

        function backToCharacters() {
            document.getElementById('levelSelectScreen').classList.add('hidden');
            document.getElementById('characterSelectScreen').classList.remove('hidden');
            playSound('hit');
        }

        function selectLevelAndStart(lvl) {
            document.getElementById('levelSelectScreen').classList.add('hidden');
            document.getElementById('gameArea').classList.remove('hidden');
            
            totalFails = 0;
            currentLevel = lvl;
            isGameOver = false;
            
            playSound('win');
            initLevel(currentLevel);
        }

        function renderCharacterSelectionGrid() {
            const grid = document.getElementById('characterGrid');
            grid.innerHTML = '';

            for (let i = 1; i <= numPlayers; i++) {
                const currentSel = playerSelectionData[i - 1];
                const charTypeObj = CHARACTER_TYPES.find(c => c.id === currentSel.type);
                
                let optionsHtml = '';
                CHARACTER_TYPES.forEach(ct => {
                    const isSelected = ct.id === currentSel.type;
                    optionsHtml += `
                        <button onclick="changePlayerCharacter(${i}, '${ct.id}')" 
                                class="flex-1 py-3 px-2 rounded-xl border text-xs font-bold transition flex flex-col items-center gap-1
                                ${isSelected ? 'bg-indigo-600 text-white border-indigo-400' : 'bg-gray-800 text-gray-400 border-gray-700 hover:bg-gray-700'}">
                            <span>${ct.name}</span>
                        </button>
                    `;
                });

                let previewColor = charTypeObj.color;
                let animalEmoji = charTypeObj.id === 'dog' ? '🐶' : charTypeObj.id === 'cat' ? '🐱' : '🦆';

                const card = document.createElement('div');
                card.className = "bg-gray-900 border border-gray-800 rounded-2xl p-6 flex flex-col items-center gap-4 shadow-inner";
                card.innerHTML = `
                    <div class="px-3 py-1 bg-gray-950 rounded-full border border-gray-800 text-xs text-indigo-400 font-extrabold">
                        JUCĂTOR ${i}
                    </div>
                    <div class="w-24 h-24 rounded-2xl flex items-center justify-center text-5xl shadow-lg border-4 transition-transform hover:scale-110" 
                         style="background-color: ${previewColor + '20'}; border-color: ${previewColor}">
                        ${animalEmoji}
                    </div>
                    <div class="text-sm font-black text-white">
                        Controlează: <span class="text-indigo-400">${charTypeObj.name}</span>
                    </div>
                    <div class="flex gap-2 w-full mt-2">
                        ${optionsHtml}
                    </div>
                    <div class="w-full text-left mt-3 pt-3 border-t border-gray-800 text-xs text-gray-400">
                        <span class="font-bold block mb-1 text-gray-300">Comenzi Jucător ${i}:</span>
                        ${i === 1 ? '⬅️ A / D pentru mișcare. ⬆️ W / Spațiu pentru sărit.' : ''}
                        ${i === 2 ? '⬅️ Săgeți Stânga/Dreapta. ⬆️ Săgeată Sus pentru sărit.' : ''}
                        ${i === 3 ? '⬅️ J / L pentru mișcare. ⬆️ I pentru sărit.' : ''}
                    </div>
                `;
                grid.appendChild(card);
            }
        }

        function changePlayerCharacter(playerId, typeId) {
            const index = playerSelectionData.findIndex(p => p.playerId === playerId);
            if (index !== -1) {
                playerSelectionData[index].type = typeId;
                playSound('collect');
                renderCharacterSelectionGrid();
            }
        }

        function resetToMenu() {
            clearInterval(levelTimerInterval);
            document.getElementById('gameArea').classList.add('hidden');
            document.getElementById('characterSelectScreen').classList.add('hidden');
            document.getElementById('levelSelectScreen').classList.add('hidden');
            document.getElementById('menuScreen').classList.remove('hidden');
            isGameOver = false;
        }

        // INIȚIALIZARE NIVEL SPECIFIC
        function initLevel(levelNum) {
            currentLevel = levelNum;
            isGameOver = false;
            keysPressed = {};
            
            platforms = [];
            obstacles = [];
            bullets = [];
            fireballs = [];
            fallingItems = [];
            level2Boxes = [];
            level2Cannons = [];
            bananaItem = null;
            teamInvincibleTime = 0;
            fireballSpawnTimer = 0;

            document.getElementById('triesCounter').innerText = totalFails;

            const badge = document.getElementById('levelBadge');
            const nameText = document.getElementById('levelName');
            const instructionsText = document.getElementById('levelInstructions');
            const timerContainer = document.getElementById('levelTimerContainer');
            const scoreContainer = document.getElementById('levelScoreContainer');

            timerContainer.classList.add('hidden');
            scoreContainer.classList.add('hidden');
            clearInterval(levelTimerInterval);

            // Generare jucatori
            players = [];
            playerSelectionData.forEach((pData, idx) => {
                const charType = CHARACTER_TYPES.find(c => c.id === pData.type);
                
                // Pozitii implicite de start
                let startX = 100;
                let startY = 400;

                if (levelNum === 1) {
                    // ÎNCEPEȚI ÎN DREAPTA SUS pe blocul dedicat
                    startX = 780 + (idx * 25);
                    startY = 50; 
                } else if (levelNum === 2) {
                    // Boxe separate, plasam jucatorul in mijlocul propriei sale boxe pe axa Y
                    const boxH = 400 / numPlayers;
                    startY = 70 + idx * boxH + (boxH - 35) - 32;
                    startX = 100;
                } else if (levelNum === 3) {
                    // Start în colțul din stânga-sus, coridorul Pac-Man liber
                    startX = 60 + (idx * 30);
                    startY = 60;
                } else if (levelNum === 4) {
                    startX = 120 + (idx * 150);
                    startY = 450;
                }

                players.push({
                    id: pData.playerId,
                    type: pData.type,
                    name: charType.name,
                    color: charType.color,
                    secondary: charType.secondary,
                    x: startX,
                    y: startY, 
                    vx: 0,
                    vy: 0,
                    width: 32,
                    height: 32,
                    speed: 4.5,
                    jumpForce: 10,
                    isGrounded: false,
                    score: 0,
                    completedLvl1: false // Variabila speciala pentru usa nivelul 1
                });
            });

            document.getElementById('canvasOverlay').classList.add('hidden');

            // --- NIVELUL 1 (Zig-Zag Revers, start Dreapta-Sus) ---
            if (levelNum === 1) {
                badge.innerText = "NIVELUL 1";
                badge.className = "retro-font text-xs bg-indigo-600 text-white px-3 py-1.5 rounded-lg border border-indigo-400";
                nameText.innerText = "Coborâre și Zig-Zag în Sus";
                instructionsText.innerText = "Coborâți din dreapta-sus, feriți-vă de platforma cu 3 ghimpi din centru și urcați spre ușa din stânga-sus! Săriți în ușă pentru a intra!";

                // Poziționare ușa sus-stânga pe o platformă finală sigură
                levelExit.x = 80;
                levelExit.y = 30; 
                levelExit.width = 50;
                levelExit.height = 70;
                levelExit.open = true;

                // 1. Platforma de pornire din dreapta sus (unde apar jucatorii)
                platforms.push({ x: 760, y: 110, width: 160, height: 20, type: 'ground' });

                // 2. Platforma centrală cu exact 3 ghimpi
                platforms.push({ x: 380, y: 220, width: 200, height: 20, type: 'ground' });
                obstacles.push({ x: 410, y: 200, width: 20, height: 20, type: 'spike' });
                obstacles.push({ x: 470, y: 200, width: 20, height: 20, type: 'spike' });
                obstacles.push({ x: 530, y: 200, width: 20, height: 20, type: 'spike' });

                // 3. Platforma joasă pe care trebuie să sară pentru a începe urcarea
                platforms.push({ x: 720, y: 460, width: 180, height: 20, type: 'ground' });

                // 4. Traseul Zig-Zag spre ușa din stânga-sus
                platforms.push({ x: 520, y: 390, width: 130, height: 20, type: 'ground' });
                platforms.push({ x: 290, y: 320, width: 130, height: 20, type: 'ground' });
                platforms.push({ x: 80,  y: 250, width: 130, height: 20, type: 'ground' });
                platforms.push({ x: 260, y: 160, width: 130, height: 20, type: 'ground' });
                
                // Platforma finală cu ușa
                platforms.push({ x: 40,  y: 100, width: 160, height: 20, type: 'ground' });

                // Groapă imensă de țepi la bază
                obstacles.push({ x: 0, y: 520, width: 720, height: 20, type: 'spike_pit' });

            // --- NIVELUL 2 (Boxe separate cu tunuri statice ce trag spre stânga) ---
            } else if (levelNum === 2) {
                badge.innerText = "NIVELUL 2";
                badge.className = "retro-font text-xs bg-pink-600 text-white px-3 py-1.5 rounded-lg border border-pink-400";
                nameText.innerText = "Boxe Separate (Rezistă Proiectilelor)";
                instructionsText.innerText = "Fiecare jucător e închis în cubul său. Feriți-vă de tunul din dreapta care trage orizontal. Rezistați 60 de secunde!";

                timerContainer.classList.remove('hidden');
                secondsLeft = 60;
                document.getElementById('levelTimer').innerText = secondsLeft + "s";

                levelExit.x = 880;
                levelExit.y = 440; 
                levelExit.open = false;

                const boxHeight = 400 / numPlayers;
                const startY = 70;

                for (let i = 0; i < numPlayers; i++) {
                    const py1 = startY + i * boxHeight;
                    const py2 = py1 + boxHeight - 15;
                    const px1 = 60;
                    const px2 = 900;

                    level2Boxes.push({
                        x1: px1,
                        x2: px2,
                        y1: py1,
                        y2: py2,
                        playerId: i + 1
                    });

                    // Adaugam un tun fix în dreapta fiecărei boxe
                    level2Cannons.push({
                        x: px2 - 25,
                        y: py2 - 22,
                        cooldown: Math.floor(Math.random() * 60)
                    });
                }

                levelTimerInterval = setInterval(() => {
                    secondsLeft--;
                    document.getElementById('levelTimer').innerText = secondsLeft + "s";
                    if (secondsLeft <= 0) {
                        clearInterval(levelTimerInterval);
                        playSound('win');
                        showOverlay(true, "VICTORIE!", "Ați supraviețuit timp de 60 de secunde în cuburile voastre! Trecem la Nivelul 3.", "Treci la Nivelul 3");
                    }
                }, 1000);

            // --- NIVELUL 3 (Harta Pac-Man, Invincibilitate Banana și Spawn Secvențial) ---
            } else if (levelNum === 3) {
                badge.innerText = "NIVELUL 3";
                badge.className = "retro-font text-xs bg-yellow-600 text-gray-950 px-3 py-1.5 rounded-lg border border-yellow-400";
                nameText.innerText = "Evadare în Labirintul Pac-Man";
                instructionsText.innerText = "Bilele de foc apar pe rând din centrul hărții și vă urmăresc. Prindeți banana pentru invincibilitate temporară! Supraviețuiți 2 minute!";

                timerContainer.classList.remove('hidden');
                secondsLeft = 120;
                document.getElementById('levelTimer').innerText = secondsLeft + "s";

                buildPacmanMaze();

                fireballs = []; // Începe complet gol
                fireballSpawnTimer = 0; // Se incrementează pentru spawn secvențial

                levelTimerInterval = setInterval(() => {
                    secondsLeft--;
                    document.getElementById('levelTimer').innerText = secondsLeft + "s";

                    if (secondsLeft <= 0) {
                        clearInterval(levelTimerInterval);
                        playSound('win');
                        showOverlay(true, "LABIRINT COMPLETAT!", "Sunteți maeștrii evadărilor! Pregătiți-vă pentru recolta de mere din Nivelul 4.", "Treci la Nivelul 4");
                    }
                }, 1000);

            // --- NIVELUL 4 (Prindere Mere si Caldarusi pe cap) ---
            } else if (levelNum === 4) {
                badge.innerText = "NIVELUL 4";
                badge.className = "retro-font text-xs bg-emerald-600 text-white px-3 py-1.5 rounded-lg border border-emerald-400";
                nameText.innerText = "Livada de Mere a Animalelor";
                instructionsText.innerText = "Prindeți merele verzi/roșii (+1 punct) cu căldărușa de pe cap. Feriți-vă de ouă (-5 puncte). Țintă: 100 puncte!";

                scoreContainer.classList.remove('hidden');
                level4Score = 0;
                document.getElementById('levelScore').innerText = level4Score + " / " + targetScore;

                platforms.push({ x: 0, y: 510, width: 960, height: 30, type: 'ground' }); 
                platforms.push({ x: 0, y: 0, width: 20, height: 540, type: 'ground' });   
                platforms.push({ x: 940, y: 0, width: 20, height: 540, type: 'ground' });  

                treeSpawners = [100, 200, 300, 400, 500, 600, 700, 800, 880];
            }
        }

        function buildPacmanMaze() {
            // Harta simetrica si bine structurata tip Pac-Man, cu "Ghost House" in centru (marcat cu 2)
            const map = [
                [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],
                [1,0,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0,0,1],
                [1,0,1,1,0,1,1,1,0,1,0,1,1,1,0,1,1,0,1],
                [1,0,1,1,0,1,1,1,0,1,0,1,1,1,0,1,1,0,1],
                [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
                [1,0,1,1,0,1,0,1,1,2,1,1,0,1,0,1,1,0,1], // 2 = Ghost House în centru
                [1,0,0,0,0,1,0,1,0,0,0,1,0,1,0,0,0,0,1],
                [1,0,1,1,0,1,0,1,1,1,1,1,0,1,0,1,1,0,1],
                [1,0,0,1,0,0,0,0,0,1,0,0,0,0,0,1,0,0,1],
                [1,1,0,1,0,1,1,1,0,1,0,1,1,1,0,1,0,1,1],
                [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]
            ];

            const cellW = 960 / 19;
            const cellH = 540 / 11;

            for (let r = 0; r < map.length; r++) {
                for (let c = 0; c < map[r].length; c++) {
                    if (map[r][c] === 1) {
                        platforms.push({
                            x: c * cellW,
                            y: r * cellH,
                            width: cellW,
                            height: cellH,
                            type: 'ground'
                        });
                    } else if (map[r][c] === 2) {
                        // Memorăm coordonatele pentru Ghost House pentru a desena designul special și a face spawn-ul
                        platforms.push({
                            x: c * cellW,
                            y: r * cellH,
                            width: cellW,
                            height: cellH,
                            type: 'ghost_house'
                        });
                    }
                }
            }
        }

        function drawPlayer(ctx, p) {
            ctx.fillStyle = "rgba(0,0,0,0.25)";
            ctx.beginPath();
            ctx.ellipse(p.x + p.width/2, p.y + p.height, p.width/2.2, 5, 0, 0, Math.PI * 2);
            ctx.fill();

            if (currentLevel === 3 && teamInvincibleTime > 0) {
                ctx.strokeStyle = '#facc15';
                ctx.lineWidth = 4;
                ctx.beginPath();
                ctx.arc(p.x + p.width/2, p.y + p.height/2, p.width * 0.8, 0, Math.PI * 2);
                ctx.stroke();
            }

            ctx.fillStyle = p.color;
            ctx.beginPath();
            ctx.roundRect(p.x, p.y, p.width, p.height, 10);
            ctx.fill();

            if (p.type === 'dog') {
                ctx.fillStyle = p.secondary;
                ctx.beginPath();
                ctx.roundRect(p.x - 3, p.y + 4, 6, 16, 3); 
                ctx.roundRect(p.x + p.width - 3, p.y + 4, 6, 16, 3); 
                ctx.fill();

                ctx.fillStyle = '#FFFFFF';
                ctx.beginPath();
                ctx.arc(p.x + 10, p.y + 12, 4, 0, Math.PI*2);
                ctx.arc(p.x + 22, p.y + 12, 4, 0, Math.PI*2);
                ctx.fill();

                ctx.fillStyle = '#111827';
                ctx.beginPath();
                ctx.arc(p.x + 10, p.y + 12, 2, 0, Math.PI*2);
                ctx.arc(p.x + 22, p.y + 12, 2, 0, Math.PI*2);
                ctx.fill();

                ctx.fillStyle = '#F3F4F6';
                ctx.beginPath();
                ctx.arc(p.x + 16, p.y + 20, 6, 0, Math.PI*2);
                ctx.fill();

                ctx.fillStyle = '#000000';
                ctx.beginPath();
                ctx.arc(p.x + 16, p.y + 18, 2.5, 0, Math.PI*2);
                ctx.fill();

            } else if (p.type === 'cat') {
                ctx.fillStyle = p.secondary;
                ctx.beginPath();
                ctx.moveTo(p.x + 2, p.y);
                ctx.lineTo(p.x + 10, p.y + 6);
                ctx.lineTo(p.x + 2, p.y + 10);
                ctx.closePath();
                ctx.fill();

                ctx.beginPath();
                ctx.moveTo(p.x + p.width - 2, p.y);
                ctx.lineTo(p.x + p.width - 10, p.y + 6);
                ctx.lineTo(p.x + p.width - 2, p.y + 10);
                ctx.closePath();
                ctx.fill();

                ctx.fillStyle = '#FEF08A';
                ctx.beginPath();
                ctx.arc(p.x + 9, p.y + 13, 4, 0, Math.PI*2);
                ctx.arc(p.x + 23, p.y + 13, 4, 0, Math.PI*2);
                ctx.fill();

                ctx.strokeStyle = '#111827';
                ctx.lineWidth = 1.5;
                ctx.beginPath();
                ctx.moveTo(p.x + 9, p.y + 10);
                ctx.lineTo(p.x + 9, p.y + 16);
                ctx.moveTo(p.x + 23, p.y + 10);
                ctx.lineTo(p.x + 23, p.y + 16);
                ctx.stroke();

                ctx.strokeStyle = '#E5E7EB';
                ctx.lineWidth = 1.5;
                ctx.beginPath();
                ctx.moveTo(p.x + 5, p.y + 20);
                ctx.lineTo(p.x - 3, p.y + 18);
                ctx.moveTo(p.x + 5, p.y + 22);
                ctx.lineTo(p.x - 3, p.y + 22);
                ctx.moveTo(p.x + p.width - 5, p.y + 20);
                ctx.lineTo(p.x + p.width + 3, p.y + 18);
                ctx.moveTo(p.x + p.width - 5, p.y + 22);
                ctx.lineTo(p.x + p.width + 3, p.y + 22);
                ctx.stroke();

                ctx.fillStyle = '#F472B6';
                ctx.beginPath();
                ctx.arc(p.x + 16, p.y + 18, 2, 0, Math.PI*2);
                ctx.fill();

            } else if (p.type === 'duck') {
                ctx.fillStyle = '#FFFFFF';
                ctx.beginPath();
                ctx.arc(p.x + 10, p.y + 11, 4.5, 0, Math.PI*2);
                ctx.arc(p.x + 22, p.y + 11, 4.5, 0, Math.PI*2);
                ctx.fill();

                ctx.fillStyle = '#111827';
                ctx.beginPath();
                ctx.arc(p.x + 10, p.y + 11, 2, 0, Math.PI*2);
                ctx.arc(p.x + 22, p.y + 11, 2, 0, Math.PI*2);
                ctx.fill();

                ctx.fillStyle = '#F97316';
                ctx.beginPath();
                ctx.roundRect(p.x + 8, p.y + 16, 16, 8, 4);
                ctx.fill();
            }

            // ADAUGARE CĂLDĂRUȘĂ PE CAP IN NIVELUL 4
            if (currentLevel === 4) {
                ctx.fillStyle = '#94a3b8'; 
                ctx.strokeStyle = '#475569';
                ctx.lineWidth = 2;

                ctx.beginPath();
                ctx.moveTo(p.x + 4, p.y - 12); 
                ctx.lineTo(p.x + p.width - 4, p.y - 12); 
                ctx.lineTo(p.x + p.width + 2, p.y - 2); 
                ctx.lineTo(p.x - 2, p.y - 2); 
                ctx.closePath();
                ctx.fill();
                ctx.stroke();

                ctx.strokeStyle = '#1e293b';
                ctx.lineWidth = 1;
                ctx.beginPath();
                ctx.arc(p.x + p.width/2, p.y - 2, p.width/2, Math.PI, 0, false);
                ctx.stroke();
            }

            // Indicator text deasupra jucătorului
            ctx.fillStyle = "rgba(11, 15, 25, 0.85)";
            ctx.beginPath();
            let textOffset = currentLevel === 4 ? 26 : 18;
            ctx.roundRect(p.x - 6, p.y - textOffset, p.width + 12, 14, 4);
            ctx.fill();

            ctx.fillStyle = "#FFFFFF";
            ctx.font = "bold 8px Inter";
            ctx.textAlign = "center";
            
            if (currentLevel === 1 && p.completedLvl1) {
                ctx.fillStyle = "#10b981";
                ctx.fillText(`P${p.id} OK ✓`, p.x + p.width/2, p.y - textOffset + 10);
            } else {
                ctx.fillText(`P${p.id}`, p.x + p.width/2, p.y - textOffset + 10);
            }
        }

        // DETECTARE COLIZIUNI
        function checkCollision(rect1, rect2) {
            return rect1.x < rect2.x + rect2.width &&
                   rect1.x + rect1.width > rect2.x &&
                   rect1.y < rect2.y + rect2.height &&
                   rect1.y + rect1.height > rect2.y;
        }

        function handlePlayerDeath() {
            totalFails++;
            document.getElementById('triesCounter').innerText = totalFails;
            playSound('hit');

            const savedLevel = currentLevel;
            initLevel(savedLevel);
        }

        function showOverlay(success, title, text, btnText) {
            const overlay = document.getElementById('canvasOverlay');
            const content = document.getElementById('overlayContent');

            let iconClass = success ? "fa-circle-check text-emerald-500 text-6xl" : "fa-triangle-exclamation text-amber-500 text-6xl";
            let btnClass = success ? 
                "bg-emerald-600 hover:bg-emerald-500 text-white font-extrabold px-6 py-3 rounded-xl transition shadow-md" :
                "bg-amber-600 hover:bg-amber-500 text-white font-extrabold px-6 py-3 rounded-xl transition shadow-md";

            let actionCall = success ? `nextLevel()` : `initLevel(${currentLevel})`;

            content.innerHTML = `
                <i class="fa-solid ${iconClass} mb-2 animate-bounce"></i>
                <h3 class="retro-font text-base md:text-lg text-white font-black">${title}</h3>
                <p class="text-xs md:text-sm text-gray-400 my-2">${text}</p>
                <button onclick="${actionCall}" class="${btnClass} mt-4">
                    ${btnText}
                </button>
            `;
            overlay.classList.remove('hidden');
        }

        function nextLevel() {
            if (currentLevel < 4) {
                initLevel(currentLevel + 1);
            } else {
                showOverlay(true, "VICTORIE ABSOLUTĂ!", "Ați finalizat cu succes toate cele 4 provocări! Felicitări echipei!", "Meniu Principal");
                currentLevel = 1;
                resetToMenu();
            }
        }

        function updateGame() {
            if (isGameOver || document.getElementById('gameArea').classList.contains('hidden')) return;

            // Invincibilitate echipa descrescatoare
            if (teamInvincibleTime > 0) {
                teamInvincibleTime--;
            }

            // --- NIVELUL 1: VERIFICARE APĂSARE SĂRITURĂ ÎN UȘĂ ---
            if (currentLevel === 1) {
                players.forEach(p => {
                    const controls = PLAYER_CONTROLS[p.id];
                    if (!controls) return;

                    const touchingExit = checkCollision(p, levelExit);
                    const pressedJump = keysPressed[controls.jump] || (controls.altJump ? keysPressed[controls.altJump] : false);

                    if (touchingExit && pressedJump && !p.completedLvl1) {
                        p.completedLvl1 = true;
                        playSound('collect');
                    }
                });

                // Toti jucatorii trebuie sa finalizeze
                let allCompleted = true;
                players.forEach(p => {
                    if (!p.completedLvl1) allCompleted = false;
                });

                if (allCompleted) {
                    playSound('win');
                    showOverlay(true, "NIVELUL 1 COMPLETAT!", "Sunteți o echipă minunată! Trecem la Nivelul 2 în boxele securizate.", "Treci la Nivelul 2");
                    isGameOver = true;
                }
            }

            // --- NIVELUL 2: INCHISOARE CUBURI & BULLETS SPRE STÂNGA ---
            if (currentLevel === 2) {
                level2Cannons.forEach((can, idx) => {
                    can.cooldown++;
                    if (can.cooldown >= 120 && secondsLeft > 0) { // O data la 2 secunde (120 de cadre la ~60fps)
                        can.cooldown = 0;
                        playSound('hit');

                        bullets.push({
                            x: can.x - 20,
                            y: can.y + 6,
                            vx: -5.5, 
                            vy: 0,
                            radius: 6,
                            boxId: idx 
                        });
                    }
                });

                // Miscare si stergere gloante la atingerea peretelui din stanga
                for (let i = bullets.length - 1; i >= 0; i--) {
                    let b = bullets[i];
                    b.x += b.vx;

                    const associatedBox = level2Boxes[b.boxId];
                    if (associatedBox && b.x - b.radius <= associatedBox.x1) {
                        bullets.splice(i, 1);
                        continue;
                    }

                    const playerInBox = players[b.boxId];
                    if (playerInBox) {
                        let dist = Math.hypot(b.x - (playerInBox.x + playerInBox.width/2), b.y - (playerInBox.y + playerInBox.height/2));
                        if (dist < b.radius + playerInBox.width/2) {
                            bullets.splice(i, 1);
                            handlePlayerDeath();
                            break;
                        }
                    }
                }
            }

            // --- NIVELUL 3: LOGICA BANANĂ & SPAWN SECVENȚIAL FIREBALLS (GHOSTS) ---
            if (currentLevel === 3) {
                // SPAWN SECVENȚIAL FIREBALLS (din Ghost House în centru)
                fireballSpawnTimer++;
                // Apare o bilă de foc nouă la fiecare 8 secunde, maxim 5 în total pe ecran
                if (fireballSpawnTimer >= 480 && fireballs.length < 5) {
                    fireballSpawnTimer = 0;
                    
                    // Centrul Ghost House-ului Pac-Man (aproximativ x: 480, y: 270)
                    const cellW = 960 / 19;
                    const cellH = 540 / 11;
                    const spawnX = 9 * cellW + cellW / 2;
                    const spawnY = 5 * cellH + cellH / 2;

                    fireballs.push({
                        x: spawnX,
                        y: spawnY,
                        vx: (Math.random() > 0.5 ? 2.2 : -2.2),
                        vy: (Math.random() > 0.5 ? 2.2 : -2.2),
                        radius: 12
                    });
                    playSound('hit');
                }

                // Spawn banană o dată la 30 de secunde
                bananaTimer++;
                if (bananaTimer >= 1800 && !bananaItem) {
                    bananaTimer = 0;
                    let validPos = false;
                    let bX = 200, bY = 200;
                    while (!validPos) {
                        // Evităm marginea exterioară a labirintului
                        bX = Math.floor(Math.random() * 17 + 1) * (960 / 19) + 12;
                        bY = Math.floor(Math.random() * 9 + 1) * (540 / 11) + 12;
                        
                        let overlaps = false;
                        const tempBanana = { x: bX, y: bY, width: 25, height: 25 };
                        for (let plat of platforms) {
                            if (checkCollision(tempBanana, plat)) {
                                overlaps = true;
                                break;
                            }
                        }
                        if (!overlaps) validPos = true;
                    }

                    bananaItem = { x: bX, y: bY, width: 25, height: 25 };
                    playSound('win');
                }

                // Verificare colectare banană
                if (bananaItem) {
                    for (let p of players) {
                        if (checkCollision(p, bananaItem)) {
                            bananaItem = null;
                            teamInvincibleTime = 480; // 8 secunde invincibilitate pentru toată echipa
                            playSound('banana_power');
                            break;
                        }
                    }
                }

                // Mișcare inteligentă / urmărire fireballs
                fireballs.forEach(f => {
                    let closestP = players[0];
                    let minDist = 99999;
                    players.forEach(p => {
                        let d = Math.hypot((p.x + p.width/2) - f.x, (p.y + p.height/2) - f.y);
                        if (d < minDist) {
                            minDist = d;
                            closestP = p;
                        }
                    });

                    // Urmărește activ dacă este aproape (< 280px)
                    if (minDist < 280) {
                        let angle = Math.atan2((closestP.y + closestP.height/2) - f.y, (closestP.x + closestP.width/2) - f.x);
                        f.vx += Math.cos(angle) * 0.15;
                        f.vy += Math.sin(angle) * 0.15;
                    } else {
                        // Deplasare fluidă semi-aleatoare
                        f.vx += (Math.random() - 0.5) * 0.1;
                        f.vy += (Math.random() - 0.5) * 0.1;
                    }

                    const maxSpeed = 3.2;
                    let speed = Math.hypot(f.vx, f.vy);
                    if (speed > maxSpeed) {
                        f.vx = (f.vx / speed) * maxSpeed;
                        f.vy = (f.vy / speed) * maxSpeed;
                    }

                    f.x += f.vx;
                    f.y += f.vy;

                    // Coliziune robustă fireball - pereți labirint
                    platforms.forEach(plat => {
                        if (f.x + f.radius > plat.x && f.x - f.radius < plat.x + plat.width &&
                            f.y + f.radius > plat.y && f.y - f.radius < plat.y + plat.height) {
                            
                            // Ricoșare elegantă
                            if (f.x - f.vx + f.radius <= plat.x || f.x - f.vx - f.radius >= plat.x + plat.width) {
                                f.vx = -f.vx * 0.9;
                            } else {
                                f.vy = -f.vy * 0.9;
                            }
                        }
                    });

                    // Limite ecran general
                    if (f.x - f.radius < 0 || f.x + f.radius > 960) f.vx = -f.vx;
                    if (f.y - f.radius < 0 || f.y + f.radius > 540) f.vy = -f.vy;

                    // Lovire jucători (dacă nu au invincibilitate activă)
                    if (teamInvincibleTime <= 0) {
                        players.forEach(p => {
                            let dist = Math.hypot(f.x - (p.x + p.width/2), f.y - (p.y + p.height/2));
                            if (dist < f.radius + p.width/2) {
                                handlePlayerDeath();
                            }
                        });
                    }
                });
            }

            // --- NIVELUL 4: SPAWN FRUCTE SI VERIFICARE CALDARUSE ---
            if (currentLevel === 4) {
                if (Math.random() < 0.04) {
                    const randomSpawnerX = treeSpawners[Math.floor(Math.random() * treeSpawners.length)];
                    const isApple = Math.random() > 0.3; 
                    
                    fallingItems.push({
                        x: randomSpawnerX + (Math.random() * 20 - 10),
                        y: 35,
                        vy: 2 + Math.random() * 2.5,
                        radius: isApple ? 9 : 11,
                        type: isApple ? (Math.random() > 0.5 ? 'red_apple' : 'green_apple') : 'bad_egg'
                    });
                }

                for (let i = fallingItems.length - 1; i >= 0; i--) {
                    let item = fallingItems[i];
                    item.y += item.vy;

                    let collected = false;
                    for (let p of players) {
                        // Centrul caldarusei de pe capul jucatorului
                        let bucketX = p.x + p.width/2;
                        let bucketY = p.y - 10;
                        let dist = Math.hypot(item.x - bucketX, item.y - bucketY);
                        
                        if (dist < item.radius + 15) {
                            if (item.type.includes('apple')) {
                                level4Score += 1;
                                playSound('collect');
                            } else {
                                level4Score -= 5;
                                if (level4Score < 0) level4Score = 0;
                                playSound('hit');
                            }
                            
                            document.getElementById('levelScore').innerText = level4Score + " / " + targetScore;
                            fallingItems.splice(i, 1);
                            collected = true;
                            break;
                        }
                    }

                    if (collected) continue;

                    if (item.y > 510) {
                        fallingItems.splice(i, 1);
                    }
                }

                if (level4Score >= targetScore) {
                    playSound('win');
                    showOverlay(true, "VICTORIE ECHIPĂ!", "Ați strâns 100 de puncte în căldărușe! Animal Co-Op completat cu succes!", "Meniu Principal");
                    currentLevel = 0;
                    isGameOver = true;
                }
            }

            // --- FIZICĂ STANDARD JUCĂTORI (AXE INDEPENDENTE) ---
            players.forEach((p, idx) => {
                const controls = PLAYER_CONTROLS[p.id];
                if (!controls) return;

                let moveX = 0;
                if (keysPressed[controls.left]) moveX = -p.speed;
                if (keysPressed[controls.right]) moveX = p.speed;

                p.vx = moveX;

                // Nivelul 3 functioneaza ca Pac-Man (fara gravitatie)
                if (currentLevel === 3) {
                    let moveY = 0;
                    if (keysPressed[controls.jump]) moveY = -p.speed;
                    if (p.id === 1 && keysPressed['KeyS']) moveY = p.speed;
                    if (p.id === 2 && keysPressed['ArrowDown']) moveY = p.speed;
                    if (p.id === 3 && keysPressed['KeyK']) moveY = p.speed;

                    p.vy = moveY;

                    // Coliziuni separate pe axele X și Y pentru alunecare perfectă
                    p.x += p.vx;
                    platforms.forEach(plat => {
                        if (checkCollision(p, plat)) {
                            if (p.vx > 0) p.x = plat.x - p.width;
                            if (p.vx < 0) p.x = plat.x + plat.width;
                        }
                    });

                    p.y += p.vy;
                    platforms.forEach(plat => {
                        if (checkCollision(p, plat)) {
                            if (p.vy > 0) p.y = plat.y - p.height;
                            if (p.vy < 0) p.y = plat.y + plat.height;
                        }
                    });

                    // Limite ecran general
                    if (p.x < 10) p.x = 10;
                    if (p.x + p.width > 950) p.x = 950 - p.width;
                    if (p.y < 10) p.y = 10;
                    if (p.y + p.height > 530) p.y = 530 - p.height;

                } else {
                    // --- FIZICA CLASICA CU GRAVITATIE (NIVELELE 1, 2, 4) ---
                    const gravity = 0.45;
                    p.vy += gravity;

                    p.x += p.vx;

                    if (currentLevel === 2) {
                        const box = level2Boxes[idx];
                        if (box) {
                            if (p.x < box.x1) p.x = box.x1;
                            if (p.x + p.width > box.x2) p.x = box.x2 - p.width;
                        }
                    }

                    platforms.forEach(plat => {
                        if (plat.type !== 'ghost_house') { // Evită coliziunea cu căsuța fantomelor dacă este nivel 1/2/4
                            if (checkCollision(p, plat)) {
                                if (p.vx > 0) p.x = plat.x - p.width;
                                if (p.vx < 0) p.x = plat.x + plat.width;
                            }
                        }
                    });

                    p.y += p.vy;
                    p.isGrounded = false;

                    if (currentLevel === 2) {
                        const box = level2Boxes[idx];
                        if (box) {
                            if (p.y < box.y1) {
                                p.y = box.y1;
                                p.vy = 0;
                            }
                            if (p.y + p.height > box.y2) {
                                p.y = box.y2 - p.height;
                                p.vy = 0;
                                p.isGrounded = true;
                            }
                        }
                    }

                    platforms.forEach(plat => {
                        if (plat.type !== 'ghost_house') {
                            if (checkCollision(p, plat)) {
                                if (p.vy > 0) { 
                                    p.y = plat.y - p.height;
                                    p.vy = 0;
                                    p.isGrounded = true;
                                } else if (p.vy < 0) { 
                                    p.y = plat.y + plat.height;
                                    p.vy = 0;
                                }
                            }
                        }
                    });

                    const jumpKey1 = keysPressed[controls.jump];
                    const jumpKey2 = controls.altJump ? keysPressed[controls.altJump] : false;

                    if ((jumpKey1 || jumpKey2) && p.isGrounded) {
                        p.vy = -p.jumpForce;
                        p.isGrounded = false;
                        playSound('jump');
                    }

                    if (p.x < 0) p.x = 0;
                    if (p.x + p.width > 960) p.x = 960 - p.width;
                }

                // Coliziune cu țepii
                obstacles.forEach(obs => {
                    if (checkCollision(p, obs)) {
                        handlePlayerDeath();
                    }
                });

                if (p.y > 540) {
                    handlePlayerDeath();
                }
            });
        }

        // RENDERING / DESENARE JOC PE CANVAS
        function renderGame() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // --- FUNDAL NIVEL 1 ---
            if (currentLevel === 1) {
                let grad = ctx.createLinearGradient(0, 0, 0, 540);
                grad.addColorStop(0, '#0f172a');
                grad.addColorStop(1, '#020617');
                ctx.fillStyle = grad;
                ctx.fillRect(0, 0, 960, 540);

                ctx.fillStyle = "rgba(255, 255, 255, 0.03)";
                ctx.beginPath();
                ctx.arc(200, 100, 40, 0, Math.PI * 2);
                ctx.arc(250, 90, 50, 0, Math.PI * 2);
                ctx.arc(300, 100, 40, 0, Math.PI * 2);
                ctx.fill();

            // --- FUNDAL NIVEL 2 ---
            } else if (currentLevel === 2) {
                ctx.fillStyle = '#080710';
                ctx.fillRect(0, 0, 960, 540);

                level2Boxes.forEach(box => {
                    ctx.strokeStyle = 'rgba(244, 63, 94, 0.25)';
                    ctx.lineWidth = 2;
                    ctx.strokeRect(box.x1, box.y1, box.x2 - box.x1, box.y2 - box.y1);

                    ctx.fillStyle = 'rgba(255, 255, 255, 0.02)';
                    ctx.font = 'bold 30px Press Start 2P';
                    ctx.textAlign = 'center';
                    ctx.fillText(`CUB P${box.playerId}`, (box.x1 + box.x2)/2, (box.y1 + box.y2)/2 + 10);
                });

            // --- FUNDAL NIVEL 3 (Pac-Man Retro Dark Blue) ---
            } else if (currentLevel === 3) {
                ctx.fillStyle = '#030310';
                ctx.fillRect(0, 0, 960, 540);

            // --- FUNDAL NIVEL 4 ---
            } else if (currentLevel === 4) {
                let grad = ctx.createLinearGradient(0, 0, 0, 540);
                grad.addColorStop(0, '#075985');
                grad.addColorStop(0.5, '#38bdf8');
                grad.addColorStop(1, '#115e59');
                ctx.fillStyle = grad;
                ctx.fillRect(0, 0, 960, 540);

                // Trunchi mare de copac pe fundal
                ctx.fillStyle = '#542d13'; 
                ctx.fillRect(450, 120, 60, 390); 

                ctx.beginPath();
                ctx.moveTo(450, 250);
                ctx.lineTo(250, 170);
                ctx.lineTo(250, 190);
                ctx.lineTo(450, 280);
                ctx.closePath();
                ctx.fill();

                ctx.beginPath();
                ctx.moveTo(510, 250);
                ctx.lineTo(710, 170);
                ctx.lineTo(710, 190);
                ctx.lineTo(510, 280);
                ctx.closePath();
                ctx.fill();

                ctx.fillStyle = '#14532d';
                ctx.beginPath();
                ctx.arc(480, 80, 190, 0, Math.PI * 2);
                ctx.fill();

                ctx.fillStyle = '#166534';
                ctx.beginPath();
                ctx.arc(320, 120, 140, 0, Math.PI * 2);
                ctx.arc(640, 120, 140, 0, Math.PI * 2);
                ctx.fill();
            }

            // --- DESENARE PLATFORME ---
            platforms.forEach(plat => {
                let borderCol = '#6366f1';
                let fillCol = '#1e1b4b';

                if (currentLevel === 2) {
                    borderCol = '#f43f5e';
                    fillCol = '#4c0519';
                } else if (currentLevel === 3) {
                    if (plat.type === 'ghost_house') {
                        // Design special pentru Casa Fantomelor din centru
                        borderCol = '#f472b6';
                        fillCol = 'rgba(244, 114, 182, 0.1)';
                        ctx.fillStyle = fillCol;
                        ctx.fillRect(plat.x, plat.y, plat.width, plat.height);
                        ctx.strokeStyle = borderCol;
                        ctx.lineWidth = 3;
                        ctx.strokeRect(plat.x, plat.y, plat.width, plat.height);
                        
                        // Desenăm o mică dungă de intrare pe deasupra
                        ctx.fillStyle = '#fbcfe8';
                        ctx.fillRect(plat.x + 10, plat.y, plat.width - 20, 4);
                        return;
                    } else {
                        // Labirint Pac-Man clasic, design de culoare albastră neon
                        borderCol = '#2563eb';
                        fillCol = '#0f172a';
                    }
                } else if (currentLevel === 4) {
                    borderCol = '#22c55e';
                    fillCol = '#052e16';
                }

                ctx.fillStyle = fillCol;
                ctx.beginPath();
                ctx.roundRect(plat.x, plat.y, plat.width, plat.height, 4);
                ctx.fill();

                ctx.strokeStyle = borderCol;
                ctx.lineWidth = 3;
                ctx.beginPath();
                ctx.moveTo(plat.x, plat.y);
                ctx.lineTo(plat.x + plat.width, plat.y);
                ctx.stroke();
            });

            // --- DESENARE ȚEPI / OBSTACOLE ---
            ctx.fillStyle = '#ef4444';
            obstacles.forEach(obs => {
                ctx.beginPath();
                let numSpikes = Math.ceil(obs.width / 12);
                let spikeWidth = obs.width / numSpikes;
                ctx.moveTo(obs.x, obs.y + obs.height);

                for (let s = 0; s < numSpikes; s++) {
                    ctx.lineTo(obs.x + (s * spikeWidth) + (spikeWidth / 2), obs.y);
                    ctx.lineTo(obs.x + (s + 1) * spikeWidth, obs.y + obs.height);
                }
                ctx.closePath();
                ctx.fill();
            });

            // --- DESENARE UȘĂ DE IEȘIRE (DOAR NIVELUL 1) ---
            if (currentLevel === 1) {
                ctx.fillStyle = 'rgba(251, 191, 36, 0.15)';
                ctx.beginPath();
                ctx.arc(levelExit.x + levelExit.width/2, levelExit.y + levelExit.height/2, 45, 0, Math.PI * 2);
                ctx.fill();

                ctx.fillStyle = '#fbbf24';
                ctx.beginPath();
                ctx.roundRect(levelExit.x, levelExit.y, levelExit.width, levelExit.height, [15, 15, 0, 0]);
                ctx.fill();

                ctx.strokeStyle = '#fff';
                ctx.lineWidth = 3;
                ctx.stroke();

                ctx.fillStyle = '#0f172a';
                ctx.font = '900 8px Inter';
                ctx.textAlign = 'center';
                ctx.fillText("SARI ÎN", levelExit.x + levelExit.width/2, levelExit.y + levelExit.height/2 - 2);
                ctx.fillText("UȘĂ", levelExit.x + levelExit.width/2, levelExit.y + levelExit.height/2 + 8);
            }

            // --- DESENARE TUNURI NEMIȘCATE SPRE STÂNGA (NIVEL 2) ---
            if (currentLevel === 2) {
                level2Cannons.forEach(can => {
                    ctx.fillStyle = '#64748b';
                    ctx.beginPath();
                    ctx.arc(can.x, can.y, 14, 0, Math.PI * 2);
                    ctx.fill();

                    ctx.fillStyle = '#1e293b';
                    ctx.fillRect(can.x - 24, can.y - 6, 24, 12);
                    ctx.strokeStyle = '#f43f5e';
                    ctx.lineWidth = 2;
                    ctx.strokeRect(can.x - 24, can.y - 6, 24, 12);
                });

                bullets.forEach(b => {
                    ctx.fillStyle = '#f43f5e';
                    ctx.beginPath();
                    ctx.arc(b.x, b.y, b.radius, 0, Math.PI * 2);
                    ctx.fill();

                    ctx.strokeStyle = '#fecdd3';
                    ctx.lineWidth = 2;
                    ctx.stroke();
                });
            }

            // --- DESENARE BANANĂ MAGICĂ & FIREBALLS (NIVEL 3) ---
            if (currentLevel === 3) {
                if (bananaItem) {
                    ctx.strokeStyle = '#facc15';
                    ctx.lineWidth = 6;
                    ctx.beginPath();
                    ctx.arc(bananaItem.x + 12, bananaItem.y + 12, 10, 0.2, Math.PI - 0.4, false);
                    ctx.stroke();

                    ctx.fillStyle = '#854d0e';
                    ctx.fillRect(bananaItem.x + 2, bananaItem.y + 16, 4, 4);

                    ctx.fillStyle = 'rgba(250, 204, 21, 0.25)';
                    ctx.beginPath();
                    ctx.arc(bananaItem.x + 12, bananaItem.y + 12, 16, 0, Math.PI * 2);
                    ctx.fill();
                }

                fireballs.forEach(f => {
                    let outerGrad = ctx.createRadialGradient(f.x, f.y, 2, f.x, f.y, f.radius);
                    outerGrad.addColorStop(0, '#FFFFFF');
                    outerGrad.addColorStop(0.3, '#f97316');
                    outerGrad.addColorStop(1, 'rgba(239, 68, 68, 0)');

                    ctx.fillStyle = outerGrad;
                    ctx.beginPath();
                    ctx.arc(f.x, f.y, f.radius * 1.6, 0, Math.PI * 2);
                    ctx.fill();

                    ctx.fillStyle = '#ef4444';
                    ctx.beginPath();
                    ctx.arc(f.x, f.y, f.radius * 0.75, 0, Math.PI * 2);
                    ctx.fill();
                });
            }

            // --- DESENARE MERE / OUĂ (NIVEL 4) ---
            if (currentLevel === 4) {
                fallingItems.forEach(item => {
                    if (item.type.includes('apple')) {
                        ctx.fillStyle = item.type === 'red_apple' ? '#ef4444' : '#22c55e';
                        ctx.beginPath();
                        ctx.arc(item.x, item.y, item.radius, 0, Math.PI * 2);
                        ctx.fill();

                        ctx.strokeStyle = '#78350f';
                        ctx.lineWidth = 1.5;
                        ctx.beginPath();
                        ctx.moveTo(item.x, item.y - item.radius);
                        ctx.lineTo(item.x + 2, item.y - item.radius - 3);
                        ctx.stroke();
                    } else {
                        ctx.fillStyle = '#fef08a';
                        ctx.beginPath();
                        ctx.ellipse(item.x, item.y, item.radius * 0.8, item.radius, 0, 0, Math.PI * 2);
                        ctx.fill();

                        ctx.strokeStyle = '#ca8a04';
                        ctx.lineWidth = 1.5;
                        ctx.stroke();

                        ctx.fillStyle = '#84cc16';
                        ctx.beginPath();
                        ctx.arc(item.x - 2, item.y - 1, 2.5, 0, Math.PI * 2);
                        ctx.arc(item.x + 1, item.y + 3, 2, 0, Math.PI * 2);
                        ctx.fill();
                    }
                });
            }

            players.forEach(p => {
                drawPlayer(ctx, p);
            });
        }

        function gameLoop() {
            updateGame();
            renderGame();
            requestAnimationFrame(gameLoop);
        }

        window.onload = function() {
            gameLoop();
        };
    </script>
</body>
</html>
