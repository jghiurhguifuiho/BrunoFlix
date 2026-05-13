<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portfólio Dev | Curso Técnico</title>
    <style>
        :root {
            --bg: #0a0a0f;
            --surface: #12121a;
            --surface2: #1a1a28;
            --primary: #00e5ff;
            --primary2: #7c4dff;
            --accent: #ff4081;
            --text: #e0e0e0;
            --text2: #a0a0b8;
            --glass: rgba(18, 18, 30, 0.7);
            --glass-border: rgba(255, 255, 255, 0.08);
            --shadow: 0 8px 40px rgba(0, 0, 0, 0.5);
            --radius: 16px;
            --transition: 0.35s cubic-bezier(0.4, 0, 0.2, 1);
            --font: 'Segoe UI', 'Inter', system-ui, -apple-system, sans-serif;
            --mono: 'Cascadia Code', 'Fira Code', 'JetBrains Mono', 'Consolas', monospace;
        }

        .light-mode {
            --bg: #f0f2f7;
            --surface: #ffffff;
            --surface2: #e8ecf3;
            --text: #1a1a2e;
            --text2: #555;
            --glass: rgba(255, 255, 255, 0.75);
            --glass-border: rgba(0, 0, 0, 0.1);
            --shadow: 0 8px 40px rgba(0, 0, 0, 0.1);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html {
            scroll-behavior: smooth;
            scrollbar-width: thin;
            scrollbar-color: var(--primary2) var(--bg);
        }
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: var(--bg);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--primary2);
            border-radius: 10px;
        }

        body {
            font-family: var(--font);
            background: var(--bg);
            color: var(--text);
            overflow-x: hidden;
            transition: background var(--transition), color var(--transition);
            line-height: 1.6;
        }

        /* ===== CANVAS DE PARTÍCULAS ===== */
        #particleCanvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            pointer-events: none;
            opacity: 0.6;
        }
        .light-mode #particleCanvas {
            opacity: 0.25;
        }

        /* ===== CONTEÚDO PRINCIPAL ===== */
        .main-content {
            position: relative;
            z-index: 1;
        }

        /* ===== NAVBAR ===== */
        .navbar {
            position: fixed;
            top: 16px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 10px 24px;
            border-radius: 50px;
            background: var(--glass);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid var(--glass-border);
            box-shadow: var(--shadow);
            transition: all var(--transition);
            flex-wrap: wrap;
            justify-content: center;
        }
        .navbar a {
            text-decoration: none;
            color: var(--text2);
            font-weight: 600;
            font-size: 0.9rem;
            padding: 8px 16px;
            border-radius: 30px;
            transition: all var(--transition);
            white-space: nowrap;
            position: relative;
        }
        .navbar a:hover,
        .navbar a.active {
            color: #fff;
            background: var(--primary2);
            box-shadow: 0 0 20px rgba(124, 77, 255, 0.5);
        }
        .light-mode .navbar a:hover,
        .light-mode .navbar a.active {
            color: #fff;
        }
        .theme-toggle {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            border: 1px solid var(--glass-border);
            background: var(--surface2);
            color: var(--text);
            cursor: pointer;
            font-size: 1.2rem;
            transition: all var(--transition);
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
        }
        .theme-toggle:hover {
            background: var(--primary2);
            color: #fff;
            box-shadow: 0 0 20px rgba(124, 77, 255, 0.5);
        }

        /* ===== HERO ===== */
        .hero {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            text-align: center;
            padding: 120px 20px 60px;
            position: relative;
        }
        .hero-badge {
            display: inline-block;
            padding: 6px 18px;
            border-radius: 30px;
            background: rgba(124, 77, 255, 0.2);
            border: 1px solid rgba(124, 77, 255, 0.4);
            color: var(--primary2);
            font-weight: 600;
            font-size: 0.85rem;
            letter-spacing: 1px;
            text-transform: uppercase;
            margin-bottom: 20px;
            animation: fadeInUp 0.8s ease;
        }
        .hero h1 {
            font-size: clamp(2.5rem, 6vw, 4.5rem);
            font-weight: 800;
            background: linear-gradient(135deg, var(--primary), var(--primary2), var(--accent));
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: fadeInUp 0.8s ease 0.15s both;
            line-height: 1.2;
        }
        .hero .typing-wrapper {
            font-size: clamp(1rem, 2.5vw, 1.4rem);
            color: var(--text2);
            margin-top: 12px;
            min-height: 2em;
            animation: fadeInUp 0.8s ease 0.3s both;
        }
        .hero .typing-wrapper .cursor-blink {
            display: inline-block;
            width: 3px;
            height: 1.2em;
            background: var(--primary);
            margin-left: 4px;
            vertical-align: text-bottom;
            animation: blink 0.8s infinite;
        }
        @keyframes blink {
            0%,
            100% {
                opacity: 1;
            }
            50% {
                opacity: 0;
            }
        }
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        .hero-cta {
            margin-top: 30px;
            display: flex;
            gap: 16px;
            flex-wrap: wrap;
            justify-content: center;
            animation: fadeInUp 0.8s ease 0.45s both;
        }
        .btn {
            padding: 14px 32px;
            border-radius: 50px;
            font-weight: 700;
            font-size: 1rem;
            cursor: pointer;
            border: none;
            text-decoration: none;
            transition: all var(--transition);
            letter-spacing: 0.5px;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }
        .btn-primary {
            background: var(--primary2);
            color: #fff;
            box-shadow: 0 4px 25px rgba(124, 77, 255, 0.45);
        }
        .btn-primary:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 35px rgba(124, 77, 255, 0.65);
        }
        .btn-outline {
            background: transparent;
            border: 2px solid var(--glass-border);
            color: var(--text);
        }
        .btn-outline:hover {
            border-color: var(--primary);
            color: var(--primary);
            transform: translateY(-3px);
            box-shadow: 0 8px 30px rgba(0, 229, 255, 0.2);
        }

        /* ===== SEÇÕES ===== */
        section {
            padding: 80px 20px;
            max-width: 1200px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }
        .section-title {
            text-align: center;
            margin-bottom: 50px;
        }
        .section-title h2 {
            font-size: 2.2rem;
            font-weight: 700;
            background: linear-gradient(135deg, var(--primary), var(--primary2));
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            display: inline-block;
        }
        .section-title .line {
            width: 60px;
            height: 4px;
            background: var(--primary2);
            margin: 12px auto 0;
            border-radius: 10px;
        }

        /* ===== CARDS DE HABILIDADES ===== */
        .skills-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 24px;
        }
        .skill-card {
            background: var(--glass);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border: 1px solid var(--glass-border);
            border-radius: var(--radius);
            padding: 30px 24px;
            text-align: center;
            transition: all var(--transition);
            cursor: pointer;
            position: relative;
            overflow: hidden;
            box-shadow: var(--shadow);
        }
        .skill-card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(124, 77, 255, 0.15) 0%, transparent 70%);
            opacity: 0;
            transition: opacity var(--transition);
        }
        .skill-card:hover::before {
            opacity: 1;
        }
        .skill-card:hover {
            transform: translateY(-10px) scale(1.03);
            border-color: var(--primary2);
            box-shadow: 0 20px 60px rgba(124, 77, 255, 0.25);
        }
        .skill-card .icon {
            font-size: 3rem;
            margin-bottom: 16px;
            transition: transform var(--transition);
        }
        .skill-card:hover .icon {
            transform: scale(1.2) rotate(-5deg);
        }
        .skill-card h3 {
            font-weight: 700;
            margin-bottom: 8px;
            color: var(--text);
        }
        .skill-card .progress-bar {
            width: 100%;
            height: 8px;
            background: var(--surface2);
            border-radius: 10px;
            margin-top: 12px;
            overflow: hidden;
        }
        .skill-card .progress-fill {
            height: 100%;
            border-radius: 10px;
            background: linear-gradient(90deg, var(--primary), var(--primary2));
            transition: width 1.5s ease;
            width: 0;
        }

        /* ===== TIMELINE ===== */
        .timeline {
            position: relative;
            padding-left: 40px;
        }
        .timeline::before {
            content: '';
            position: absolute;
            left: 18px;
            top: 0;
            bottom: 0;
            width: 3px;
            background: linear-gradient(180deg, var(--primary2), var(--accent));
            border-radius: 10px;
        }
        .timeline-item {
            position: relative;
            margin-bottom: 40px;
            padding-left: 30px;
            opacity: 0;
            transform: translateX(-40px);
            transition: all 0.7s ease;
        }
        .timeline-item.visible {
            opacity: 1;
            transform: translateX(0);
        }
        .timeline-item::before {
            content: '';
            position: absolute;
            left: -28px;
            top: 6px;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: var(--primary2);
            border: 3px solid var(--bg);
            box-shadow: 0 0 15px rgba(124, 77, 255, 0.7);
            z-index: 2;
        }
        .timeline-item h4 {
            font-weight: 700;
            color: var(--primary);
        }
        .timeline-item .date {
            font-size: 0.8rem;
            color: var(--text2);
            margin-bottom: 6px;
        }

        /* ===== TERMINAL ===== */
        .terminal-section {
            background: var(--glass);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border: 1px solid var(--glass-border);
            border-radius: var(--radius);
            padding: 0;
            overflow: hidden;
            box-shadow: var(--shadow);
            max-width: 800px;
            margin: 0 auto;
        }
        .terminal-header {
            background: var(--surface2);
            padding: 12px 16px;
            display: flex;
            gap: 8px;
            align-items: center;
            border-bottom: 1px solid var(--glass-border);
        }
        .terminal-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
        }
        .terminal-dot.red {
            background: #ff5f57;
        }
        .terminal-dot.yellow {
            background: #ffbd2e;
        }
        .terminal-dot.green {
            background: #28ca41;
        }
        .terminal-title {
            margin-left: 10px;
            font-size: 0.8rem;
            color: var(--text2);
            font-family: var(--mono);
        }
        .terminal-body {
            padding: 20px;
            font-family: var(--mono);
            font-size: 0.9rem;
            min-height: 250px;
            max-height: 400px;
            overflow-y: auto;
            color: #00e5ff;
            background: rgba(0, 0, 0, 0.3);
        }
        .light-mode .terminal-body {
            background: rgba(0, 0, 0, 0.05);
            color: #1a5f6e;
        }
        .terminal-body .line {
            margin-bottom: 4px;
            word-break: break-all;
        }
        .terminal-body .prompt {
            color: #7c4dff;
            font-weight: 700;
        }
        .terminal-body .output {
            color: #a0a0b8;
        }
        .light-mode .terminal-body .output {
            color: #555;
        }
        .terminal-input-row {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-top: 8px;
        }
        .terminal-input-row .prompt {
            flex-shrink: 0;
        }
        .terminal-input-row input {
            flex: 1;
            background: transparent;
            border: none;
            color: #00e5ff;
            font-family: var(--mono);
            font-size: 0.9rem;
            outline: none;
            caret-color: #00e5ff;
        }
        .light-mode .terminal-input-row input {
            color: #1a5f6e;
            caret-color: #1a5f6e;
        }

        /* ===== CARDS 3D ===== */
        .projects-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 28px;
        }
        .project-card-3d {
            perspective: 800px;
            height: 280px;
        }
        .project-card-inner {
            width: 100%;
            height: 100%;
            position: relative;
            transition: transform 0.1s ease;
            transform-style: preserve-3d;
            border-radius: var(--radius);
            background: var(--glass);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border: 1px solid var(--glass-border);
            padding: 28px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            box-shadow: var(--shadow);
            cursor: pointer;
            user-select: none;
        }
        .project-card-inner .proj-icon {
            font-size: 3.5rem;
            margin-bottom: 16px;
            transform: translateZ(30px);
        }
        .project-card-inner h3 {
            transform: translateZ(20px);
            font-weight: 700;
        }
        .project-card-inner p {
            transform: translateZ(15px);
            color: var(--text2);
            font-size: 0.9rem;
        }
        .project-card-inner .glow {
            position: absolute;
            inset: 0;
            border-radius: var(--radius);
            pointer-events: none;
            transition: opacity 0.3s;
            opacity: 0;
            box-shadow: 0 0 60px rgba(124, 77, 255, 0.4);
        }
        .project-card-inner:hover .glow {
            opacity: 1;
        }

        /* ===== ESTATÍSTICAS ===== */
        .stats-row {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 30px;
        }
        .stat-item {
            text-align: center;
            background: var(--glass);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border: 1px solid var(--glass-border);
            border-radius: var(--radius);
            padding: 30px 28px;
            min-width: 160px;
            box-shadow: var(--shadow);
            transition: all var(--transition);
        }
        .stat-item:hover {
            transform: translateY(-6px);
            border-color: var(--primary2);
            box-shadow: 0 15px 45px rgba(124, 77, 255, 0.3);
        }
        .stat-item .number {
            font-size: 2.8rem;
            font-weight: 800;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* ===== FOOTER ===== */
        footer {
            text-align: center;
            padding: 30px;
            color: var(--text2);
            font-size: 0.85rem;
            position: relative;
            z-index: 1;
            border-top: 1px solid var(--glass-border);
            margin-top: 40px;
            background: var(--glass);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
        }

        /* ===== RESPONSIVO ===== */
        @media (max-width: 768px) {
            .navbar {
                gap: 4px;
                padding: 8px 14px;
                top: 8px;
            }
            .navbar a {
                font-size: 0.75rem;
                padding: 6px 10px;
            }
            .hero h1 {
                font-size: 2rem;
            }
            .projects-grid {
                grid-template-columns: 1fr;
            }
            .skills-grid {
                grid-template-columns: 1fr 1fr;
            }
            .timeline {
                padding-left: 25px;
            }
            .timeline-item {
                padding-left: 15px;
            }
            .timeline-item::before {
                left: -22px;
                width: 12px;
                height: 12px;
            }
            .timeline::before {
                left: 12px;
            }
        }
        @media (max-width: 480px) {
            .skills-grid {
                grid-template-columns: 1fr;
            }
            .stats-row {
                gap: 16px;
            }
            .stat-item {
                min-width: 120px;
                padding: 20px 16px;
            }
            .stat-item .number {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <!-- Canvas de Partículas -->
    <canvas id="particleCanvas"></canvas>

    <!-- Conteúdo -->
    <div class="main-content">
        <!-- Navbar -->
            <nav class="navbar" id="navbar">
        <a href="#hero" class="active">Início</a>
        <a href="#skills">Skills</a>
        <a href="#projetos">Projetos</a>
        <a href="#timeline">Jornada</a>
        <a href="#terminal">Terminal</a>
        <a href="#contato">Contato</a>
        <button class="theme-toggle" id="themeToggle" title="Alternar tema">🌙</button>
    </nav>

    <!-- Hero -->
    <section class="hero" id="hero">
        <span class="hero-badge">🚀 Curso Técnico em Informática</span>
        <h1>Desenvolvedor Full Stack</h1>
        <div class="typing-wrapper">
            <span id="typingText"></span><span class="cursor-blink"></span>
        </div>
        <div class="hero-cta">
            <a href="#projetos" class="btn btn-primary">📂 Ver Projetos</a>
            <a href="#terminal" class="btn btn-outline">💻 Abrir Terminal</a>
        </div>
    </section>

    <!-- Skills -->
    <section id="skills">
        <div class="section-title">
            <h2>Habilidades Técnicas</h2>
            <div class="line"></div>
        </div>
        <div class="skills-grid" id="skillsGrid">
            <!-- preenchido dinamicamente -->
        </div>
    </section>

    <!-- Projetos 3D -->
    <section id="projetos">
        <div class="section-title">
            <h2>Projetos em Destaque</h2>
            <div class="line"></div>
        </div>
        <div class="projects-grid" id="projectsGrid">
            <!-- preenchido dinamicamente -->
        </div>
    </section>

    <!-- Timeline -->
    <section id="timeline">
        <div class="section-title">
            <h2>Minha Jornada</h2>
            <div class="line"></div>
        </div>
        <div class="timeline" id="timelineContainer">
            <!-- preenchido dinamicamente -->
        </div>
    </section>

    <!-- Terminal Interativo -->
    <section id="terminal">
        <div class="section-title">
            <h2>Terminal Interativo</h2>
            <div class="line"></div>
            <p style="color:var(--text2);margin-top:8px;">Digite <strong>help</strong> para ver os comandos disponíveis</p>
        </div>
        <div class="terminal-section">
            <div class="terminal-header">
                <span class="terminal-dot red"></span>
                <span class="terminal-dot yellow"></span>
                <span class="terminal-dot green"></span>
                <span class="terminal-title">dev@portfolio:~</span>
            </div>
            <div class="terminal-body" id="terminalBody">
                <div class="line"><span class="output">Bem-vindo ao terminal interativo! Digite um comando abaixo.</span></div>
                <div class="line"><span class="output">-------------------------------------------</span></div>
            </div>
            <div style="padding:12px 20px;display:flex;align-items:center;gap:8px;background:rgba(0,0,0,0.2);">
                <span class="prompt" style="font-family:var(--mono);color:#7c4dff;font-weight:700;">dev@portfolio:~$</span>
                <input type="text" id="terminalInput" autofocus autocomplete="off" style="flex:1;background:transparent;border:none;color:#00e5ff;font-family:var(--mono);font-size:0.9rem;outline:none;" placeholder="Digite um comando...">
            </div>
        </div>
    </section>

    <!-- Estatísticas -->
    <section id="contato">
        <div class="section-title">
            <h2>Em Números</h2>
            <div class="line"></div>
        </div>
        <div class="stats-row" id="statsRow">
            <!-- preenchido dinamicamente -->
        </div>
    </section>

    <!-- Footer -->
    <footer>
        <p>© 2026 — Feito com 💜 para o Curso Técnico | <span style="color:var(--primary2);">#ImpressionaProfessor</span></p>
    </footer>
</div>

<script>
    (function() {
        // ========== PARTÍCULAS NO CANVAS ==========
        const canvas = document.getElementById('particleCanvas');
        const ctx = canvas.getContext('2d');
        let particles = [];
        let mouseX = -1000,
            mouseY = -1000;
        const isDark = () => !document.body.classList.contains('light-mode');

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        class Particle {
            constructor() {
                this.reset();
            }
            reset() {
                this.x = Math.random() * canvas.width;
                this.y = Math.random() * canvas.height;
                this.size = Math.random() * 2.5 + 0.8;
                this.speedX = (Math.random() - 0.5) * 0.7;
                this.speedY = (Math.random() - 0.5) * 0.7;
                this.opacity = Math.random() * 0.6 + 0.3;
                this.color = isDark() ?
                    `rgba(${Math.random()>0.5?124:0}, ${Math.random()>0.5?77:229}, 255, ` :
                    `rgba(${Math.random()>0.5?80:0}, ${Math.random()>0.5?50:150}, 200, `;
            }
            update() {
                this.x += this.speedX;
                this.y += this.speedY;
                const dx = mouseX - this.x;
                const dy = mouseY - this.y;
                const dist = Math.sqrt(dx * dx + dy * dy);
                if (dist < 150) {
                    const force = (150 - dist) / 150;
                    this.x -= dx * force * 0.03;
                    this.y -= dy * force * 0.03;
                    this.size = Math.min(this.size + force * 0.5, 4);
                } else {
                    this.size = Math.max(this.size - 0.02, 0.8);
                }
                if (this.x < -20) this.x = canvas.width + 20;
                if (this.x > canvas.width + 20) this.x = -20;
                if (this.y < -20) this.y = canvas.height + 20;
                if (this.y > canvas.height + 20) this.y = -20;
            }
            draw() {
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fillStyle = this.color + this.opacity + ')';
                ctx.fill();
            }
        }

        function initParticles(count) {
            particles = [];
            for (let i = 0; i < count; i++) {
                particles.push(new Particle());
            }
        }
        initParticles(100);

        function animateParticles() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (let i = 0; i < particles.length; i++) {
                for (let j = i + 1; j < particles.length; j++) {
                    const dx = particles[i].x - particles[j].x;
                    const dy = particles[i].y - particles[j].y;
                    const dist = Math.sqrt(dx * dx + dy * dy);
                    if (dist < 100) {
                        ctx.beginPath();
                        ctx.moveTo(particles[i].x, particles[i].y);
                        ctx.lineTo(particles[j].x, particles[j].y);
                        const alpha = (1 - dist / 100) * 0.25;
                        ctx.strokeStyle = isDark() ?
                            `rgba(124,77,255,${alpha})` :
                            `rgba(100,60,200,${alpha})`;
                        ctx.lineWidth = 0.6;
                        ctx.stroke();
                    }
                }
            }
            particles.forEach(p => { p.update();
                p.draw(); });
            requestAnimationFrame(animateParticles);
        }
        animateParticles();

        document.addEventListener('mousemove', (e) => {
            mouseX = e.clientX;
            mouseY = e.clientY;
        });
        document.addEventListener('touchmove', (e) => {
            mouseX = e.touches[0].clientX;
            mouseY = e.touches[0].clientY;
        }, { passive: true });

        // ========== TEMA CLARO/ESCURO ==========
        const themeToggle = document.getElementById('themeToggle');
        themeToggle.addEventListener('click', () => {
            document.body.classList.toggle('light-mode');
            themeToggle.textContent = document.body.classList.contains('light-mode') ? '☀️' : '🌙';
            particles.forEach(p => p.reset());
        });

        // ========== TYPING EFFECT ==========
        const typingTexts = [
            'React.js • Node.js • Python • SQL',
            'HTML5 • CSS3 • JavaScript • TypeScript',
            'Criando soluções inovadoras 🚀',
            'Estudante dedicado do Curso Técnico 💻',
            'Apaixonado por tecnologia e design ✨'
        ];
        let typingIndex = 0;
        let charIndex = 0;
        let isDeleting = false;
        const typingEl = document.getElementById('typingText');

        function typeEffect() {
            const current = typingTexts[typingIndex];
            if (isDeleting) {
                charIndex--;
                typingEl.textContent = current.substring(0, charIndex);
            } else {
                charIndex++;
                typingEl.textContent = current.substring(0, charIndex);
            }
            if (!isDeleting && charIndex === current.length) {
                setTimeout(() => { isDeleting = true;
                    typeEffect(); }, 1800);
                return;
            }
            if (isDeleting && charIndex === 0) {
                isDeleting = false;
                typingIndex = (typingIndex + 1) % typingTexts.length;
                setTimeout(typeEffect, 300);
                return;
            }
            const speed = isDeleting ? 30 : 70;
            setTimeout(typeEffect, speed);
        }
        typeEffect();

        // ========== SKILLS COM PROGRESS BARS ==========
        const skillsData = [
            { icon: '⚛️', name: 'React.js', level: 88 },
            { icon: '🟢', name: 'Node.js', level: 82 },
            { icon: '🐍', name: 'Python', level: 90 },
            { icon: '🗄️', name: 'MySQL', level: 78 },
            { icon: '🎨', name: 'CSS/Design', level: 85 },
            { icon: '🔧', name: 'Git/GitHub', level: 92 },
            { icon: '📱', name: 'Responsividade', level: 87 },
            { icon: '🔐', name: 'Segurança Web', level: 72 },
        ];
        const skillsGrid = document.getElementById('skillsGrid');
        skillsData.forEach(skill => {
            const card = document.createElement('div');
            card.className = 'skill-card';
            card.innerHTML = `
                <div class="icon">${skill.icon}</div>
                <h3>${skill.name}</h3>
                <div class="progress-bar">
                    <div class="progress-fill" data-width="${skill.level}%" style="width:0%"></div>
                </div>
                <small style="color:var(--text2);">${skill.level}%</small>
            `;
            skillsGrid.appendChild(card);
        });

        const progressFills = document.querySelectorAll('.progress-fill');
        const observerSkills = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    const fills = entry.target.querySelectorAll('.progress-fill');
                    fills.forEach(f => {
                        f.style.width = f.dataset.width;
                    });
                }
            });
        }, { threshold: 0.3 });
        observerSkills.observe(skillsGrid);

        // ========== PROJETOS 3D COM TILT ==========
        const projectsData = [
            { icon: '🛒', name: 'E-commerce Full', desc: 'Loja virtual com React, Node e Stripe. Carrinho, pagamentos e dashboard admin.' },
            { icon: '📊', name: 'Dashboard Analytics', desc: 'Painel interativo com gráficos em tempo real usando D3.js e WebSockets.' },
            { icon: '🤖', name: 'Chatbot IA', desc: 'Assistente virtual com integração à API OpenAI e interface customizada.' },
            { icon: '🎮', name: 'Game Multiplayer', desc: 'Jogo online em tempo real com Socket.io e Canvas API.' },
            { icon: '📝', name: 'Sistema Acadêmico', desc: 'CRUD completo para gestão de notas, alunos e turmas com autenticação JWT.' },
            { icon: '🌐', name: 'Rede Social', desc: 'Mini rede social com feed, likes, comentários e upload de imagens.' },
        ];
        const projectsGrid = document.getElementById('projectsGrid');
        projectsData.forEach(proj => {
            const wrapper = document.createElement('div');
            wrapper.className = 'project-card-3d';
            wrapper.innerHTML = `
                <div class="project-card-inner">
                    <div class="glow"></div>
                    <div class="proj-icon">${proj.icon}</div>
                    <h3>${proj.name}</h3>
                    <p>${proj.desc}</p>
                </div>
            `;
            projectsGrid.appendChild(wrapper);

            const inner = wrapper.querySelector('.project-card-inner');
            wrapper.addEventListener('mousemove', (e) => {
                const rect = wrapper.getBoundingClientRect();
                const x = e.clientX - rect.left;
                const y = e.clientY - rect.top;
                const centerX = rect.width / 2;
                const centerY = rect.height / 2;
                const rotateX = ((y - centerY) / centerY) * -12;
                const rotateY = ((x - centerX) / centerX) * 12;
                inner.style.transform = `rotateX(${rotateX}deg) rotateY(${rotateY}deg) scale(1.04)`;
                inner.querySelector('.glow').style.opacity = '0.7';
            });
            wrapper.addEventListener('mouseleave', () => {
                inner.style.transform = 'rotateX(0deg) rotateY(0deg) scale(1)';
                inner.querySelector('.glow').style.opacity = '0';
            });
        });

        // ========== TIMELINE ==========
        const timelineData = [
            { date: '2024 - Início', title: 'Ingresso no Curso Técnico', desc: 'Comecei minha jornada no curso técnico em informática, aprendendo lógica de programação e fundamentos.' },
            { date: '2025 - Meio', title: 'Primeiro Projeto Full Stack', desc: 'Desenvolvi uma aplicação completa com front-end em React e back-end em Node.js com banco de dados.' },
            { date: '2025 - Final', title: 'Estágio em Empresa Tech', desc: 'Conquistei um estágio onde apliquei conhecimentos em projetos reais, usando metodologias ágeis.' },
            { date: '2026 - Atual', title: 'Especialização & Portfólio', desc: 'Aprofundando em TypeScript, Docker e Cloud. Construindo este portfólio interativo!' },
        ];
        const timelineContainer = document.getElementById('timelineContainer');
        timelineData.forEach(item => {
            const div = document.createElement('div');
            div.className = 'timeline-item';
            div.innerHTML = `
                <span class="date">${item.date}</span>
                <h4>${item.title}</h4>
                <p style="color:var(--text2);">${item.desc}</p>
            `;
            timelineContainer.appendChild(div);
        });

        const timelineObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                }
            });
        }, { threshold: 0.25 });
        document.querySelectorAll('.timeline-item').forEach(el => timelineObserver.observe(el));

        // ========== TERMINAL INTERATIVO ==========
        const terminalBody = document.getElementById('terminalBody');
        const terminalInput = document.getElementById('terminalInput');

        const commands = {
            help: () => `
                <span class="output">Comandos disponíveis:</span><br>
                <span class="output">• <strong>whoami</strong> — Quem sou eu</span><br>
                <span class="output">• <strong>skills</strong> — Minhas habilidades</span><br>
                <span class="output">• <strong>projetos</strong> — Listar projetos</span><br>
                <span class="output">• <strong>date</strong> — Data e hora atuais</span><br>
                <span class="output">• <strong>clear</strong> — Limpar terminal</span><br>
                <span class="output">• <strong>neofetch</strong> — Info do sistema</span><br>
                <span class="output">• <strong>ls</strong> — Listar arquivos fictícios</span><br>
                <span class="output">• <strong>echo [texto]</strong> — Repetir texto</span><br>
            `,
            whoami: () => '👨‍💻 <span class="output">Estudante do Curso Técnico em Informática | Desenvolvedor Full Stack | Apaixonado por tecnologia 🚀</span>',
            skills: () => '🛠️ <span class="output">HTML, CSS, JavaScript, React, Node.js, Python, MySQL, Git, Docker, TypeScript</span>',
            projetos: () => '📂 <span class="output">E-commerce, Dashboard Analytics, Chatbot IA, Game Multiplayer, Sistema Acadêmico, Rede Social</span>',
            date: () => '📅 <span class="output">' + new Date().toLocaleString('pt-BR') + '</span>',
            clear: () => '__CLEAR__',
            neofetch: () => `
                <pre class="output" style="margin:0;line-height:1.3;">
    ⠀⠀⠀⢀⣤⣶⣶⣤⡀⠀⠀   OS: DevOS v26.05
    ⠀⠀⣾⣿⣿⣿⣿⣿⣷⠀   Host: Portfolio Interativo
    ⠀⢸⣿⣿⣿⣿⣿⣿⣿⡇   Kernel: JavaScript ES2026
    ⠀⣿⣿⣿⣿⣿⣿⣿⣿⣿   Shell: Terminal.js 1.0
    ⠀⢿⣿⣿⣿⣿⣿⣿⣿⡿   RAM: ∞ GB
    ⠀⠈⠻⣿⣿⣿⣿⠟⠁⠀   Uptime: Desde 2024
                </pre>
            `,
            ls: () => '📁 <span class="output">projetos/  src/  assets/  docs/  README.md  package.json  .gitignore</span>',
        };

        function appendTerminalLine(html) {
            const line = document.createElement('div');
            line.className = 'line';
            line.innerHTML = html;
            terminalBody.appendChild(line);
            terminalBody.scrollTop = terminalBody.scrollHeight;
        }

        function processCommand(cmd) {
            const trimmed = cmd.trim().toLowerCase();
            appendTerminalLine(`<span class="prompt">dev@portfolio:~$</span> <span style="color:#00e5ff;">${cmd}</span>`);
            if (trimmed === '') return;
            if (trimmed.startsWith('echo ')) {
                const msg = cmd.substring(5).trim();
                appendTerminalLine(`<span class="output">${msg || '(vazio)'}</span>`);
                return;
            }
            if (commands[trimmed]) {
                const result = commands[trimmed]();
                if (result === '__CLEAR__') {
                    terminalBody.innerHTML = '';
                    return;
                }
                appendTerminalLine(result);
            } else {
                appendTerminalLine(`<span style="color:#ff4081;">❌ Comando não encontrado: "${cmd}". Digite <strong>help</strong> para ver a lista.</span>`);
            }
        }

        terminalInput.addEventListener('keydown', (e) => {
            if (e.key === 'Enter') {
                const cmd = terminalInput.value;
                processCommand(cmd);
                terminalInput.value = '';
            }
        });
        document.querySelector('.terminal-section').addEventListener('click', () => {
            terminalInput.focus();
        });

        // ========== ESTATÍSTICAS COM CONTADOR ==========
        const statsData = [
            { number: 0, target: 24, label: 'Projetos', suffix: '+' },
            { number: 0, target: 3500, label: 'Horas Código', suffix: 'h' },
            { number: 0, target: 18, label: 'Tecnologias', suffix: '' },
            { number: 0, target: 100, label: 'Commits', suffix: '%' },
        ];
        const statsRow = document.getElementById('statsRow');
        statsData.forEach(stat => {
            const div = document.createElement('div');
            div.className = 'stat-item';
            div.innerHTML = `
                <div class="number" data-target="${stat.target}" data-suffix="${stat.suffix}">0</div>
                                <div style="color:var(--text2);font-weight:600;">${stat.label}</div>
            `;
            statsRow.appendChild(div);
        });

        const numberEls = document.querySelectorAll('.stat-item .number');
        const statsObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    const el = entry.target;
                    const target = parseInt(el.dataset.target);
                    const suffix = el.dataset.suffix;
                    let current = 0;
                    const increment = Math.ceil(target / 50);
                    const interval = setInterval(() => {
                        current += increment;
                        if (current >= target) {
                            current = target;
                            clearInterval(interval);
                        }
                        el.textContent = current + suffix;
                    }, 35);
                    statsObserver.unobserve(el);
                }
            });
        }, { threshold: 0.5 });
        numberEls.forEach(el => statsObserver.observe(el));

        // ========== NAVBAR ATIVA NO SCROLL ==========
        const sections = document.querySelectorAll('section[id]');
        const navLinks = document.querySelectorAll('.navbar a');
        window.addEventListener('scroll', () => {
            let current = '';
            sections.forEach(section => {
                const sectionTop = section.offsetTop - 120;
                if (window.scrollY >= sectionTop) {
                    current = section.getAttribute('id');
                }
            });
            navLinks.forEach(link => {
                link.classList.remove('active');
                if (link.getAttribute('href') === '#' + current) {
                    link.classList.add('active');
                }
            });
        });

        console.log('%c🚀 Site carregado com sucesso! %cImpressione seu professor! 💜',
            'font-size:1.2em;color:#7c4dff;', 'font-size:1em;color:#00e5ff;');
        console.log('%cDica: Abra o terminal e digite "help" para interagir!', 'color:#ff4081;');

        setTimeout(() => {
            const rect = skillsGrid.getBoundingClientRect();
            if (rect.top < window.innerHeight) {
                skillsGrid.querySelectorAll('.progress-fill').forEach(f => {
                    f.style.width = f.dataset.width;
                });
            }
        }, 500);
    })();
</script>
</body>
</html>
