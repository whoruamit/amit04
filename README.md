<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Amit | Chemical Process Engineer</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;500&family=Orbitron:wght@400;700&family=Sacramento&display=swap');

        :root {
            --neon-pink: #ff2d75;
            --neon-green: #0f0;
            --neon-blue: #00d2ff;
            --dark-bg: #050505;
        }

        body {
            background-color: var(--dark-bg);
            color: #fff;
            font-family: 'Fira Code', monospace;
            overflow-x: hidden;
            scroll-behavior: smooth;
        }

        h1, h2, h3 {
            font-family: 'Orbitron', sans-serif;
        }

        /* Gate-Keeper Styling */
        #gate-keeper {
            position: fixed;
            inset: 0;
            background: #000;
            z-index: 2000;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: transform 1s cubic-bezier(0.85, 0, 0.15, 1), opacity 1s;
        }

        .cat-btn {
            font-size: 6rem;
            cursor: pointer;
            color: var(--neon-green);
            filter: drop-shadow(0 0 15px var(--neon-green));
            animation: cat-glitch 2s infinite;
        }

        @keyframes cat-glitch {
            0% { transform: scale(1); opacity: 1; }
            5% { transform: translate(-5px, 2px); opacity: 0.8; color: var(--neon-pink); }
            10% { transform: translate(5px, -2px); opacity: 1; color: var(--neon-green); }
            15% { transform: translate(0); }
            100% { transform: translate(0); }
        }

        /* Matrix Background */
        #matrix-canvas {
            position: fixed;
            top: 0;
            left: 0;
            z-index: -1;
            opacity: 0.3;
        }

        /* Benzene Ring Animation */
        .benzene-ring {
            width: 80px;
            height: 80px;
            border: 2px solid var(--neon-blue);
            position: relative;
            clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
            animation: rotate-ring 8s linear infinite;
            opacity: 0.4;
        }

        @keyframes rotate-ring {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        /* Process Stats Floating */
        .process-stat {
            position: fixed;
            font-size: 10px;
            color: var(--neon-green);
            opacity: 0.5;
            pointer-events: none;
            z-index: 50;
        }

        /* Scroll Reveal Effect */
        .reveal {
            opacity: 0;
            transform: translateY(30px);
            transition: all 1s ease-out;
        }
        .reveal.active {
            opacity: 1;
            transform: translateY(0);
        }

        .heart-beat {
            animation: heartbeat 1.2s infinite cubic-bezier(0.215, 0.61, 0.355, 1);
            filter: drop-shadow(0 0 20px var(--neon-pink));
        }

        /* Terminal Box for Content */
        .terminal-box {
            background: rgba(0, 0, 0, 0.9);
            border: 1px solid var(--neon-green);
            box-shadow: 0 0 15px rgba(0, 255, 0, 0.1);
            padding: 25px;
            border-radius: 12px;
            transition: 0.4s;
        }
        .terminal-box:hover {
            border-color: var(--neon-pink);
            transform: translateY(-5px);
        }

        /* Profile Card Glassmorphism */
        .profile-card {
            background: linear-gradient(135deg, rgba(255,45,117,0.1) 0%, rgba(0,210,255,0.05) 100%);
            border: 1px solid rgba(255,255,255,0.1);
            backdrop-filter: blur(15px);
            border-radius: 20px;
        }

        /* Coming Soon Blinking Tag */
        .coming-soon-tag {
            font-size: 0.7rem;
            color: var(--neon-pink);
            border: 1px solid var(--neon-pink);
            padding: 2px 8px;
            border-radius: 4px;
            animation: blink 1.5s infinite;
        }

        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.3; }
        }

        #music-toggle {
            position: fixed;
            bottom: 30px;
            right: 30px;
            z-index: 1000;
            background: rgba(0, 0, 0, 0.7);
            border: 2px solid var(--neon-pink);
            width: 55px; height: 55px;
            border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            cursor: pointer;
        }

        .topics-row { display: flex; overflow-x: auto; gap: 20px; padding: 20px 10px; scrollbar-width: none; }
        .topics-row::-webkit-scrollbar { display: none; }

        .glitch-text { position: relative; }
        .glitch-text::before {
            content: attr(data-text);
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            color: #0ff; z-index: -1;
            animation: glitch-anim 2s infinite linear alternate-reverse;
        }
    </style>
</head>
<body>

    <!-- GATE KEEPER (Billi wala page) -->
    <div id="gate-keeper">
        <div id="gate-logs" class="absolute top-10 left-10 text-[9px] text-green-500 font-mono opacity-50"></div>
        <div class="cat-btn" id="cat-trigger">
            <i class="fas fa-cat"></i>
        </div>
        <div class="mt-10 text-center">
            <h2 class="neon-text-green font-bold tracking-[0.4em] text-sm uppercase">Process Control Terminal</h2>
            <p class="text-[9px] opacity-40 mt-2 uppercase" id="gate-status">Click Billi to Initialize Plant</p>
        </div>
    </div>

    <!-- Floating Parameters layer -->
    <div id="stats-layer"></div>

    <!-- Background Music Audio -->
    <audio id="bg-music" loop>
        <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
    </audio>

    <div id="music-toggle">
        <i class="fas fa-volume-mute text-pink-500" id="music-icon"></i>
    </div>

    <canvas id="matrix-canvas"></canvas>

    <!-- [HOME] - Intro Page -->
    <section id="home" class="min-h-screen flex flex-col justify-center items-center text-center relative px-6">
        <div class="flex gap-4 md:gap-8 mb-8 items-center justify-center scale-75 md:scale-100">
            <div class="benzene-ring"></div>
            <div class="relative">
                <i class="fas fa-flask-vial text-7xl neon-text-green animate-pulse"></i>
                <div class="absolute -top-4 -right-4 text-[10px] text-blue-400 font-bold">ΔH < 0</div>
            </div>
            <div class="benzene-ring" style="animation-direction: reverse;"></div>
        </div>
        <h1 class="text-6xl md:text-9xl font-bold mb-6 glitch-text" data-text="AMIT">AMIT</h1>
        <p class="text-lg md:text-3xl text-pink-200 mb-10 italic max-w-2xl" style="font-family: 'Sacramento', cursive;">
            "Simulating human emotions through the laws of Thermodynamics."
        </p>
        <div class="heart-beat cursor-pointer">
            <i class="fas fa-heart text-7xl text-red-500"></i>
        </div>
    </section>

    <!-- [ABOUT] - Portfolio Bio -->
    <section id="about" class="reveal">
        <div class="profile-card p-8 md:p-12 flex flex-col md:flex-row items-center gap-10 max-w-5xl mx-auto">
            <div class="w-48 h-48 md:w-56 md:h-56 rounded-full border-4 border-blue-500 flex-shrink-0 flex items-center justify-center bg-black shadow-[0_0_30px_rgba(0,210,255,0.4)] relative">
                <i class="fas fa-microchip text-6xl text-blue-400"></i>
                <div class="absolute inset-0 border-2 border-dashed border-blue-500 rounded-full animate-spin-slow opacity-30"></div>
            </div>
            <div>
                <h2 class="text-3xl font-bold mb-4 neon-text-pink uppercase">Designation: Amit</h2>
                <p class="text-gray-300 text-lg italic leading-relaxed mb-6">
                    "I am Amit. A Chemical Engineer by day, a heart hacker by night. I specialize in Mass Transfer and Stoichiometric affection."
                </p>
                <div class="flex flex-wrap gap-3">
                    <span class="px-3 py-1 bg-green-500/10 border border-green-500 text-green-500 text-[10px] rounded">ASPEN_HYSYS_PRO</span>
                    <span class="px-3 py-1 bg-blue-500/10 border border-blue-500 text-blue-400 text-[10px] rounded">FLUID_DYNAMICS</span>
                    <span class="px-3 py-1 bg-pink-500/10 border border-pink-500 text-pink-500 text-[10px] rounded">THERMO_MASTER</span>
                </div>
            </div>
        </div>
    </section>

    <!-- [LAB TOPICS] - Chemical Engineering Cards -->
    <section id="lab" class="reveal">
        <h2 class="text-2xl md:text-3xl font-bold mb-12 text-center neon-text-green uppercase tracking-[0.3em]">Unit Operations Lab</h2>
        <div class="topics-row">
            <div class="terminal-box border-blue-500 min-w-[280px]">
                <i class="fas fa-wind text-3xl mb-4 text-blue-400"></i>
                <h3 class="font-bold mb-2 uppercase text-sm">Fluid Mechanics</h3>
                <p class="text-[11px] opacity-70">Laminar flow of affection with a Reynolds Number of pure love.</p>
            </div>
            <div class="terminal-box border-pink-500 min-w-[280px]">
                <i class="fas fa-droplet text-3xl mb-4 text-pink-500"></i>
                <h3 class="font-bold mb-2 uppercase text-sm">Mass Transfer</h3>
                <p class="text-[11px] opacity-70">Diffusion of soul-data through a semi-permeable heart membrane.</p>
            </div>
            <div class="terminal-box border-green-500 min-w-[280px]">
                <i class="fas fa-bolt text-3xl mb-4 text-green-500"></i>
                <h3 class="font-bold mb-2 uppercase text-sm">Reaction Kinetics</h3>
                <p class="text-[11px] opacity-70">Calculating the rate constant (k) for an everlasting reaction.</p>
            </div>
            <!-- Coming Soon Topic Card -->
            <div class="terminal-box border-dashed border-gray-600 min-w-[280px] flex flex-col justify-center items-center text-center">
                <i class="fas fa-microscope text-3xl mb-4 text-gray-500"></i>
                <h3 class="font-bold mb-2 uppercase text-sm text-gray-500">AI Refining</h3>
                <span class="coming-soon-tag">Coming Soon</span>
                <p class="text-[10px] opacity-40 mt-2">Integrating Neural Networks with Chemical Reactors.</p>
            </div>
        </div>
    </section>

    <!-- [PROJECT PIPELINE] - Dedicated Coming Soon Section -->
    <section id="pipeline" class="reveal bg-black/40 py-20 my-10 border-y border-pink-500/20">
        <div class="max-w-4xl mx-auto text-center px-6">
            <h2 class="text-3xl font-bold mb-8 neon-text-pink uppercase tracking-widest">Future Simulations</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="p-6 border border-blue-900 rounded-lg text-left bg-black/60 relative overflow-hidden">
                    <div class="absolute top-2 right-2 coming-soon-tag">Q3 2026</div>
                    <h3 class="text-blue-400 font-bold mb-2 uppercase">Bio-Chemistry Mesh</h3>
                    <p class="text-xs text-gray-400">Industrial scale production of biological catalysts for sustainable heart modeling.</p>
                </div>
                <div class="p-6 border border-green-900 rounded-lg text-left bg-black/60 relative overflow-hidden">
                    <div class="absolute top-2 right-2 coming-soon-tag">Q4 2026</div>
                    <h3 class="text-green-500 font-bold mb-2 uppercase">Nanotech Distillation</h3>
                    <p class="text-xs text-gray-400">Designing filters at the 1nm scale to separate jealousy from pure affection.</p>
                </div>
            </div>
            <div class="mt-12 text-pink-500 animate-pulse font-mono text-sm">
                > WAITING FOR NEW DATA PACKETS... [COMING SOON]
            </div>
        </div>
    </section>

    <!-- [INSTAGRAM] - Footer/Contact -->
    <section id="contact" class="reveal flex flex-col items-center pb-32">
        <h2 class="text-4xl font-bold mb-12 neon-text-pink uppercase tracking-widest text-center">Plant Manager Dashboard</h2>
        <div class="heart-beat cursor-pointer mb-10">
            <a href="https://instagram.com/abyss.creep" target="_blank">
                <div class="w-44 h-44 rounded-full flex items-center justify-center bg-black border-4 border-pink-500 shadow-[0_0_50px_rgba(255,45,117,0.5)] overflow-hidden">
                    <i class="fab fa-instagram text-7xl text-white"></i>
                </div>
            </a>
        </div>
        <a href="https://instagram.com/abyss.creep" target="_blank" class="bg-white text-black px-10 py-4 rounded-full font-black text-xl uppercase tracking-widest hover:bg-pink-500 hover:text-white transition-all">
            @abyss.creep
        </a>
    </section>

    <script>
        const apiKey = ""; 

        // Helper function: PCM to WAV for AI Voice
        function pcmToWav(pcmData, sampleRate) {
            const buffer = new ArrayBuffer(44 + pcmData.length);
            const view = new DataView(buffer);
            const writeString = (offset, string) => {
                for (let i = 0; i < string.length; i++) view.setUint8(offset + i, string.charCodeAt(i));
            };
            writeString(0, 'RIFF');
            view.setUint32(4, 36 + pcmData.length, true);
            writeString(8, 'WAVE'); writeString(12, 'fmt ');
            view.setUint32(16, 16, true); view.setUint16(20, 1, true);
            view.setUint16(22, 1, true); view.setUint32(24, sampleRate, true);
            view.setUint32(28, sampleRate * 2, true); view.setUint16(32, 2, true);
            view.setUint16(34, 16, true); writeString(36, 'data');
            view.setUint32(40, pcmData.length, true);
            for (let i = 0; i < pcmData.length; i++) view.setUint8(44 + i, pcmData[i]);
            return new Blob([buffer], { type: 'audio/wav' });
        }

        // AI Voice Greeting function
        async function playHackerWelcome() {
            const text = "Say in a deep, robotic hacker voice: Welcome to Amit's chemical refining plant. Future project modules are currently loading. Stoichiometric balance complete. Access granted.";
            const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`;
            const payload = {
                contents: [{ parts: [{ text }] }],
                generationConfig: {
                    responseModalities: ["AUDIO"],
                    speechConfig: { voiceConfig: { prebuiltVoiceConfig: { voiceName: "Iapetus" } } }
                }
            };

            const fetchVoice = async () => {
                try {
                    const resp = await fetch(url, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(payload) });
                    const res = await resp.json();
                    const audio = res.candidates[0].content.parts[0].inlineData;
                    const sr = parseInt(audio.mimeType.split('rate=')[1]) || 24000;
                    const bin = atob(audio.data);
                    const pcm = new Uint8Array(bin.length);
                    for (let i = 0; i < bin.length; i++) pcm[i] = bin.charCodeAt(i);
                    const wav = pcmToWav(pcm, sr);
                    new Audio(URL.createObjectURL(wav)).play();
                } catch (e) {}
            };
            fetchVoice();
        }

        // Floating Stats generation logic
        const statsLayer = document.getElementById('stats-layer');
        const statNames = ["T: 373 K", "P: 10.5 bar", "Q: 45 m³/h", "η: 98%", "pH: 7.2", "v: 2.5 m/s", "LOADING...", "COMING SOON"];
        function spawnStat() {
            const div = document.createElement('div');
            div.className = 'process-stat font-mono';
            div.textContent = statNames[Math.floor(Math.random() * statNames.length)];
            div.style.left = Math.random() * 90 + 'vw';
            div.style.top = Math.random() * 90 + 'vh';
            statsLayer.appendChild(div);
            setTimeout(() => div.remove(), 4000);
        }
        setInterval(spawnStat, 1500);

        // Matrix Background Logic
        const canvas = document.getElementById('matrix-canvas');
        const ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth; canvas.height = window.innerHeight;
        const chars = "01❤⚛H₂O⚛COMING_SOON";
        const fontSize = 16;
        let drops = Array(Math.floor(canvas.width / fontSize)).fill(1);

        function drawMatrix() {
            ctx.fillStyle = "rgba(0, 0, 0, 0.05)";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.font = fontSize + "px monospace";
            drops.forEach((y, i) => {
                const text = chars.charAt(Math.floor(Math.random() * chars.length));
                ctx.fillStyle = Math.random() > 0.9 ? "#00d2ff" : "#ff2d75";
                ctx.fillText(text, i * fontSize, y * fontSize);
                if (y * fontSize > canvas.height && Math.random() > 0.975) drops[i] = 0;
                drops[i]++;
            });
        }
        setInterval(drawMatrix, 40);

        // Gate Logic (Click Billi)
        const gate = document.getElementById('gate-keeper');
        const catTrigger = document.getElementById('cat-trigger');
        const music = document.getElementById('bg-music');
        const musicIcon = document.getElementById('music-icon');
        const statusText = document.getElementById('gate-status');
        const gateLogs = document.getElementById('gate-logs');
        let isPlaying = false;

        const fakeLogs = [
            "> INITIATING STOICHIOMETRIC SCAN...",
            "> LOADING FUTURE PROJECT MODULES...",
            "> DISTILLATION PURITY: 99.98%",
            "> MASS BALANCE: STABLE",
            "> BYPASSING SECURITY FOR ENG. AMIT",
            "> COMING SOON: AI_REFINING_V2",
            "> ACCESS GRANTED."
        ];

        let logIdx = 0;
        function typeLogs() {
            if (logIdx < fakeLogs.length) {
                const p = document.createElement('p');
                p.textContent = fakeLogs[logIdx];
                gateLogs.appendChild(p);
                logIdx++;
                setTimeout(typeLogs, 600);
            }
        }
        typeLogs();

        catTrigger.addEventListener('click', () => {
            statusText.textContent = "BALANCING EQUATIONS... WELCOME AMIT...";
            playHackerWelcome();
            setTimeout(() => {
                gate.style.transform = "translateY(-100%)";
                gate.style.opacity = "0";
                music.play().catch(() => {});
                isPlaying = true;
                musicIcon.className = 'fas fa-volume-up text-pink-500';
            }, 1800);
        });

        document.getElementById('music-toggle').addEventListener('click', () => {
            if (isPlaying) { music.pause(); musicIcon.className = 'fas fa-volume-mute'; }
            else { music.play(); musicIcon.className = 'fas fa-volume-up'; }
            isPlaying = !isPlaying;
        });

        // Intersection Observer for scroll reveal
        window.addEventListener('scroll', () => {
            document.querySelectorAll('.reveal').forEach(rev => {
                if (rev.getBoundingClientRect().top < window.innerHeight - 100) rev.classList.add('active');
            });
        });

        window.onresize = () => {
            canvas.width = window.innerWidth; canvas.height = window.innerHeight;
            drops = Array(Math.floor(canvas.width / fontSize)).fill(1);
        };
    </script>
</body>
</html>
