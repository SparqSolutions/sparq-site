<script lang="ts">
  import { onMount } from 'svelte';
  
  type Point = { x: number, y: number };
  type Path = { start: Point, end: Point };

  class Particle {
    x: number = 0;
    y: number = 0;
    path: Path | null = null;
    progress: number = 0;
    forwards: boolean = true;
    lifetime: number = 0;
    alpha: number = 1;

    constructor(circuitPaths: Path[]) {
      this.reset(circuitPaths);
    }

    reset(circuitPaths: Path[]) {
      if (circuitPaths.length === 0) return;
      const pathIndex = Math.floor(Math.random() * circuitPaths.length);
      this.path = circuitPaths[pathIndex];
      this.progress = Math.random();
      this.forwards = Math.random() > 0.5;
      this.updatePosition();
      this.lifetime = 1000 + Math.random() * 1000; // 1-2 seconds
      this.alpha = 1;
    }

    updatePosition() {
      if (!this.path) return;
      const p = this.forwards ? this.progress : 1 - this.progress;
      this.x = this.path.start.x + (this.path.end.x - this.path.start.x) * p;
      this.y = this.path.start.y + (this.path.end.y - this.path.start.y) * p;
    }

    findNextPath(circuitPaths: Path[]) {
      if (!this.path) { this.reset(circuitPaths); return; }
      const endPoint = this.forwards ? this.path.end : this.path.start;
      const connectedPaths = circuitPaths.filter(p => (p.start === endPoint || p.end === endPoint) && p !== this.path);

      if (connectedPaths.length > 0) {
        this.path = connectedPaths[Math.floor(Math.random() * connectedPaths.length)];
        this.forwards = this.path.start === endPoint;
        this.progress = 0;
      } else {
        this.reset(circuitPaths);
      }
    }

    update(ctx: CanvasRenderingContext2D, particleSpeed: number, circuitPaths: Path[], deltaTime: number) {
      if (!this.path || !ctx) return;
      
      this.lifetime -= deltaTime;
      if (this.lifetime <= 0) {
        this.reset(circuitPaths);
      }
      
      const pathLength = Math.hypot(this.path.end.x - this.path.start.x, this.path.end.y - this.path.start.y);
      if(pathLength > 0) {
          this.progress += particleSpeed / pathLength;
      }

      if (this.progress >= 1) {
        this.findNextPath(circuitPaths);
      }

      this.updatePosition();

      this.alpha = Math.min(1.0, this.lifetime / 500.0);

      ctx.fillStyle = `rgba(255, 215, 100, ${0.8 * this.alpha})`;
      ctx.shadowColor = `rgba(255, 215, 0, ${this.alpha})`;
      ctx.shadowBlur = 5;
      ctx.beginPath();
      ctx.arc(this.x, this.y, 1.2, 0, Math.PI * 2);
      ctx.fill();
      ctx.shadowBlur = 0;
    }
  }
  
  onMount(() => {
    // Simple electrical effects without canvas
    createSimpleEffects();
    
    // Generative PCB background
    setupPcbAnimation();
  });
  
  function setupPcbAnimation() {
    const canvas = document.getElementById('pcb-canvas') as HTMLCanvasElement;
    if (!canvas) return;
    const ctx = canvas.getContext('2d');
    if (!ctx) return;

    let width: number, height: number, dpr: number;
    let particles: Particle[] = [];
    const numParticles = 30;
    const particleSpeed = 0.5;

    let circuitPaths: Path[] = [];
    let nodes = new Map<string, Point>();

    function getNodeKey(p: Point) {
      return `${p.x},${p.y}`;
    }

    function createCircuitLayout() {
      circuitPaths = [];
      nodes.clear();
      const gridSize = 40;
      const cols = Math.floor(width / gridSize);
      const rows = Math.floor(height / gridSize);

      for (let y = 0; y <= rows; y++) {
        for (let x = 0; x <= cols; x++) {
          const point = { x: x * gridSize, y: y * gridSize };
          nodes.set(getNodeKey(point), point);
        }
      }

      for (const node of nodes.values()) {
        const x = node.x / gridSize;
        const y = node.y / gridSize;

        if (x < cols && Math.random() > 0.5) {
          const rightNeighbor = nodes.get(getNodeKey({x: (x + 1) * gridSize, y: node.y}));
          if (rightNeighbor) circuitPaths.push({ start: node, end: rightNeighbor });
        }
        if (y < rows && Math.random() > 0.5) {
          const bottomNeighbor = nodes.get(getNodeKey({x: node.x, y: (y + 1) * gridSize}));
          if (bottomNeighbor) circuitPaths.push({ start: node, end: bottomNeighbor });
        }
      }
    }

    function drawStaticBoard() {
      if (!ctx) return;
      ctx.strokeStyle = 'rgba(218, 165, 32, 0.08)';
      ctx.lineWidth = 0.7;
      circuitPaths.forEach(path => {
        ctx.beginPath();
        ctx.moveTo(path.start.x, path.start.y);
        ctx.lineTo(path.end.x, path.end.y);
        ctx.stroke();
      });
    }

    function init() {
      resize();
      createCircuitLayout();
      particles = [];
      for (let i = 0; i < numParticles; i++) {
        particles.push(new Particle(circuitPaths));
      }
    }

    let lastTime = 0;
    function animate(time = 0) {
      if (!ctx) return;
      
      const deltaTime = time - lastTime;
      lastTime = time;

      ctx.fillStyle = 'rgba(13, 13, 13, 0.2)';
      ctx.fillRect(0, 0, width, height);

      drawStaticBoard();

      particles.forEach(p => p.update(ctx, particleSpeed, circuitPaths, deltaTime));
      requestAnimationFrame(animate);
    }

    function resize() {
      if (!ctx) return;
      dpr = window.devicePixelRatio || 1;
      width = window.innerWidth;
      height = window.innerHeight;
      canvas.width = width * dpr;
      canvas.height = height * dpr;
      canvas.style.width = `${width}px`;
      canvas.style.height = `${height}px`;
      ctx.scale(dpr, dpr);
    }

    window.addEventListener('resize', init);
    init();
    requestAnimationFrame(animate);

    return () => {
      window.removeEventListener('resize', init);
    };
  }
  
  function createSimpleEffects() {
    // Add random sparks to existing elements
    setInterval(() => {
      const sparkElements = document.querySelectorAll('.spark-target');
      sparkElements.forEach(el => {
        if (Math.random() < 0.1) {
          el.classList.add('spark-active');
          setTimeout(() => el.classList.remove('spark-active'), 1000);
        }
      });
    }, 2000);
  }

  let isChatOpen = false;

  function toggleChat() {
    isChatOpen = !isChatOpen;
  }
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;700&display=swap" rel="stylesheet">
</svelte:head>

<div class="app-container">
  <!-- Advanced PCB Background -->
  <div class="pcb-background">
    <canvas id="pcb-canvas"></canvas>
    <div class="occlusion-vignette"></div>
  </div>
  
  <!-- Navigation -->
  <nav class="main-nav">
    <div class="nav-container">
      <div class="nav-links">
        <a href="#home" class="nav-link active">Home</a>
        <a href="#solutions" class="nav-link">Solutions</a>
        <a href="#demos" class="nav-link">AI Demos</a>
        <a href="#about" class="nav-link">About</a>
        <a href="#contact" class="nav-link">Contact</a>
      </div>
      <button class="nav-cta">Let's Go!</button>
    </div>
  </nav>
  
  <main class="main-content">
    <slot />
  </main>
  
  <!-- Footer -->
  <footer class="main-footer">
    <div class="footer-container">
      <div class="footer-section">
        <h4>SPARQ AI Solutions</h4>
        <p>Premium artificial intelligence crafted for enterprise excellence.</p>
      </div>
      <div class="footer-section">
        <h4>Services</h4>
        <ul>
          <li><a href="#custom-ai">Custom AI Development</a></li>
          <li><a href="#consulting">AI Consulting</a></li>
          <li><a href="#integration">System Integration</a></li>
        </ul>
      </div>
      <div class="footer-section">
        <h4>Resources</h4>
        <ul>
          <li><a href="#docs">Documentation</a></li>
          <li><a href="#support">Support</a></li>
          <li><a href="#blog">Blog</a></li>
        </ul>
      </div>
    </div>
  </footer>

  <!-- Chat Nub -->
  <div class="chat-nub-container">
    
    <button class="chat-nub-button" on:click={toggleChat}>
      <img src="/logoold.png" alt="Chat Agent" />
    </button>
    {#if isChatOpen}
      <div class="chat-window glass-panel">
        <div class="chat-header">
          <h4 class="gold-gradient">SPARQ AI Assistant</h4>
          <button class="close-chat" on:click={toggleChat}>&times;</button>
        </div>
        <div class="chat-body">
          <p>AI agent integration coming soon...</p>
        </div>
        <div class="chat-footer">
            <input type="text" placeholder="Type your message..." />
            <button>Send</button>
        </div>
      </div>
    {/if}
  </div>
</div>

<style>
  :global(*) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  :global(body) {
    font-family: 'Orbitron', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    background: #0D0D0D;
    color: #E8E8E8;
    overflow-x: hidden;
    line-height: 1.6;
  }
  
  .app-container {
    position: relative;
    min-height: 100vh;
    background: 
      radial-gradient(ellipse at top, rgba(24, 24, 24, 0.8) 0%, rgba(13, 13, 13, 1) 100%),
      linear-gradient(135deg, #0D0D0D 0%, #1A1A1A 25%, #0F0F0F 50%, #1D1D1D 75%, #0D0D0D 100%);
  }
  
  .pcb-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 0;
  }
  
  .occlusion-vignette {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: radial-gradient(ellipse at center, rgba(13, 13, 13, 0.95) 25%, rgba(13, 13, 13, 0.2) 75%);
    pointer-events: none;
  }
  
  #pcb-canvas {
    width: 100%;
    height: 100%;
    display: block;
  }
  
  /* Navigation */
  .main-nav {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 100;
    background: rgba(13, 13, 13, 0.95);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid rgba(184, 134, 11, 0.25);
  }
  
  .nav-container {
    max-width: 1400px;
    margin: 0 auto;
    padding: 1rem 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  
  .nav-logo {
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }
  
  .nav-logo-img {
    width: 40px;
    height: 40px;
    object-fit: contain;
    filter: brightness(1.2) contrast(1.1) drop-shadow(0 0 8px rgba(184, 134, 11, 0.3));
  }
  
  .nav-brand {
    font-size: 1.5rem;
    font-weight: 700;
    background: linear-gradient(135deg, #D4AF37 0%, #B48811 25%, #E2B638 50%, #C49A28 75%, #AD8A2C 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  
  .nav-links {
    display: flex;
    gap: 2rem;
  }
  
  .nav-link {
    color: #B0B0B0;
    text-decoration: none;
    font-weight: 500;
    transition: all 0.3s ease;
    position: relative;
  }
  
  .nav-link:hover,
  .nav-link.active {
    color: #DAA520;
  }
  
  .nav-link.active::after {
    content: '';
    position: absolute;
    bottom: -8px;
    left: 0;
    right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, #DAA520, transparent);
  }
  
  .nav-cta {
    background: linear-gradient(135deg, #B8860B 0%, #DAA520 100%);
    color: #0D0D0D;
    border: none;
    padding: 0.75rem 1.5rem;
    border-radius: 6px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
  }
  
  .nav-cta:hover {
    transform: translateY(-1px);
    box-shadow: 0 8px 25px rgba(218, 165, 32, 0.3);
  }
  
  .main-content {
    position: relative;
    z-index: 10;
    padding-top: 80px;
  }
  
  /* Footer */
  .main-footer {
    background: rgba(13, 13, 13, 0.98);
    border-top: 1px solid rgba(184, 134, 11, 0.25);
    padding: 3rem 0 1.5rem;
    margin-top: 4rem;
    position: relative;
    z-index: 10;
  }
  
  .footer-container {
    max-width: 1400px;
    margin: 0 auto;
    padding: 0 2rem;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 2rem;
  }
  
  .footer-section h4 {
    color: #DAA520;
    margin-bottom: 1rem;
    font-weight: 600;
  }
  
  .footer-section ul {
    list-style: none;
  }
  
  .footer-section ul li {
    margin-bottom: 0.5rem;
  }
  
  .footer-section a {
    color: #B0B0B0;
    text-decoration: none;
    transition: color 0.3s ease;
  }
  
  .footer-section a:hover {
    color: #DAA520;
  }
  
  /* Global utility classes */
  :global(.gold-gradient) {
    background: linear-gradient(135deg, #B8860B 0%, #DAA520 25%, #CD853F 50%, #B8860B 75%, #8B7355 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  
  :global(.glass-panel) {
    background: rgba(26, 26, 26, 0.85);
    backdrop-filter: blur(20px);
    border: 1px solid rgba(184, 134, 11, 0.2);
    border-radius: 12px;
    box-shadow: 
      0 8px 32px rgba(0, 0, 0, 0.4),
      inset 0 1px 0 rgba(218, 165, 32, 0.1);
  }
  
  :global(.interactive-card) {
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    cursor: pointer;
  }
  
  :global(.interactive-card:hover) {
    transform: translateY(-4px);
    box-shadow: 
      0 20px 40px rgba(0, 0, 0, 0.6),
      0 0 30px rgba(218, 165, 32, 0.2),
      inset 0 1px 0 rgba(218, 165, 32, 0.25);
    border-color: rgba(218, 165, 32, 0.4);
  }
  
  :global(.spark-target) {
    position: relative;
  }
  
  :global(.spark-target.spark-active::before) {
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 4px;
    height: 4px;
    background: #FFD700;
    border-radius: 50%;
    box-shadow: 0 0 10px #FFD700, 0 0 20px #FFD700, 0 0 30px #FFD700;
    animation: spark-flash 1s ease-out;
  }
  
  @keyframes spark-flash {
    0% { opacity: 0; transform: translate(-50%, -50%) scale(0); }
    50% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
    100% { opacity: 0; transform: translate(-50%, -50%) scale(1.5); }
  }
  
  :global(.section-title) {
    font-size: 2.5rem;
    font-weight: 700;
    margin-bottom: 1rem;
    text-align: center;
  }
  
  :global(.section-subtitle) {
    font-size: 1.2rem;
    color: #B0B0B0;
    text-align: center;
    margin-bottom: 3rem;
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
  }
  
  /* Responsive design */
  @media (max-width: 768px) {
    .nav-container {
      padding: 1rem;
    }
    
    .nav-links {
      display: none;
    }
    
    .nav-cta {
      padding: 0.5rem 1rem;
      font-size: 0.9rem;
    }
    
    .footer-container {
      grid-template-columns: 1fr;
      text-align: center;
    }
  }

  /* Chat Nub */
  .chat-nub-container {
    position: fixed;
    bottom: 2rem;
    right: 2rem;
    z-index: 1000;
    display: flex;
    align-items: flex-end;
    gap: 1rem;
  }

  .chat-nub-button {
    width: 80px;
    height: 80px;
    border-radius: 50%;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    border: none;
    transition: all 0.3s ease;
    flex-shrink: 0;
    background: transparent;
  }
  
  .chat-nub-button:hover {
    transform: scale(1.1);
  }

  .chat-nub-button img {
    width: 90%;
    height: 90%;
    object-fit: contain;
  }
  
  .chat-prompt {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.5rem 1rem;
    background: rgba(218, 165, 32, 0.1);
    border: 1px solid rgba(218, 165, 32, 0.2);
    border-radius: 8px;
    color: #DAA520;
    animation: fade-in 0.5s ease;
  }
  
  @keyframes fade-in {
    from { opacity: 0; transform: translateX(10px); }
    to { opacity: 1; transform: translateX(0); }
  }

  .chat-window {
    position: absolute;
    bottom: calc(100% + 1rem);
    right: 0;
    width: 350px;
    max-width: 90vw;
    height: 500px;
    max-height: 70vh;
    border-radius: 12px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    transform-origin: bottom right;
    animation: open-chat 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  }

  @keyframes open-chat {
      from { opacity: 0; transform: scale(0.8); }
      to { opacity: 1; transform: scale(1); }
  }

  .chat-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    background: rgba(22, 28, 45, 0.7);
    border-bottom: 1px solid rgba(184, 134, 11, 0.25);
    flex-shrink: 0;
  }

  .chat-header h4 {
    font-weight: 600;
  }

  .close-chat {
    background: none;
    border: none;
    color: #B0B0B0;
    font-size: 1.5rem;
    cursor: pointer;
    line-height: 1;
    padding: 0;
  }
  
  .close-chat:hover {
    color: #DAA520;
  }

  .chat-body {
    flex-grow: 1;
    padding: 1rem;
    background: rgba(13, 13, 13, 0.8);
    color: #E8E8E8;
    overflow-y: auto;
  }
  
  .chat-footer {
    display: flex;
    padding: 0.75rem;
    border-top: 1px solid rgba(184, 134, 11, 0.25);
    background: rgba(22, 28, 45, 0.7);
    flex-shrink: 0;
  }
  
  .chat-footer input {
    flex-grow: 1;
    background: rgba(13, 13, 13, 0.9);
    border: 1px solid rgba(184, 134, 11, 0.25);
    border-radius: 6px;
    padding: 0.5rem;
    color: #E8E8E8;
    font-family: 'Orbitron', sans-serif;
  }

  .chat-footer input::placeholder {
    color: #888;
  }
  
  .chat-footer button {
    background: #DAA520;
    border: none;
    color: #0D0D0D;
    padding: 0.5rem 1rem;
    margin-left: 0.5rem;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 600;
  }
</style> 