<script lang="ts">
  import { onMount, beforeUpdate, afterUpdate, tick } from 'svelte';
  import { browser } from '$app/environment';
  import { PUBLIC_N8N_WEBHOOK_URL } from '$env/static/public';
  import { injectAnalytics } from '@vercel/analytics/sveltekit';
  
  injectAnalytics();

  type Point = { x: number, y: number };
  type Path = { start: Point, end: Point };

  // Chat functionality variables
  let chatInput = '';
  const n8nUrl = PUBLIC_N8N_WEBHOOK_URL;
  let chatMessages: { from: 'user' | 'bot', text: string }[] = [{
    from: 'bot',
    text: 'Hello! I am the Automated Assistant for Sparq Solutions. I can answer questions about our services and I can also schedule consultations for you! What can I help with?'
  }];
  let isSending = false;
  let sessionId = '';
  let chatInputElement: HTMLInputElement;

  // Simple formatting function for dates and times
  function formatChatMessage(text: string): string {
    if (!text) return text;

    let formatted = text
      // Format time slots (9 AM, 10:30 PM, etc.)
      .replace(/(\d{1,2}:?\d{0,2}\s*(?:AM|PM|am|pm))/g, '<span class="time-slot">$1</span>')

      // Format dates (Monday, Tuesday, tomorrow, today, etc.)
      .replace(/((?:Monday|Tuesday|Wednesday|Thursday|Friday|Saturday|Sunday|tomorrow|today),?\s*(?:\w+\s+\d{1,2}(?:st|nd|rd|th)?)?)/gi, '<span class="date-highlight">$1</span>')

      // Format CST/timezone mentions
      .replace(/\b(CST|Central|Central Time)\b/gi, '<span class="timezone">$1</span>')

      // Format confirmation messages
      .replace(/(you're all set|confirmed|scheduled|appointment confirmed)/gi, '<span class="confirmation">$1</span>')

      // Simple available times formatting - convert lists to bullet points
      .replace(/(?:available|open|free)\s*(?:times?|slots?)?\s*(?:include|are|:)?\s*((?:\d{1,2}:?\d{0,2}\s*(?:AM|PM|am|pm)(?:\s*,?\s*(?:and\s*)?\d{1,2}:?\d{0,2}\s*(?:AM|PM|am|pm))*)+)/gi,
        (match, times) => {
          const timeList = times.split(/,|\sand\s/).map((time: string) => time.trim()).filter((t: string) => t);
          const formattedTimes = timeList.map((time: string) => `• ${time}`).join('<br>');
          return `<div class="available-times">Available times:<br>${formattedTimes}</div>`;
        })

      // Format line breaks
      .replace(/\n/g, '<br>');

    return formatted;
  }

  // Chat functions
  async function sendMessage() {
    if (!chatInput.trim()) return;
    console.log('[Chat] sendMessage triggered.');

    const message = chatInput;
    chatMessages = [...chatMessages, { from: 'user', text: message }];
    console.log('[Chat] User message added to UI:', message);
    chatInput = '';
    isSending = true;

    try {
      if (!sessionId) {
        sessionId = crypto.randomUUID();
        console.log('[Chat] New session created with ID:', sessionId);
      }
      console.log(`[Chat] Preparing to send message for session: ${sessionId}`);

      const controller = new AbortController();
      const timeoutId = setTimeout(() => {
        console.warn('[Chat] Request timed out after 30 seconds.');
        controller.abort();
      }, 30000);

      //const requestBody = { message };
      const headers = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'sessionId': sessionId
      };

      console.log('[Chat] Sending POST request to:', n8nUrl);
      console.log('[Chat] Headers:', headers);
      console.log('[Chat] Body:', JSON.stringify(message, null, 2));

      const res = await fetch(n8nUrl, {
        method: 'POST',
        headers: headers,
        body: JSON.stringify({message: message}),
        signal: controller.signal
      });

      clearTimeout(timeoutId);

      console.log(`[Chat] Received response. Status: ${res.status}, StatusText: ${res.statusText}`);

      if (!res.ok) {
        const errorBody = await res.text();
        console.error(`[Chat] HTTP Error Response! Status: ${res.status}. Body: ${errorBody}`);
        throw new Error(`HTTP error! status: ${res.status}`);
      }

      const data = await res.json();
      console.log('[Chat] Successfully parsed JSON response:', data);

      const botReply = data.reply || data.output || data.response || data.message || data.text || data.content || 'No response received';
      console.log('[Chat] Extracted bot reply:', botReply);

      chatMessages = [...chatMessages, { from: 'bot', text: botReply }];

    } catch (e: any) {
      console.error('[Chat] Error in sendMessage:', e);

      let errorMessage = 'Error contacting server.';
      if (e.name === 'AbortError') {
        errorMessage = 'Request timed out. Please try again.';
        console.error('[Chat] Error type: AbortError (Timeout)');
      } else if (e.message?.includes('Failed to fetch')) {
        errorMessage = 'Network error. Check your connection.';
        console.error('[Chat] Error type: Network error (Failed to fetch)');
      } else if (e.message?.includes('HTTP error')) {
        errorMessage = `Server error: ${e.message}`;
        console.error(`[Chat] Error type: HTTP error (${e.message})`);
      }

      chatMessages = [...chatMessages, { from: 'bot', text: errorMessage }];
    } finally {
      isSending = false;
      await tick();
      if (chatInputElement) {
        chatInputElement.focus();
      }
      console.log('[Chat] sendMessage process finished.');
    }
  }

  function handleKeyDown(e: KeyboardEvent) {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      sendMessage();
    }
  }

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

      ctx.fillStyle = `rgba(255, 215, 215, ${0.6 * this.alpha})`;
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
    const pcbCleanup = setupPcbAnimation();

    const smoothScrollHandler = (event: MouseEvent) => {
      const target = event.target as HTMLElement;
      const link = target.closest('a[href^="/#"]');

      if (link) {
        const href = link.getAttribute('href');
        if (href) {
          event.preventDefault();

          // Special handling for home link
          if (href === '/#home') {
            window.scrollTo({
              top: 0,
              behavior: 'smooth'
            });
            return;
          }

          const selector = href.substring(1); // e.g., /#home -> #home
          const element = document.querySelector(selector) as HTMLElement;
          if (element) {
            const nav = document.querySelector('.main-nav') as HTMLElement;
            const navHeight = nav ? nav.offsetHeight : 0;
            const extraPadding = 20; // extra space below the nav bar

            const elementPosition = element.getBoundingClientRect().top;
            const offsetPosition = elementPosition + window.pageYOffset - navHeight - extraPadding;

            window.scrollTo({
              top: offsetPosition,
              behavior: 'smooth'
            });
          }
        }
      }
    };

    if (browser) {
      document.body.addEventListener('click', smoothScrollHandler);
    }

    return () => {
      if (pcbCleanup) pcbCleanup();
      if (browser) {
        document.body.removeEventListener('click', smoothScrollHandler);
      }
    };
  });

  function setupPcbAnimation() {
    const canvas = document.getElementById('pcb-canvas') as HTMLCanvasElement;
    if (!canvas) return;
    const ctx = canvas.getContext('2d');
    if (!ctx) return;

    let width: number, height: number, dpr: number;
    let particles: Particle[] = [];
    const numParticles = 40;
    const particleSpeed = 0.5;

    let circuitPaths: Path[] = [];
    let nodes = new Map<string, Point>();

    function getNodeKey(p: Point) {
      return `${p.x},${p.y}`;
    }

    function createCircuitLayout() {
      circuitPaths = [];
      nodes.clear();
      const gridSize = 60;
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
      ctx.strokeStyle = 'rgba(218, 165, 32, 0.2)';
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

  let chatBodyEl: HTMLDivElement;
  let shouldScroll = false;

  beforeUpdate(() => {
    if (!chatBodyEl) return;
    // We should scroll down only if the user is already at the bottom of the chat.
    // A little buffer is added (e.g. 5px) in case of rounding issues.
    shouldScroll = chatBodyEl.scrollHeight - chatBodyEl.clientHeight <= chatBodyEl.scrollTop + 5;
  });

  afterUpdate(() => {
    if (shouldScroll && chatBodyEl) {
      chatBodyEl.scrollTop = chatBodyEl.scrollHeight;
    }
  });
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
  </div>

  <!-- Navigation -->
  <nav class="main-nav">
    <div class="nav-container">
      <div class="nav-links">
        <a href="/" class="nav-link active">Home</a>
        <a href="/#demos" class="nav-link">Autonomous Demos</a>
        <a href="/#solutions" class="nav-link">Solutions</a>

        <a href="/#about" class="nav-link">About</a>
        <a href="/#contact" class="nav-link">Contact</a>
      </div>
      <a href="/contact" style="text-decoration: none;" class="nav-cta">Contact Us</a>
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
    </div>
  </footer>

  <!-- Chat Nub -->
  <div class="chat-nub-container">
    <button class="chat-nub-button" on:click={toggleChat}>
      <div class="chat-nub-gradient">
        <img src="/logoold.png" alt="Chat Agent" />
      </div>
    </button>
    {#if isChatOpen}
      <div class="chat-window glass-panel">
        <div class="chat-header">
          <h4 class="gold-gradient">SPARQ AI Assistant</h4>
          <button class="close-chat" on:click={toggleChat}>&times;</button>
        </div>
        <div class="chat-body" bind:this={chatBodyEl}>
          {#each chatMessages as msg}
            <div class="chat-message {msg.from}">
              {#if msg.from === 'bot'}
                <div class="message-content">
                  {@html formatChatMessage(msg.text)}
                </div>
              {:else}
                <span>{msg.text}</span>
              {/if}
            </div>
          {/each}
          {#if isSending}
            <div class="chat-message bot">
              <span>Typing...</span>
            </div>
          {/if}
        </div>
        <div class="chat-footer">
            <input
              type="text"
              placeholder="Type your message..."
              bind:value={chatInput}
              on:keydown={handleKeyDown}
              disabled={isSending}
              bind:this={chatInputElement}
            />
            <button on:click={sendMessage} disabled={isSending || !chatInput.trim()}>Send</button>
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
    color: #E0E0E0;
    overflow-x: hidden;
    line-height: 1.6;
  }

  .app-container {
    position: relative;
    min-height: 100vh;
    background: transparent;
    width: 100%;
    overflow-x: hidden;
  }

  .pcb-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    pointer-events: none;
    z-index: 0;
    overflow: hidden;
  }

  .app-container::before {
    content: '';
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 1;
    background: linear-gradient(to right, rgba(13,13,13,0) 0%, rgba(240, 184, 30, 0.1) 20%, rgba(240, 184, 30, 0.1) 80%, rgba(13,13,13,0) 100%);
  }

  .occlusion-vignette {
    /* Style removed for light theme */
  }

  #pcb-canvas {
    width: 100%;
    height: 100%;
    display: block;
    position: absolute;
    top: 0;
    left: 0;
  }

  /* Navigation */
  .main-nav {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 100;
    background: rgba(13, 13, 13, 0.7);
    backdrop-filter: blur(20px);
  }

  .nav-container {
    max-width: 1400px;
    margin: 0 auto;
    padding: 1rem 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 2px solid rgba(184, 134, 11, 0.1);
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
    color: #CCCCCC;
    text-decoration: none;
    font-weight: 500;
    transition: all 0.3s ease;
    position: relative;
  }

  .nav-link:hover,
  .nav-link.active {
    color: #B8860B;
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
    width: 100%;
    overflow-x: hidden;
  }

  /* Footer */
  .main-footer {
    background: rgba(13, 13, 13, 0.98);
    border-top: 1px solid rgba(184, 134, 11, 0.15);
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
    color: #CCCCCC;
    text-decoration: none;
    transition: color 0.3s ease;
  }

  .footer-section a:hover {
    color: #B8860B;
  }

  /* Global utility classes */
  :global(.gold-gradient) {
    background: linear-gradient(135deg, #B8860B 0%, #DAA520 25%, #CD853F 50%, #B8860B 75%, #8B7355 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  :global(.glass-panel) {
    background: rgba(20, 20, 20, 0.75);
    backdrop-filter: blur(20px);
    border: 1px solid rgba(184, 134, 11, 0.1);
    border-radius: 12px;
    box-shadow:
      0 8px 32px rgba(0, 0, 0, 0.1),
      inset 0 1px 0 rgba(218, 165, 32, 0.05);
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
    width: 100px;
    height: 100px;
    border-radius: 50%;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    border: none;
    transition: all 0.3s ease;
    flex-shrink: 0;
    position: relative;
    background: transparent;
  }

  .chat-nub-button::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    border-radius: 50%;
    background: linear-gradient(135deg, #B8860B, #DAA520);
    filter: blur(20px);
    opacity: 0;
    transition: opacity 0.3s ease;
    z-index: -1;
  }

  .chat-nub-button:hover {
    transform: scale(1.1);
  }

  .chat-nub-button:hover::before {
    opacity: 0.7;
  }

  .chat-nub-gradient {
    width: 90%;
    height: 90%;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    background: radial-gradient(circle, rgb(0, 0, 0) 15%, rgba(0, 0, 0, 0.85) 30%, rgba(255, 255, 255, 0)50%);
    animation: pulse-transform 2.5s infinite ease-in-out;
  }

  .chat-nub-button img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }

  @keyframes pulse-transform {
    0% { transform: scale(1); }
    50% { transform: scale(1.1); }
    100% { transform: scale(1); }
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
    background: rgba(25, 25, 25, 0.95);
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
    background: rgba(13, 13, 13, 0.9);
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
    background: rgba(18, 18, 18, 0.9);
    color: #E0E0E0;
    overflow-y: auto;
  }

  /* Custom scrollbar styling */
  .chat-body::-webkit-scrollbar {
    width: 6px;
  }

  .chat-body::-webkit-scrollbar-track {
    background: rgba(13, 13, 13, 0.3);
    border-radius: 3px;
  }

  .chat-body::-webkit-scrollbar-thumb {
    background: rgba(218, 165, 32, 0.4);
    border-radius: 3px;
    transition: background 0.3s ease;
  }

  .chat-body::-webkit-scrollbar-thumb:hover {
    background: rgba(218, 165, 32, 0.6);
  }

  /* Firefox scrollbar */
  .chat-body {
    scrollbar-width: thin;
    scrollbar-color: rgba(218, 165, 32, 0.4) rgba(13, 13, 13, 0.3);
  }

  .chat-footer {
    display: flex;
    padding: 0.75rem;
    border-top: 1px solid rgba(184, 134, 11, 0.25);
    background: rgba(13, 13, 13, 0.9);
    flex-shrink: 0;
  }

  .chat-footer input {
    flex-grow: 1;
    background: rgba(30, 30, 30, 0.95);
    border: 1px solid rgba(184, 134, 11, 0.25);
    border-radius: 6px;
    padding: 0.5rem;
    color: #E0E0E0;
    font-family: 'Orbitron', sans-serif;
    font-weight: bold;
  }

  .chat-footer input::placeholder {
    color: #777;
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

  .chat-footer button:disabled {
    background: #666;
    cursor: not-allowed;
  }

  .chat-message {
    margin-bottom: 0.75rem;
    padding: 0.5rem 0.75rem;
    border-radius: 8px;
    max-width: 85%;
    word-wrap: break-word;
  }

  .chat-message.user {
    background: rgba(218, 165, 32, 0.1);
    border: 1px solid rgba(218, 165, 32, 0.2);
    margin-left: auto;
    text-align: right;
  }

  .chat-message.bot {
    background: rgba(40, 40, 40, 0.9);
    border: 1px solid rgba(184, 134, 11, 0.1);
    margin-right: auto;
  }

  .chat-message span {
    color: #E0E0E0;
    font-size: 1rem;
    line-height: 1.6;
    font-weight: bold;
  }

  /* Simple chat formatting styles */
  .message-content {
    color: #E0E0E0;
    font-size: 1rem;
    line-height: 1.6;
    font-weight: bold;
  }

  .time-slot {
    background: rgba(218, 165, 32, 0.2);
    color: #DAA520;
    padding: 0.15rem 0.3rem;
    border-radius: 3px;
    font-weight: 600;
    font-size: 0.95rem;
  }

  .date-highlight {
    background: rgba(72, 187, 120, 0.2);
    color: #48BB78;
    padding: 0.15rem 0.3rem;
    border-radius: 3px;
    font-weight: 600;
    font-size: 0.95rem;
  }

  .timezone {
    background: rgba(159, 122, 234, 0.2);
    color: #9F7AEA;
    padding: 0.15rem 0.3rem;
    border-radius: 3px;
    font-weight: 500;
    font-size: 0.9rem;
  }

  .confirmation {
    background: rgba(72, 187, 120, 0.2);
    color: #48BB78;
    padding: 0.2rem 0.4rem;
    border-radius: 4px;
    font-weight: 600;
  }

  .available-times {
    margin: 0.5rem 0;
    padding: 0.5rem;
    background: rgba(218, 165, 32, 0.1);
    border-left: 3px solid #DAA520;
    border-radius: 4px;
    font-weight: 600;
    color: #DAA520;
    line-height: 1.4;
  }

  /* n8n chat integration styles */
  :global(#n8n-chat) {
    position: absolute !important;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  :global(.n8n-chat-footer) {
    display: none !important;
  }

  :global(.n8n-chat-header) {
    display: none !important;
  }

  :global(.n8n-chat-messages) {
    background: transparent !important;
  }

  :global(.n8n-chat-input-container) {
    background: transparent !important;
  }

  :global(.n8n-chat-message-bot .n8n-chat-message-text) {
    background: rgba(245, 245, 245, 0.9) !important;
    border: 1px solid rgba(184, 134, 11, 0.1) !important;
  }

  :global(.n8n-chat-message-user .n8n-chat-message-text) {
    background: rgba(218, 165, 32, 0.1) !important;
    border: 1px solid rgba(218, 165, 32, 0.2) !important;
  }

  /* Responsive design */
  @media (max-width: 768px) {
    .nav-container {
      padding: 0.75rem;
      width: 100%;
      overflow-x: hidden;
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
      padding: 0 1rem;
    }

    .chat-nub-container {
      bottom: 0.75rem;
      right: 0.75rem;
    }

    .chat-nub-button {
      width: 60px;
      height: 60px;
    }

    .chat-window {
      width: calc(100vw - 2rem);
      right: 0;
      bottom: calc(100% + 0.5rem);
    }

    .pcb-background {
      width: 100%;
      height: 100%;
    }

    #pcb-canvas {
      transform: scale(0.8);
      transform-origin: center center;
    }
  }
</style>
