<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>For Riddhi ❤️</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Indie+Flower&display=swap" rel="stylesheet">
    <style>
        body { background-color: #0a0a1a; color: white; font-family: 'Indie Flower', cursive; overflow-x: hidden; }
        .font-cursive { font-family: 'Dancing Script', cursive; }
        
        /* Polaroid Style for Gallery */
        .polaroid { 
            background: white; 
            padding: 6px; 
            padding-bottom: 20px; 
            box-shadow: 0 4px 6px rgba(0,0,0,0.3); 
            transition: transform 0.3s, z-index 0.3s;
            cursor: pointer;
            position: relative;
        }
        .polaroid:hover { transform: scale(1.5) rotate(0deg) !important; z-index: 100; }
        
        .animate-float { animation: float 3s ease-in-out infinite; }
        @keyframes float { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .fade-in { animation: fadeIn 1s ease-out forwards; }
        .star { position: absolute; background: white; border-radius: 50%; opacity: 0.8; }
        
        /* Floating Emojis Background */
        .floating-emoji {
            position: absolute;
            font-size: 2rem;
            opacity: 0.3;
            animation: floatUp 15s linear infinite;
            z-index: 0;
            user-select: none;
        }
        @keyframes floatUp {
            0% { transform: translateY(100vh) rotate(0deg); opacity: 0; }
            10% { opacity: 0.5; }
            90% { opacity: 0.5; }
            100% { transform: translateY(-10vh) rotate(360deg); opacity: 0; }
        }

        /* Cute Corner Decoration */
        .corner-img {
            position: fixed;
            bottom: 0;
            width: 150px;
            opacity: 0.8;
            pointer-events: none;
            z-index: 5;
            transition: opacity 0.5s;
        }
        .corner-left { left: -20px; transform: scaleX(-1); }
        .corner-right { right: -20px; }
        
        /* Hide corners on small mobile screens to save space */
        @media (max-width: 600px) {
            .corner-img { width: 100px; opacity: 0.5; }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-center relative overflow-hidden">

    <audio id="bgMusic" loop>
        <source src="https://cdn.pixabay.com/audio/2022/02/07/audio_1387f35422.mp3" type="audio/mpeg">
    </audio>

    <!-- Background Layers -->
    <div id="stars-container" class="absolute inset-0 z-0 pointer-events-none"></div>
    <div id="stickers-container" class="absolute inset-0 z-0 pointer-events-none overflow-hidden"></div>

    <!-- Cute Corner Bears -->
    <img src="https://media.tenor.com/P1t2y6i6rQ4AAAAC/bear-love.gif" class="corner-img corner-left" alt="cute bear">
    <img src="https://media.tenor.com/b9k8Wq2qQyAAAAAC/bear-sad.gif" class="corner-img corner-right" alt="cute bear">

    <button onclick="toggleMusic()" class="absolute top-5 right-5 z-20 bg-white/10 p-3 rounded-full hover:bg-white/20 backdrop-blur-sm border border-white/20 transition shadow-lg">
        <span id="musicIcon">🎵 Play Music</span>
    </button>

    <div id="app" class="z-10 w-full max-w-md px-6 py-8 flex flex-col items-center text-center min-h-[80vh] justify-center"></div>

    <script>
        // =================================================================
        //  BACKUP IMAGES
        // =================================================================
        const cuteBackups = [
            "https://media.tenor.com/t2Xp3t1a2VQAAAAC/milk-and-mocha-sad.gif",
            "https://media.tenor.com/1-f-t-W-z-4AAAAC/mocha-bear-sad.gif",
            "https://media.tenor.com/P1t2y6i6rQ4AAAAC/bear-love.gif",
            "https://media.tenor.com/7j3A_hI7xBcAAAAC/milk-and-mocha-hug.gif"
        ];

        // =================================================================
        //  SLIDES
        // =================================================================
        const slides = [
            // --- STEP 1 ---
            {
                id: 1,
                text: "It's late night... I've been walking for hours...",
                subtext: "I'm holding something heavy... not in hands, but in heart...",
                image: "24168.jpg", 
                fallback: "https://media.tenor.com/t2Xp3t1a2VQAAAAC/milk-and-mocha-sad.gif",
                button: "Let me talk..."
            },

            // --- STEP 2 ---
            {
                id: 2,
                text: "I know I hurt you...",
                subtext: "You didn't deserve that.",
                image: "24171.jpg",
                fallback: "https://media.tenor.com/1-f-t-W-z-4AAAAC/mocha-bear-sad.gif",
                button: "Read my letter..."
            },

            // --- STEP 3 ---
            {
                id: 3,
                title: "My Sorry Letter to You...",
                text: "I've been sitting here for hours, trying to find the right words...",
                image: "24169.jpg",
                fallback: "https://media.tenor.com/b9k8Wq2qQyAAAAAC/bear-sad.gif",
                button: "Continue reading..."
            },

            // --- STEP 4: BIG GALLERY ---
            {
                id: 4,
                title: "Our Memories...",
                text: "I've been staring at these all night... 🌙",
                type: "gallery",
                images: [
                    "24101.jpg", "24102.jpg", "24104.jpg", "24106.jpg", 
                    "24107.jpg", "23902.jpg", "24105.jpg", "24103.jpg"
                ],
                button: "See what I realized..."
            },

            // --- STEP 5: The Apology ---
            {
                id: 5,
                text: "You're so beautiful, but I still hurt you... I'm so sorry. 💔",
                longText: "I know words can't undo what I did, but I need you to know that you mean everything to me... I promise to do better. ✨\n\nCan you please forgive me...? 💔",
                image: "23906.jpg", 
                fallback: "https://media.tenor.com/P1t2y6i6rQ4AAAAC/bear-love.gif",
                button: "I need you..."
            },

            // --- STEP 6: The Hug ---
            {
                id: 6,
                text: "I don't need anything fancy right now...",
                subtext: "Just your arms around me, that's all I want. 💕",
                image: "24174.jpg",
                fallback: "https://media.tenor.com/7j3A_hI7xBcAAAAC/milk-and-mocha-hug.gif",
                button: "Send a hug",
                isFinal: true
            }
        ];

        // --- GAME LOGIC ---
        let currentSlide = 0;
        const app = document.getElementById('app');
        const bgMusic = document.getElementById('bgMusic');
        let isPlaying = false;

        // Create Star Background
        function createStars() {
            const container = document.getElementById('stars-container');
            for (let i = 0; i < 60; i++) {
                const star = document.createElement('div');
                star.className = 'star';
                star.style.left = Math.random() * 100 + '%';
                star.style.top = Math.random() * 100 + '%';
                star.style.width = Math.random() * 3 + 'px';
                star.style.height = star.style.width;
                container.appendChild(star);
            }
        }

        // Create Floating Stickers (Emojis)
        function createFloatingStickers() {
            const container = document.getElementById('stickers-container');
            const emojis = ['❤️', '🧸', '✨', '🌸', '🦋', '💌', '🌙'];
            
            // Create 15 floating emojis
            for (let i = 0; i < 15; i++) {
                const sticker = document.createElement('div');
                sticker.className = 'floating-emoji';
                sticker.innerText = emojis[Math.floor(Math.random() * emojis.length)];
                sticker.style.left = Math.random() * 100 + '%';
                sticker.style.animationDuration = (Math.random() * 10 + 10) + 's';
                sticker.style.animationDelay = (Math.random() * 5) + 's';
                container.appendChild(sticker);
            }
        }

        function toggleMusic() {
            if (isPlaying) { bgMusic.pause(); document.getElementById('musicIcon').innerText = "🎵 Play Music"; } 
            else { bgMusic.play(); document.getElementById('musicIcon').innerText = "Cw Pause Music"; }
            isPlaying = !isPlaying;
        }

        function renderSlide() {
            const slide = slides[currentSlide];
            app.innerHTML = ''; 
            const container = document.createElement('div');
            container.className = 'fade-in w-full flex flex-col items-center relative';

            if (slide.title) {
                const h1 = document.createElement('h1');
                h1.className = 'text-3xl font-bold text-pink-400 mb-6 font-cursive animate-pulse drop-shadow-lg';
                h1.innerText = slide.title;
                container.appendChild(h1);
            }

            if (slide.type === 'gallery') {
                const grid = document.createElement('div');
                grid.className = 'grid grid-cols-2 gap-3 mb-8 w-full px-2';
                slide.images.forEach((imgSrc, index) => {
                    const imgDiv = document.createElement('div');
                    const rotateDeg = (Math.random() * 6) - 3; 
                    imgDiv.className = 'polaroid';
                    imgDiv.style.transform = `rotate(${rotateDeg}deg)`;
                    
                    const img = document.createElement('img');
                    img.src = imgSrc;
                    img.className = 'w-full h-24 object-cover rounded shadow-sm';
                    img.onerror = () => { img.src = cuteBackups[index % cuteBackups.length]; };
                    
                    imgDiv.appendChild(img);
                    grid.appendChild(imgDiv);
                });
                container.appendChild(grid);
            } else {
                const imgContainer = document.createElement('div');
                imgContainer.className = 'relative mb-8 group animate-float';
                const img = document.createElement('img');
                img.src = slide.image;
                img.className = 'relative w-64 h-64 object-cover rounded-xl shadow-2xl border-4 border-white/20';
                img.onerror = () => { img.src = slide.fallback || cuteBackups[0]; };
                imgContainer.appendChild(img);
                container.appendChild(imgContainer);
            }

            const textDiv = document.createElement('div');
            textDiv.className = 'space-y-4 mb-10';
            const h2 = document.createElement('h2');
            h2.className = 'text-2xl font-semibold text-pink-100 leading-relaxed drop-shadow-md';
            h2.innerText = slide.text;
            textDiv.appendChild(h2);

            if (slide.subtext) {
                const p = document.createElement('p');
                p.className = 'text-lg text-gray-300 italic font-light';
                p.innerText = slide.subtext;
                textDiv.appendChild(p);
            }
            if (slide.longText) {
                const longP = document.createElement('div');
                longP.className = 'text-base text-gray-100 leading-relaxed bg-white/10 p-6 rounded-xl backdrop-blur-md border border-white/20 text-left whitespace-pre-wrap shadow-xl relative';
                longP.innerHTML = `
                    <span class="absolute -top-3 -left-2 text-2xl">💌</span>
                    ${slide.longText}
                    <span class="absolute -bottom-3 -right-2 text-2xl">🧸</span>
                `;
                textDiv.appendChild(longP);
            }
            container.appendChild(textDiv);

            const btn = document.createElement('button');
            btn.className = `px-8 py-3 rounded-full text-lg font-semibold tracking-wide transition-all transform hover:scale-105 shadow-lg ${slide.isFinal ? 'bg-gradient-to-r from-pink-500 to-red-500 text-white animate-pulse' : 'bg-white/10 hover:bg-white/20 border border-white/30'}`;
            btn.innerHTML = `${slide.button} ${slide.isFinal ? '❤️' : '→'}`;
            btn.onclick = () => { if (slide.isFinal) alert("Sending a huge hug to Riddhi! ❤️"); else { currentSlide++; renderSlide(); } };
            container.appendChild(btn);
            app.appendChild(container);
        }

        createStars();
        createFloatingStickers();
        renderSlide();
    </script>
</body>
</html>
