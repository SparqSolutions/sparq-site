<script>
  import { onMount } from 'svelte';
  
  onMount(() => {
    // Simple electrical effects without canvas
    createSimpleEffects();
  });
  
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
</script>

<div class="app-container">
  <!-- Advanced PCB Background -->
  <div class="pcb-background"></div>
  
  <!-- Navigation -->
  <nav class="main-nav">
    <div class="nav-container">
      <div class="nav-logo">
        <img src="/logo.png" alt="SPARQ" class="nav-logo-img" />
        <span class="nav-brand">SPARQ</span>
      </div>
      <div class="nav-links">
        <a href="#home" class="nav-link active">Home</a>
        <a href="#solutions" class="nav-link">Solutions</a>
        <a href="#demos" class="nav-link">AI Demos</a>
        <a href="#about" class="nav-link">About</a>
        <a href="#contact" class="nav-link">Contact</a>
      </div>
      <button class="nav-cta">Get Started</button>
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
</div>

<style>
  :global(*) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  :global(body) {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
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
    background-image: 
      /* Realistic PCB grid */
      linear-gradient(rgba(184, 134, 11, 0.08) 1px, transparent 1px),
      linear-gradient(90deg, rgba(184, 134, 11, 0.08) 1px, transparent 1px),
      /* Circuit traces */
      linear-gradient(45deg, transparent 48%, rgba(218, 165, 32, 0.05) 49%, rgba(218, 165, 32, 0.05) 51%, transparent 52%),
      linear-gradient(-45deg, transparent 48%, rgba(218, 165, 32, 0.05) 49%, rgba(218, 165, 32, 0.05) 51%, transparent 52%),
      /* Via holes */
      radial-gradient(circle at 25% 25%, rgba(184, 134, 11, 0.15) 1px, transparent 2px),
      radial-gradient(circle at 75% 75%, rgba(184, 134, 11, 0.12) 1px, transparent 2px),
      /* Component pads */
      radial-gradient(circle at 50% 50%, rgba(218, 165, 32, 0.08) 2px, transparent 3px);
    background-size: 
      25px 25px,
      25px 25px,
      50px 50px,
      50px 50px,
      100px 100px,
      150px 150px,
      200px 200px;
    animation: pcb-flow 30s linear infinite;
  }
  
  @keyframes pcb-flow {
    0% { background-position: 0 0, 0 0, 0 0, 0 0, 0 0, 0 0, 0 0; }
    100% { background-position: 25px 25px, -25px 25px, 50px 50px, -50px 50px, 100px 100px, -150px 150px, 200px 200px; }
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
    width: 32px;
    height: 32px;
    filter: brightness(1.2) contrast(1.1) drop-shadow(0 0 8px rgba(184, 134, 11, 0.3));
  }
  
  .nav-brand {
    font-size: 1.5rem;
    font-weight: 700;
    background: linear-gradient(135deg, #B8860B 0%, #DAA520 25%, #CD853F 50%, #B8860B 75%, #8B7355 100%);
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
</style> 