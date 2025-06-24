<script lang="ts">
  import { onMount } from 'svelte';
  import { fade } from 'svelte/transition';
  
  let currentDemo = 0;
  let demoInterval: NodeJS.Timeout;
  
  const aiDemos = [
    {
      title: "Social Media Outreach",
      description: "AI agents that automate social media engagement, schedule posts, and analyze audience sentiment.",
      features: ["Automated Posting", "Sentiment Analysis", "Engagement Tracking"]
    },
    {
      title: "AI Content Generation",
      description: "Generate high-quality blog posts, articles, and marketing copy tailored to your brand's voice.",
      features: ["SEO Optimization", "Multiple Formats", "Brand Voice Matching"]
    },
    {
      title: "Webpage Chat Agents",
      description: "Intelligent chat agents to handle customer inquiries, schedule appointments, and capture leads 24/7.",
      features: ["Lead Capture", "Appointment Scheduling", "Instant Responses"]
    }
  ];
  
  onMount(() => {
    // Auto-rotate demos
    demoInterval = setInterval(() => {
      currentDemo = (currentDemo + 1) % aiDemos.length;
    }, 5000);
    
    return () => clearInterval(demoInterval);
  });
</script>

<svelte:head>
  <title>SPARQ - AI for Small & Medium Businesses</title>
  <meta name="description" content="AI-powered solutions for small and medium businesses. We build AI agents for social media, content creation, and customer engagement." />
</svelte:head>

<div class="homepage">
  <!-- Hero Section -->
  <section class="hero-section" id="home">
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
              <span class="stat-number gold-gradient">3</span>
              <span class="stat-label">Active Deployments</span>
            </div>
            <div class="stat-item spark-target">
              <span class="stat-number gold-gradient">24/7</span>
              <span class="stat-label">Agent Availability</span>
            </div>
            
          </div>
          
          <div class="hero-actions">
            <a href="/#demos" class="cta-primary glass-panel interactive-card">
              <span>Explore AI Demos</span>
            </a>
            <a href="/#contact" class="cta-secondary interactive-card">
              <span>Schedule Consultation</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
  
  <!-- AI Demos Section -->
  <section class="demos-section" id="demos">
    <div class="section-container">
      <h2 class="section-title gold-gradient">Infinite possibilities with Agentic Deployment</h2>
      <p class="section-subtitle">
        See how our AI agents can work for your business. Automation in key areas help you grow.
      </p>
      
      <div class="rotating-demo-window glass-panel">
        {#key currentDemo}
          <div class="demo-card" in:fade={{ duration: 500, delay: 200 }} out:fade={{ duration: 300 }}>
            
            <div class="demo-header">
              <h3 class="demo-title gold-gradient">{aiDemos[currentDemo].title}</h3>
            </div>
            
            <p class="demo-description">{aiDemos[currentDemo].description}</p>
            
            <div class="demo-features">
              <ul>
                {#each aiDemos[currentDemo].features as feature}
                  <li class="spark-target">{feature}</li>
                {/each}
              </ul>
            </div>

            <div class="demo-visual-background">
              <div class="neural-network-viz">
                <div class="network-layer input-layer"><div class="neuron spark-target"></div><div class="neuron spark-target"></div><div class="neuron spark-target"></div><div class="neuron spark-target"></div></div>
                <div class="network-layer hidden-layer"><div class="neuron spark-target"></div><div class="neuron spark-target"></div><div class="neuron spark-target"></div><div class="neuron spark-target"></div><div class="neuron spark-target"></div><div class="neuron spark-target"></div></div>
                <div class="network-layer output-layer"><div class="neuron spark-target"></div><div class="neuron spark-target"></div><div class="neuron spark-target"></div></div>
              </div>
            </div>

          </div>
        {/key}
      </div>
    </div>
  </section>
  
  <!-- Solutions Overview -->
  <section class="solutions-section" id="solutions">
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
  <section class="about-section" id="about">
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
          <a href="/#about" class="cta-secondary interactive-card">
            <span>Learn More About Our Mission</span>
          </a>
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
  <section class="cta-section" id="contact">
    <div class="section-container">
      <div class="cta-content glass-panel">
        <div class="cta-text">
          <h2 class="section-title gold-gradient">Ready to Grow Your Business?</h2>
          <p>
            Let's talk about how our AI agents can help you achieve your goals. Schedule a free consultation with one of our AI specialists today.
          </p>
        </div>
        <div class="cta-action">
          <a href="/schedule" class="cta-primary glass-panel interactive-card">
            <span>Schedule Free Consultation</span>
          </a>
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
    box-shadow: 0 0px 80px 0 rgba(255, 215, 0, 0.5);
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
    text-align: center;
    justify-content: center;
  }

  .hero-text {
    max-width: 600px;
  }

  .hero-title-visual {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1.5rem;
    margin-bottom: 1.5rem;
  }

  .hero-title-logo-image {
    width: 120px;
    height: auto;
    margin-bottom: 8px;
  }

  .hero-title-name-image {
    width: 320px;
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
    justify-content: center;
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
    justify-content: center;
    gap: 1.5rem;
  }

  .cta-primary, .cta-secondary {
    padding: 1rem 2rem;
    font-size: 1rem;
    font-weight: 600;
    border-radius: 0.5rem;
    cursor: pointer;
    border: none;
    text-decoration: none;
    display: inline-block;
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
  .rotating-demo-window {
    padding: 2.5rem;
    position: relative;
    overflow: hidden;
    min-height: 350px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .demo-card {
    text-align: center;
    position: absolute;
    z-index: 2;
  }
  
  .demo-header {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 1.5rem;
    margin-bottom: 2rem;
  }
  
  .demo-title {
    font-size: 2rem;
  }

  .demo-description {
    max-width: 600px;
    margin: 0 auto 2rem auto;
    font-size: 1.1rem;
    color: #c0c0c0;
    line-height: 1.6;
  }

  .demo-features ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 1rem;
  }

  .demo-features li {
    padding: 0.6rem 1.2rem;
    position: relative;
    color: #ffd700;
    background: rgba(255, 215, 0, 0.05);
    border: 1px solid rgba(255, 215, 0, 0.15);
    border-radius: 8px;
    font-weight: 600;
  }

  .demo-visual-background {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) scale(1.5);
    width: 100%;
    height: 100%;
    z-index: -1;
    opacity: 0.08;
  }

  .neural-network-viz {
    display: flex;
    align-items: center;
    justify-content: space-around;
    height: 100%;
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
