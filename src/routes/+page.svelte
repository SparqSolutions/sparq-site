<script lang="ts">
  import { onMount } from 'svelte';
  
  let currentDemo = 0;
  let demoInterval: NodeJS.Timeout;
  
  const aiDemos = [
    {
      title: "Social Media Outreach",
      description: "AI agents that automate social media engagement, schedule posts, and analyze audience sentiment.",
      features: ["Automated Posting", "Sentiment Analysis", "Engagement Tracking"],
      status: "Live Demo"
    },
    {
      title: "AI Content Generation",
      description: "Generate high-quality blog posts, articles, and marketing copy tailored to your brand's voice.",
      features: ["SEO Optimization", "Multiple Formats", "Brand Voice Matching"],
      status: "Interactive Demo"
    },
    {
      title: "Webpage Chat Agents",
      description: "Intelligent chat agents to handle customer inquiries, schedule appointments, and capture leads 24/7.",
      features: ["Lead Capture", "Appointment Scheduling", "Instant Responses"],
      status: "Simulation Ready"
    }
  ];
  
  onMount(() => {
    // Auto-rotate demos
    demoInterval = setInterval(() => {
      currentDemo = (currentDemo + 1) % aiDemos.length;
    }, 5000);
    
    return () => clearInterval(demoInterval);
  });
  
  function selectDemo(index: number) {
    currentDemo = index;
    clearInterval(demoInterval);
    // Restart auto-rotation after manual selection
    setTimeout(() => {
      demoInterval = setInterval(() => {
        currentDemo = (currentDemo + 1) % aiDemos.length;
      }, 5000);
    }, 10000);
  }
</script>

<svelte:head>
  <title>SPARQ - AI for Small & Medium Businesses</title>
  <meta name="description" content="AI-powered solutions for small and medium businesses. We build AI agents for social media, content creation, and customer engagement." />
</svelte:head>

<div class="homepage">
  <!-- Hero Section -->
  <section class="hero-section">
    <div class="hero-container">
      <div class="hero-content">
        <div class="hero-text">
          <div class="hero-title-visual">
            <img src="/logoold.png" alt="SPARQ Logo" class="hero-title-logo-image" />
            <img src="/sparqname.png" alt="SPARQ" class="hero-title-name-image" />
          </div>
          <p class="hero-tagline">Specialized Agentic Business Solutions</p>
          <p class="hero-description">
            We build and deploy AI agents to handle your social media, generate content, and engage with your customers. 
            Our solutions are designed to grow your business and streamline your operations.
          </p>
          
          <div class="hero-stats">
            <div class="stat-item spark-target">
              <span class="stat-number gold-gradient">100+</span>
              <span class="stat-label">SMBs Empowered</span>
            </div>
            <div class="stat-item spark-target">
              <span class="stat-number gold-gradient">24/7</span>
              <span class="stat-label">Agent Availability</span>
            </div>
            <div class="stat-item spark-target">
              <span class="stat-number gold-gradient">500+</span>
              <span class="stat-label">Active AI Agents</span>
            </div>
          </div>
          
          <div class="hero-actions">
            <button class="cta-primary glass-panel interactive-card">
              <span>Explore AI Demos</span>
            </button>
            <button class="cta-secondary interactive-card">
              <span>Schedule Consultation</span>
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
  
  <!-- AI Demos Section -->
  <section class="demos-section">
    <div class="section-container">
      <h2 class="section-title gold-gradient">Our AI Solutions in Action</h2>
      <p class="section-subtitle">
        See how our AI agents can work for your business. Each demo highlights a key area where we can help you grow.
      </p>
      
      <div class="demos-interface">
        <div class="demo-selector">
          {#each aiDemos as demo, index}
            <button 
              class="demo-tab {currentDemo === index ? 'active' : ''} glass-panel interactive-card spark-target"
              on:click={() => selectDemo(index)}
            >
              <span class="demo-tab-title">{demo.title}</span>
              <span class="demo-status {demo.status.toLowerCase().replace(' ', '-')}">{demo.status}</span>
            </button>
          {/each}
        </div>
        
        <div class="demo-display glass-panel">
          <div class="demo-header">
            <h3 class="demo-title gold-gradient">{aiDemos[currentDemo].title}</h3>
            <div class="demo-indicators">
              <span class="indicator active spark-target">Neural Processing</span>
              <span class="indicator active spark-target">Real-time Analysis</span>
              <span class="indicator active spark-target">Enterprise Ready</span>
            </div>
          </div>
          
          <div class="demo-content">
            <div class="demo-visual">
              <div class="neural-network-viz">
                <div class="network-layer input-layer">
                  <div class="neuron spark-target"></div>
                  <div class="neuron spark-target"></div>
                  <div class="neuron spark-target"></div>
                  <div class="neuron spark-target"></div>
                </div>
                <div class="network-layer hidden-layer">
                  <div class="neuron spark-target"></div>
                  <div class="neuron spark-target"></div>
                  <div class="neuron spark-target"></div>
                  <div class="neuron spark-target"></div>
                  <div class="neuron spark-target"></div>
                  <div class="neuron spark-target"></div>
                </div>
                <div class="network-layer output-layer">
                  <div class="neuron spark-target"></div>
                  <div class="neuron spark-target"></div>
                  <div class="neuron spark-target"></div>
                </div>
              </div>
            </div>
            
            <div class="demo-info">
              <p class="demo-description">{aiDemos[currentDemo].description}</p>
              <div class="demo-features">
                <h4>Key Capabilities:</h4>
                <ul>
                  {#each aiDemos[currentDemo].features as feature}
                    <li class="spark-target">{feature}</li>
                  {/each}
                </ul>
              </div>
              <button class="demo-cta glass-panel interactive-card">
                <span class="gold-gradient">Launch Interactive Demo</span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
  
  <!-- Solutions Overview -->
  <section class="solutions-section">
    <div class="section-container">
      <h2 class="section-title gold-gradient">Solutions for Your Business</h2>
      <p class="section-subtitle">
        We offer a range of AI services to meet the needs of small and medium-sized businesses.
      </p>
      
      <div class="solutions-grid">
        <div class="solution-card glass-panel interactive-card">
          <div class="solution-icon spark-target">
            <div class="icon-automation"></div>
          </div>
          <h3 class="gold-gradient">Social Media Engagement</h3>
          <p>Our AI agents manage your social media, engaging with your audience and growing your online presence.</p>
          <div class="solution-features">
            <span class="feature-tag">Outreach</span>
            <span class="feature-tag">Engagement</span>
            <span class="feature-tag">Analytics</span>
          </div>
        </div>
        
        <div class="solution-card glass-panel interactive-card">
          <div class="solution-icon spark-target">
            <div class="icon-nlp"></div>
          </div>
          <h3 class="gold-gradient">AI Content Creation</h3>
          <p>Generate SEO-friendly articles, blog posts, and marketing copy that resonates with your target audience.</p>
          <div class="solution-features">
            <span class="feature-tag">Content Strategy</span>
            <span class="feature-tag">SEO</span>
            <span class="feature-tag">Publishing</span>
          </div>
        </div>
        
        <div class="solution-card glass-panel interactive-card">
          <div class="solution-icon spark-target">
            <div class="icon-analytics"></div>
          </div>
          <h3 class="gold-gradient">Sentiment Analysis</h3>
          <p>Understand your customers better by analyzing feedback from social media, reviews, and support channels.</p>
          <div class="solution-features">
            <span class="feature-tag">Customer Feedback</span>
            <span class="feature-tag">Brand Monitoring</span>
            <span class="feature-tag">Market Trends</span>
          </div>
        </div>
        
        <div class="solution-card glass-panel interactive-card">
          <div class="solution-icon spark-target">
            <div class="icon-neural"></div>
          </div>
          <h3 class="gold-gradient">Webpage Chat Agents</h3>
          <p>Deploy intelligent chat agents on your website to schedule appointments, answer questions, and generate leads.</p>
          <div class="solution-features">
            <span class="feature-tag">Scheduling</span>
            <span class="feature-tag">Lead Generation</span>
            <span class="feature-tag">Customer Support</span>
          </div>
        </div>
      </div>
    </div>
  </section>
  
  <!-- Technical Excellence -->
  <section class="tech-section">
    <div class="section-container">
      <h2 class="section-title gold-gradient">Why Choose SPARQ?</h2>
      <div class="tech-grid">
        <div class="tech-column">
          <div class="tech-card glass-panel interactive-card">
            <h3 class="gold-gradient">Built for Small Business</h3>
            <p>We focus on affordable, high-impact AI solutions that are tailored to the unique needs of small and medium-sized businesses.</p>
          </div>
          <div class="tech-card glass-panel interactive-card">
            <h3 class="gold-gradient">Fast Deployment</h3>
            <p>Our pre-built AI models and streamlined processes mean we can get your AI agents up and running in record time.</p>
          </div>
        </div>
        <div class="tech-column">
          <div class="tech-card glass-panel interactive-card">
            <h3 class="gold-gradient">Scalable & Flexible</h3>
            <p>Start with one solution and add more as your business grows. Our platform is designed to scale with you.</p>
          </div>
          <div class="tech-card glass-panel interactive-card">
            <h3 class="gold-gradient">Dedicated Support</h3>
            <p>We provide ongoing support and maintenance to ensure your AI solutions continue to deliver value.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- About Us Section -->
  <section class="about-section">
    <div class="section-container">
      <div class="about-content">
        <div class="about-text">
          <h2 class="section-title gold-gradient">Your Partner in AI Growth</h2>
          <p>
            SPARQ was founded with a simple mission: to make the power of artificial intelligence accessible to small and medium-sized businesses. We believe that AI can be a powerful tool for growth, and we're dedicated to helping our clients succeed.
          </p>
          <p>
            Our team of experts has a deep understanding of both AI technology and the challenges that small businesses face. We work closely with you to understand your goals and develop a customized AI strategy that delivers real results.
          </p>
          <button class="cta-secondary interactive-card">
            <span>Learn More About Our Mission</span>
          </button>
        </div>
        <div class="about-visual glass-panel interactive-card">
          <img src="/team-photo.jpg" alt="The SPARQ Team" class="about-image" />
          <div class="about-image-overlay">
            <p>"Empowering businesses with intelligent, accessible AI."</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Call to Action -->
  <section class="cta-section">
    <div class="section-container">
      <div class="cta-content glass-panel">
        <div class="cta-text">
          <h2 class="section-title gold-gradient">Ready to Grow Your Business?</h2>
          <p>
            Let's talk about how our AI agents can help you achieve your goals. Schedule a free consultation with one of our AI specialists today.
          </p>
        </div>
        <div class="cta-action">
          <button class="cta-primary glass-panel interactive-card">
            <span class="gold-gradient">Schedule Free Consultation</span>
          </button>
        </div>
      </div>
    </div>
  </section>
</div>

<style>
  .homepage {
    display: flex;
    flex-direction: column;
    gap: 6rem;
    padding-bottom: 4rem;
  }

  .section-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 2rem;
  }

  .gold-gradient {
    background: linear-gradient(135deg, #D4AF37 0%, #B48811 25%, #E2B638 50%, #C49A28 75%, #AD8A2C 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  /* Glass Panel Effect */
  .glass-panel {
    background: rgba(22, 28, 45, 0.6);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 215, 0, 0.1);
    border-radius: 1rem;
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease;
  }

  .interactive-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 40px 0 rgba(255, 215, 0, 0.15);
    border-color: rgba(255, 215, 0, 0.3);
  }

  /* Hero Section */
  .hero-section {
    padding: 6rem 2rem 4rem;
    text-align: center;
  }

  .hero-container {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 2rem;
  }

  .hero-content {
    display: flex;
    align-items: center;
    gap: 3rem;
    text-align: left;
    justify-content: center;
  }

  .hero-text {
    max-width: 600px;
  }

  .hero-title-visual {
    display: flex;
    align-items: center;
    gap: 1.5rem;
    margin-bottom: 1.5rem;
  }

  .hero-title-logo-image {
    width: 100px;
    height: auto;
    margin-bottom: 8px;
  }

  .hero-title-name-image {
    width: 280px;
    height: auto;
  }

  .hero-tagline {
    font-size: 1.5rem;
    font-weight: 600;
    color: #ffd700;
    margin-bottom: 1.5rem;
  }

  .hero-description {
    font-size: 1.1rem;
    color: #c0c0c0;
    margin-bottom: 2rem;
    line-height: 1.6;
  }

  .hero-stats {
    display: flex;
    gap: 2rem;
    margin-bottom: 2.5rem;
  }

  .stat-item {
    padding: 1rem;
    border-radius: 0.5rem;
    text-align: center;
  }

  .stat-number {
    display: block;
    font-size: 2rem;
    font-weight: 700;
  }

  .stat-label {
    font-size: 0.9rem;
    color: #c0c0c0;
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .hero-actions {
    display: flex;
    gap: 1.5rem;
  }

  .cta-primary, .cta-secondary {
    padding: 1rem 2rem;
    font-size: 1rem;
    font-weight: 600;
    border-radius: 0.5rem;
    cursor: pointer;
    border: none;
  }

  .cta-primary {
    background: linear-gradient(135deg, #D4AF37 0%, #B48811 100%);
    color: #0D0D0D;
    border: none;
    padding: 1rem 2rem;
    border-radius: 8px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.3s ease;
    font-size: 1.1rem;
    font-family: 'Orbitron', sans-serif;
  }

  .cta-primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 10px 30px rgba(212, 175, 55, 0.4);
  }

  .cta-secondary {
    background: transparent;
    color: #ffd700;
    border: 1px solid #ffd700;
  }

  /* Section Styles */
  .section-title {
    font-size: 2.5rem;
    font-weight: 700;
    text-align: center;
    margin-bottom: 1rem;
  }

  .section-subtitle {
    font-size: 1.2rem;
    color: #c0c0c0;
    text-align: center;
    max-width: 700px;
    margin: 0 auto 3rem auto;
  }

  /* Demos Section */
  .demos-interface {
    display: grid;
    grid-template-columns: 300px 1fr;
    gap: 2rem;
    align-items: flex-start;
  }

  .demo-selector {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .demo-tab {
    width: 100%;
    padding: 1.2rem;
    text-align: left;
    background: rgba(22, 28, 45, 0.4);
    border: 1px solid rgba(255, 215, 0, 0.1);
    color: #fff;
    cursor: pointer;
  }

  .demo-tab.active {
    background: rgba(255, 215, 0, 0.1);
    border-left: 4px solid #ffd700;
  }

  .demo-tab-title {
    display: block;
    font-size: 1.1rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
  }

  .demo-status {
    font-size: 0.8rem;
    padding: 0.2rem 0.5rem;
    border-radius: 0.3rem;
    background: #444;
    color: #fff;
  }
  .demo-status.live-demo { background: #28a745; }
  .demo-status.interactive-demo { background: #17a2b8; }
  .demo-status.simulation-ready { background: #ffc107; color: #111; }

  .demo-display {
    padding: 2rem;
    min-height: 400px;
  }

  .demo-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.5rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid rgba(255, 215, 0, 0.1);
  }

  .demo-title {
    font-size: 1.8rem;
  }

  .demo-indicators {
    display: flex;
    gap: 0.5rem;
  }

  .indicator {
    padding: 0.3rem 0.6rem;
    border-radius: 0.3rem;
    font-size: 0.8rem;
    background: rgba(255, 215, 0, 0.1);
    color: #ffd700;
  }

  .demo-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
    align-items: center;
  }

  .neural-network-viz {
    display: flex;
    align-items: center;
    justify-content: space-around;
    height: 200px;
    position: relative;
  }

  .network-layer {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
  .neuron {
    width: 20px;
    height: 20px;
    background: #ffd700;
    border-radius: 50%;
    box-shadow: 0 0 10px #ffd700;
    animation: pulse 2s infinite ease-in-out;
  }
  @keyframes pulse {
    0%, 100% { transform: scale(1); opacity: 1; }
    50% { transform: scale(1.2); opacity: 0.7; }
  }
  .hidden-layer .neuron { animation-delay: 0.5s; }
  .output-layer .neuron { animation-delay: 1s; }


  .demo-info {
    text-align: left;
  }
  .demo-description {
    margin-bottom: 1.5rem;
    line-height: 1.6;
    color: #c0c0c0;
  }
  .demo-features h4 {
    font-size: 1.1rem;
    font-weight: 600;
    color: #fff;
    margin-bottom: 0.8rem;
  }
  .demo-features ul {
    list-style: none;
    padding: 0;
    margin: 0 0 1.5rem 0;
  }
  .demo-features li {
    padding-left: 1.5rem;
    position: relative;
    margin-bottom: 0.5rem;
    color: #c0c0c0;
  }
  .demo-features li::before {
    content: '✓';
    position: absolute;
    left: 0;
    color: #ffd700;
  }

  .demo-cta {
    padding: 0.8rem 1.5rem;
    font-size: 0.9rem;
    font-weight: 600;
  }

  /* Solutions Section */
  .solutions-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1.5rem;
  }

  .solution-card {
    padding: 2rem;
    text-align: center;
  }
  .solution-icon {
    width: 60px;
    height: 60px;
    margin: 0 auto 1.5rem auto;
    border-radius: 50%;
    background: rgba(255, 215, 0, 0.1);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .icon-neural, .icon-automation, .icon-analytics, .icon-nlp {
    width: 30px;
    height: 30px;
    background-color: #ffd700;
  }
  /* This part needs actual icons, for now just placeholders */
  .icon-neural { mask: url('data:image/svg+xml,...') no-repeat center; }

  .solution-card h3 {
    font-size: 1.3rem;
    margin-bottom: 1rem;
  }

  .solution-card p {
    color: #c0c0c0;
    line-height: 1.5;
    margin-bottom: 1.5rem;
  }

  .solution-features {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 0.5rem;
  }
  .feature-tag {
    background: #333;
    color: #c0c0c0;
    font-size: 0.8rem;
    padding: 0.3rem 0.6rem;
    border-radius: 0.3rem;
  }

  /* Tech Section */
  .tech-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
  }
  .tech-card {
    padding: 2rem;
    margin-bottom: 2rem;
  }
  .tech-card h3 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }
  .tech-card p {
    color: #c0c0c0;
    line-height: 1.6;
  }


  /* About Us Section */
  .about-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 3rem;
    align-items: center;
  }

  .about-text p {
    color: #c0c0c0;
    line-height: 1.7;
    margin-bottom: 1.5rem;
  }

  .about-visual {
    position: relative;
    overflow: hidden;
  }

  .about-image {
    width: 100%;
    height: auto;
    display: block;
  }

  .about-image-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0,0,0,0.7);
    padding: 1rem;
    text-align: center;
    color: #fff;
    font-style: italic;
  }

  /* CTA Section */
  .cta-content {
    padding: 3rem 2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 2rem;
  }
  .cta-text {
    max-width: 600px;
  }
  .cta-text .section-title {
    text-align: left;
    margin-bottom: 1rem;
  }
  .cta-text p {
    color: #c0c0c0;
    font-size: 1.1rem;
    line-height: 1.6;
  }
  .cta-action .cta-primary {
    padding: 1.2rem 2.5rem;
    font-size: 1.1rem;
  }

</style>
