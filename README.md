# gamehub.github.io

<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CYBER NEXUS 2025 | Game Store</title>
    <style>
        /* --- RESET & VARIABLES --- */
        :root {
            --bg-deep: #030305;
            --bg-card: rgba(255, 255, 255, 0.03);
            --glass-border: rgba(255, 255, 255, 0.08);
            --primary: #00f0ff; /* Cyber Blue */
            --secondary: #7000ff; /* Electric Purple */
            --accent: #39ff14; /* Acid Green */
            --text-main: #ffffff;
            --text-muted: #8b9bb4;
            --font-main: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            --easing: cubic-bezier(0.23, 1, 0.32, 1);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            background-color: var(--bg-deep);
            color: var(--text-main);
            font-family: var(--font-main);
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* --- CANVAS BACKGROUND --- */
        #canvas-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.6;
            pointer-events: none;
        }

        /* --- UTILS --- */
        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 20px;
        }
        
        .btn {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid var(--glass-border);
            color: white;
            padding: 12px 24px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1px;
            cursor: pointer;
            transition: all 0.3s var(--easing);
            backdrop-filter: blur(5px);
            position: relative;
            overflow: hidden;
            display: inline-flex;
            align-items: center;
            gap: 10px;
            font-size: 0.9rem;
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 0; left: -100%;
            width: 100%; height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: 0.5s;
        }

        .btn:hover::before { left: 100%; }
        
        .btn-primary {
            background: var(--primary);
            color: #000;
            border: none;
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.4);
        }
        
        .btn-primary:hover {
            box-shadow: 0 0 30px rgba(0, 240, 255, 0.6);
            transform: translateY(-2px);
        }

        /* --- HEADER --- */
        header {
            position: fixed;
            top: 0;
            width: 100%;
            z-index: 100;
            padding: 20px 0;
            transition: 0.3s;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            background: rgba(3, 3, 5, 0.7);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
        }

        .nav-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            font-size: 1.5rem;
            font-weight: 900;
            letter-spacing: 2px;
            color: white;
            text-decoration: none;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .logo span { color: var(--primary); }

        .cart-indicator {
            position: relative;
            cursor: pointer;
        }
        
        .cart-count {
            position: absolute;
            top: -8px;
            right: -8px;
            background: var(--secondary);
            font-size: 0.7rem;
            width: 18px;
            height: 18px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }

        /* --- HERO SECTION (3D TILT) --- */
        .hero {
            padding-top: 140px;
            padding-bottom: 80px;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 60px;
            align-items: center;
            perspective: 1000px;
        }

        .hero-text h1 {
            font-size: 4.5rem;
            line-height: 1.1;
            margin-bottom: 20px;
            background: linear-gradient(135deg, #fff 0%, var(--text-muted) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .hero-text p {
            font-size: 1.2rem;
            color: var(--text-muted);
            margin-bottom: 30px;
            max-width: 500px;
        }

        .hero-card {
            width: 100%;
            height: 500px;
            background: linear-gradient(45deg, #1a1a2e, #16213e);
            border-radius: 20px;
            border: 1px solid var(--glass-border);
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.1s ease-out;
            box-shadow: 0 20px 50px rgba(0,0,0,0.5);
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            padding: 30px;
            overflow: hidden;
        }

        .hero-card-bg {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: linear-gradient(to bottom, transparent, #000), 
                        repeating-linear-gradient(45deg, rgba(112, 0, 255, 0.1) 0px, rgba(112, 0, 255, 0.1) 2px, transparent 2px, transparent 10px);
            z-index: 1;
            transition: 0.5s;
        }
        
        .hero-card:hover .hero-card-bg {
            transform: scale(1.05);
        }

        .hero-content {
            position: relative;
            z-index: 2;
            transform: translateZ(40px);
        }

        .badge {
            background: var(--accent);
            color: black;
            padding: 5px 10px;
            font-size: 0.8rem;
            font-weight: bold;
            border-radius: 4px;
            display: inline-block;
            margin-bottom: 10px;
        }

        /* --- FILTERS & GRID --- */
        .market-section {
            padding: 60px 0;
        }

        .filters {
            display: flex;
            gap: 15px;
            margin-bottom: 40px;
            flex-wrap: wrap;
        }

        .filter-btn {
            background: transparent;
            border: 1px solid var(--glass-border);
            color: var(--text-muted);
            padding: 10px 20px;
            border-radius: 30px;
            cursor: pointer;
            transition: 0.3s;
        }

        .filter-btn.active, .filter-btn:hover {
            border-color: var(--primary);
            color: var(--primary);
            background: rgba(0, 240, 255, 0.05);
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.1);
        }

        .games-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 30px;
        }

        /* --- PRODUCT CARD --- */
        .game-card {
            background: var(--bg-card);
            border: 1px solid var(--glass-border);
            border-radius: 12px;
            overflow: hidden;
            transition: all 0.4s var(--easing);
            position: relative;
            group: 'card';
            backdrop-filter: blur(10px);
        }

        .game-card:hover {
            transform: translateY(-10px);
            border-color: var(--primary);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
        }

        .card-image {
            height: 200px;
            width: 100%;
            background-size: cover;
            background-position: center;
            position: relative;
        }
        
        .card-overlay {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.6);
            opacity: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: 0.3s;
        }
        
        .game-card:hover .card-overlay { opacity: 1; }

        .card-body {
            padding: 20px;
        }

        .game-title {
            font-size: 1.1rem;
            margin-bottom: 5px;
            font-weight: 700;
        }

        .game-meta {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
        }

        .price {
            color: var(--primary);
            font-size: 1.2rem;
            font-weight: 700;
            text-shadow: 0 0 10px rgba(0, 240, 255, 0.3);
        }
        
        .rating {
            display: flex;
            align-items: center;
            gap: 5px;
            color: #ffd700;
            font-size: 0.9rem;
        }

        /* --- MODAL --- */
        dialog {
            background: #121215;
            color: white;
            border: 1px solid var(--glass-border);
            border-radius: 15px;
            padding: 0;
            max-width: 800px;
            width: 90%;
            margin: auto;
            box-shadow: 0 0 50px rgba(0,0,0,0.8);
        }
        
        dialog::backdrop {
            background: rgba(0,0,0,0.8);
            backdrop-filter: blur(5px);
        }

        .modal-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
        }
        
        .modal-img {
            background: #222;
            min-height: 400px;
        }
        
        .modal-details {
            padding: 40px;
        }

        .close-modal {
            position: absolute;
            top: 20px;
            right: 20px;
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
        }

        /* --- RESPONSIVE --- */
        @media (max-width: 900px) {
            .hero { grid-template-columns: 1fr; padding-top: 100px; text-align: center; }
            .hero-text p { margin: 0 auto 30px auto; }
            .hero-card { height: 350px; }
            .modal-content { grid-template-columns: 1fr; }
            .modal-img { height: 250px; min-height: auto; }
        }
    </style>
</head>
<body>

    <!-- CANVAS BACKGROUND -->
    <canvas id="canvas-bg"></canvas>

    <!-- HEADER -->
    <header>
        <div class="container nav-content">
            <a href="#" class="logo">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#00f0ff" stroke-width="2">
                    <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
                </svg>
                <span>NEXUS</span>PORTAL
            </a>
            <div class="nav-right" style="display: flex; gap: 20px; align-items: center;">
                <div class="cart-indicator" id="cartBtn">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2">
                        <path d="M6 2L3 6v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6l-3-4z"></path>
                        <line x1="3" y1="6" x2="21" y2="6"></line>
                        <path d="M16 10a4 4 0 0 1-8 0"></path>
                    </svg>
                    <div class="cart-count">0</div>
                </div>
            </div>
        </div>
    </header>

    <!-- MAIN CONTENT -->
    <main class="container">
        
        <!-- HERO SECTION -->
        <section class="hero">
            <div class="hero-text">
                <div class="badge">GAME OF THE YEAR EDITION</div>
                <h1>CYBER CHRONICLES: <br> THE AWAKENING</h1>
                <p>Погрузитесь в неоновый мегаполис 2077 года. Полная разрушаемость, нейросети NPC и сюжет, который меняется от каждого вашего решения.</p>
                <div style="display: flex; gap: 15px; justify-content: center; @media(min-width:900px){justify-content: flex-start;}">
                    <button class="btn btn-primary">Купить Сейчас — 2999₽</button>
                    <button class="btn">Трейлер</button>
                </div>
            </div>
            
            <div class="hero-card" id="heroCard">
                <div class="hero-card-bg"></div>
                <div class="hero-content">
                    <h2>ULTIMATE EDITION</h2>
                    <p style="color: #aaa; margin-top: 5px;">Включает все DLC + Саундтрек</p>
                </div>
            </div>
        </section>

        <!-- MARKET SECTION -->
        <section class="market-section">
            <div class="filters">
                <button class="filter-btn active" data-filter="all">Все игры</button>
                <button class="filter-btn" data-filter="RPG">RPG</button>
                <button class="filter-btn" data-filter="Action">Action</button>
                <button class="filter-btn" data-filter="Sim">Simulation</button>
            </div>

            <div class="games-grid" id="gamesGrid">
                <!-- Generated via JS -->
            </div>
        </section>
    </main>

    <!-- DETAILS MODAL -->
    <dialog id="gameModal">
        <button class="close-modal" id="closeModal">&times;</button>
        <div class="modal-content">
            <div class="modal-img" id="modalImg" style="background: linear-gradient(45deg, #333, #000);"></div>
            <div class="modal-details">
                <h2 id="modalTitle" style="font-size: 2rem; margin-bottom: 10px;">Title</h2>
                <div class="badge" id="modalCat" style="background: var(--primary);">Action</div>
                <p id="modalDesc" style="margin: 20px 0; line-height: 1.6; color: #ccc;">Description here...</p>
                <div style="display: flex; justify-content: space-between; align-items: center; margin-top: 30px;">
                    <div id="modalPrice" style="font-size: 1.5rem; font-weight: bold; color: var(--primary);">2000₽</div>
                    <button class="btn btn-primary" onclick="addToCart()">В корзину</button>
                </div>
            </div>
        </div>
    </dialog>

    <script>
        // --- DATA ---
        const games = [
            {
                id: 1,
                title: "Star Wandered",
                category: "RPG",
                price: 1999,
                rating: 4.8,
                color: "linear-gradient(135deg, #2b1055, #7597de)",
                desc: "Исследуйте бесконечные галактики в этом космическом симуляторе."
            },
            {
                id: 2,
                title: "Neon Racer X",
                category: "Action",
                price: 899,
                rating: 4.5,
                color: "linear-gradient(135deg, #3a1c71, #d76d77, #ffaf7b)",
                desc: "Высокоскоростные гонки в ретро-футуристичном стиле."
            },
            {
                id: 3,
                title: "Shadows of Kyoto",
                category: "Action",
                price: 2499,
                rating: 4.9,
                color: "linear-gradient(135deg, #000000, #434343)",
                desc: "Стелс-экшен в древней Японии с кибернетическими имплантами."
            },
            {
                id: 4,
                title: "Farm Droid 3000",
                category: "Sim",
                price: 499,
                rating: 4.2,
                color: "linear-gradient(135deg, #11998e, #38ef7d)",
                desc: "Управляйте фермой роботов и автоматизируйте урожай."
            },
            {
                id: 5,
                title: "Void Slayer",
                category: "RPG",
                price: 1299,
                rating: 4.6,
                color: "linear-gradient(135deg, #8E2DE2, #4A00E0)",
                desc: "Охотьтесь на демонов из пустоты, используя магию и технологии."
            },
            {
                id: 6,
                title: "Mech Warrior Arena",
                category: "Action",
                price: 1599,
                rating: 4.4,
                color: "linear-gradient(135deg, #cb2d3e, #ef473a)",
                desc: "Битвы гигантских мехов на разрушаемых аренах."
            }
        ];

        // --- DOM ELEMENTS ---
        const grid = document.getElementById('gamesGrid');
        const filterBtns = document.querySelectorAll('.filter-btn');
        const cartCount = document.querySelector('.cart-count');
        const heroCard = document.getElementById('heroCard');
        const modal = document.getElementById('gameModal');
        const closeModal = document.getElementById('closeModal');

        let cart = 0;

        // --- RENDER FUNCTION ---
        function renderGames(filter = 'all') {
            grid.innerHTML = '';
            games.forEach(game => {
                if (filter === 'all' || game.category === filter) {
                    const card = document.createElement('div');
                    card.className = 'game-card';
                    card.innerHTML = `
                        <div class="card-image" style="background: ${game.color}">
                            <div class="card-overlay">
                                <button class="btn btn-primary" onclick="openModal(${game.id})">Подробнее</button>
                            </div>
                        </div>
                        <div class="card-body">
                            <div class="badge" style="background: rgba(255,255,255,0.1); color: white;">${game.category}</div>
                            <h3 class="game-title">${game.title}</h3>
                            <div class="game-meta">
                                <span class="rating">★ ${game.rating}</span>
                                <span class="price">${game.price}₽</span>
                            </div>
                        </div>
                    `;
                    grid.appendChild(card);
                }
            });
        }

        // --- FILTERING ---
        filterBtns.forEach(btn => {
            btn.addEventListener('click', () => {
                filterBtns.forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                renderGames(btn.getAttribute('data-filter'));
            });
        });

        // --- MODAL LOGIC ---
        window.openModal = (id) => {
            const game = games.find(g => g.id === id);
            document.getElementById('modalTitle').innerText = game.title;
            document.getElementById('modalDesc').innerText = game.desc;
            document.getElementById('modalPrice').innerText = game.price + '₽';
            document.getElementById('modalCat').innerText = game.category;
            document.getElementById('modalImg').style.background = game.color;
            modal.showModal();
        };

        closeModal.addEventListener('click', () => modal.close());
        
        modal.addEventListener('click', (e) => {
            if (e.target === modal) modal.close();
        });

        // --- CART LOGIC ---
        window.addToCart = () => {
            cart++;
            cartCount.innerText = cart;
            modal.close();
            // Simple visual feedback
            const btn = document.querySelector('.cart-indicator');
            btn.style.transform = 'scale(1.2)';
            setTimeout(() => btn.style.transform = 'scale(1)', 200);
        }

        // --- 3D TILT EFFECT (HERO) ---
        heroCard.addEventListener('mousemove', (e) => {
            const rect = heroCard.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;
            
            const centerX = rect.width / 2;
            const centerY = rect.height / 2;
            
            const rotateX = ((y - centerY) / centerY) * -10; // Max 10deg
            const rotateY = ((x - centerX) / centerX) * 10;

            heroCard.style.transform = `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
        });

        heroCard.addEventListener('mouseleave', () => {
            heroCard.style.transform = `perspective(1000px) rotateX(0) rotateY(0)`;
        });

        // --- CANVAS ANIMATION (Background) ---
        const canvas = document.getElementById('canvas-bg');
        const ctx = canvas.getContext('2d');
        
        let width, height;
        let particles = [];
        const mouse = { x: null, y: null };

        function initCanvas() {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
            particles = [];
            const particleCount = Math.floor(width * 0.05); // Responsive count

            for(let i = 0; i < particleCount; i++) {
                particles.push({
                    x: Math.random() * width,
                    y: Math.random() * height,
                    vx: (Math.random() - 0.5) * 0.5,
                    vy: (Math.random() - 0.5) * 0.5,
                    size: Math.random() * 2 + 1,
                    color: Math.random() > 0.5 ? '#00f0ff' : '#7000ff'
                });
            }
        }

        function animateCanvas() {
            ctx.clearRect(0, 0, width, height);
            
            // Draw connections
            ctx.lineWidth = 0.5;
            
            particles.forEach((p, i) => {
                // Move
                p.x += p.vx;
                p.y += p.vy;

                // Bounce
                if(p.x < 0 || p.x > width) p.vx *= -1;
                if(p.y < 0 || p.y > height) p.vy *= -1;

                // Mouse interaction
                if(mouse.x != null) {
                    const dx = mouse.x - p.x;
                    const dy = mouse.y - p.y;
                    const dist = Math.sqrt(dx*dx + dy*dy);
                    if(dist < 150) {
                        ctx.beginPath();
                        ctx.strokeStyle = `rgba(255, 255, 255, ${1 - dist/150})`;
                        ctx.moveTo(p.x, p.y);
                        ctx.lineTo(mouse.x, mouse.y);
                        ctx.stroke();
                        
                        // Mild repulsion
                        p.x -= dx * 0.01;
                        p.y -= dy * 0.01;
                    }
                }

                // Draw Particle
                ctx.fillStyle = p.color;
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
                ctx.fill();
            });

            requestAnimationFrame(animateCanvas);
        }

        window.addEventListener('resize', initCanvas);
        window.addEventListener('mousemove', e => {
            mouse.x = e.clientX;
            mouse.y = e.clientY;
        });

        // Initialize
        initCanvas();
        animateCanvas();
        renderGames();

    </script>
</body>
</html>
