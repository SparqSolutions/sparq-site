<script lang="ts">
  import { onMount } from 'svelte';
  import { fade } from 'svelte/transition';
  
  let currentDemo = 0;
  let demoInterval: ReturnType<typeof setInterval>;
  
  const aiDemos = [
    {
      title: "Social Media Outreach",
      description: "Automated solutions for social media engagement, posting, and sentiment analysis.",
      features: ["Automated Posting", "Sentiment Analysis", "Engagement Tracking"]
    },
    {
      title: "Automated Content Generation",
      description: "Generate high-quality blog posts, articles, and marketing copy tailored to your brand's voice.",
      features: ["SEO Optimization", "Multiple Formats", "Brand Voice Matching"]
    },
    {
      title: "Webpage Engagement Automation",
      description: "Reactive Automation Solutions to handle customer inquiries, schedule appointments, and capture leads 24/7.",
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
  <title>SPARQ - Custom Autonomous Solutions</title>
  <meta name="description" content="Automated solutions for your business. We specialize in automating social media interactions, content creation, and customer engagement." />
</svelte:head>

<div class="homepage">
  <!-- Hero Section -->
  <section class="hero-section" id="home">
    <div class="hero-container">
      <div class="hero-content">
        <div class="hero-text">
          <div class="hero-title-visual">
            <img src="/logoFade.png" alt="SPARQ Logo" class="hero-logo-image" />
          </div>
          <p class="hero-tagline">Specialized Autonomous Business Solutions</p>
          <p class="hero-description">
            We design and deploy AI solutions that solve real business challenges by automating workflows, accelerating growth, and enhancing customer experiences. From social media and content automation to complex operational systems, everything we customize is engineered to enhance your business.
          </p>
          
          <!--<div class="hero-stats">
            <div class="stat-item spark-target">
              <span class="stat-number gold-gradient">3</span>
              <span class="stat-label">Active Deployments</span>
            </div>
            <div class="stat-item spark-target">
              <span class="stat-number gold-gradient">24/7</span>
              <span class="stat-label">Agent Availability</span>
            </div>

          </div>-->
          
          <div class="hero-actions">
             <!-- <a href="/#demos" class="cta-primary glass-panel interactive-card">
              <span>Explore AI Demos</span>
            </a>-->
            <a href="/schedule" class="cta-secondary interactive-card">
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
      <h2 class="section-title gold-gradient">Infinite Potential with Autonomous AI Deployments
      </h2>
      <p class="section-subtitle">
        Automate your business with smart AI systems that run 24/7. Boost efficiency, engage customers, and drive growth with no extra hands needed.
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
        We offer a range of autonomous solutions to meet the needs of businesses of all sizes. From small to large, we’ve got you covered.
      </p>
      
      <div class="solutions-grid">
        <div class="solution-card glass-panel interactive-card">
          <div class="solution-icon spark-target">
            <img src="/icon-automation.svg" alt="Social Media Automation Icon" />
          </div>
          <h3 class="gold-gradient">Social Media Engagement</h3>
          <p class="solution-description">Our automated deployments manage your social media, engaging with your audience and growing your online presence.</p>
          <div class="solution-features">
            <span class="feature-tag">Outreach</span>
            <span class="feature-tag">Engagement</span>
            <span class="feature-tag">Analytics</span>
          </div>
        </div>
        
        <div class="solution-card glass-panel interactive-card">
          <div class="solution-icon spark-target">
            <img src="/icon-nlp.svg" alt="AI Content Creation Icon" />
          </div>
          <h3 class="gold-gradient">Autonomous Content Creation</h3>
          <p class="solution-description">Leverage emerging trends. Generate and strategically deploy promotional materials that will drive engagment and create leads.</p>
          <div class="solution-features">
            <span class="feature-tag">Content Strategy</span>
            <span class="feature-tag">SEO</span>
            <span class="feature-tag">Publishing</span>
          </div>
        </div>
        
        <div class="solution-card glass-panel interactive-card">
          <div class="solution-icon spark-target">
            <img src="/icon-analytics.svg" alt="Sentiment Analysis Icon" />
          </div>
          <h3 class="gold-gradient">Sentiment Analysis</h3>
          <p class="solution-description">Understand your customers better by analyzing feedback from social media, reviews, and support channels.</p>
          <div class="solution-features">
            <span class="feature-tag">Customer Feedback</span>
            <span class="feature-tag">Brand Monitoring</span>
            <span class="feature-tag">Market Trends</span>
          </div>
        </div>
        
        <div class="solution-card glass-panel interactive-card">
          <div class="solution-icon spark-target">
            <img src="/icon-neural.svg" alt="Webpage Chat Agents Icon" />
          </div>
          <h3 class="gold-gradient">Webpage Engagement Automation</h3>
          <p class="solution-description">Deploy intelligent automation on your website to schedule appointments, answer questions, and generate leads.</p>
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
            <h3 class="gold-gradient">Built for Business</h3>
            <p class="solution-description">We focus on affordable, high-impact autonomous solutions that are tailored to the unique needs of small and medium-sized businesses.</p>
          </div>
          <div class="tech-card glass-panel interactive-card">
            <h3 class="gold-gradient">Fast Deployment</h3>
            <p class="solution-description">Our automated, streamlined processes mean we can get autonomous solutions up and running in record time.</p>
          </div>
        </div>
        <div class="tech-column">
          <div class="tech-card glass-panel interactive-card">
            <h3 class="gold-gradient">Scalable & Flexible</h3>
            <p class="solution-description">Start with one solution and add more as your business grows. Our platform is designed to scale with you.</p>
          </div>
          <div class="tech-card glass-panel interactive-card">
            <h3 class="gold-gradient">Dedicated Support</h3>
            <p class="solution-description">We provide ongoing support and maintenance to ensure autonomous solutions continue to deliver value.</p>
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
          <h2 class="section-title gold-gradient">Your Partner in Automated Growth</h2>
          <p style="font-size: 1.2rem;" class="solution-description">
            SPARQ was founded with a simple mission: to make the power of automation accessible to any size business. We believe that autonomous solutions are an incredibly powerful tool for growth, and we're dedicated to helping our clients succeed.
          </p>
          <p style="font-size: 1.2rem;" class="solution-description">
            Our team of experts has a deep understanding of automation technology and decades of business experience. We work closely with you to understand your goals and develop a customized autonomous strategies that delivers real results.
          </p>
          <!--<a href="/#about" class="cta-secondary interactive-card">
            <span>Learn More About Our Mission</span>
          </a>-->
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
          <p class="solution-description">
            Let's talk about how our solutions will help you achieve your goals. Schedule a free consultation with our specialists today.
          </p>
        </div>
        <div class="cta-action">
          <a href="/schedule" class="cta-primary glass-panel interactive-card">
            <span>Schedule Consultation</span>
          </a>
          <a href="/contact" class="cta-secondary interactive-card">
            <span>Send a Message</span>
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
    background: rgba(20, 20, 20, 0.6);
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
    padding: 0 1rem 1rem;
    text-align: center;
  }

  .hero-container {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 4rem;
  }

  .hero-content {
    display: flex;
    align-items: center;
    gap: 3rem;
    text-align: center;
    justify-content: center;
  }

  .hero-text {
    max-width: 1000px;
  }

  .hero-title-visual {
    position: relative;
    isolation: isolate;
    display: flex;
    height: 30rem;
    align-items: center;
    justify-content: center;
    gap: 4rem;
  }

  .hero-title-visual::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: -1;
    background: radial-gradient(ellipse 65% 80%, rgba(255, 255, 255, 0.20) 20%, rgba(212, 175, 55, 0) 65%);
    animation: pulse-transform 2.5s infinite ease-in-out;
  }

  .hero-logo-image {
    width: 600px;
    height: auto;
    padding: 2rem;
    /* Apply golden gradient effect to the logo */
    filter: 
      saturate(2) 
      hue-rotate(25deg) 
      brightness(1.1) 
      contrast(1.1)
      drop-shadow(0 0 30px rgba(212, 175, 55, 0.6))
      drop-shadow(0 0 60px rgba(212, 175, 55, 0.3));
    transition: filter 0.3s ease;
  }

  .hero-logo-image:hover {
    filter: 
      saturate(2.2) 
      hue-rotate(25deg) 
      brightness(1.3) 
      contrast(1.2)
      drop-shadow(0 0 40px rgba(212, 175, 55, 0.8))
      drop-shadow(0 0 80px rgba(212, 175, 55, 0.4));
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
    font-size: 2rem;
    font-weight: 600;
    color: #79746a;
    background: linear-gradient(90deg, #796011 0%, #bb952c 25%, #e7bb41 50%, #c7a034 75%, #88670e 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 1.5rem;
  }

  .hero-description {
    font-size: 1.3rem;
    color: #CCCCCC;
    margin-bottom: 2rem;
    background: linear-gradient(90deg, #bab8b1 0%, #d1cfc7 25%, #ffffff 50%, #d1cfc7 75%, #bab8b1 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    max-width: 700px;
    margin: 0 auto 3rem auto;
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
    color: #CCCCCC;
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
    box-shadow: 0 10px 80px rgba(212, 175, 55, 0.4);
  }

  .cta-secondary {
    background: transparent;
    color: #B8860B;
    border: 1px solid #B8860B;
  }

  /* Section Styles */
  .section-title {
    font-size: 2rem;
    font-weight: 700;
    text-align: center;
    margin-bottom: 1rem;
  }

  .section-subtitle {
    font-size: 1.3rem;
    color: #CCCCCC;
    text-align: center;
    background: linear-gradient(90deg, #bab8b1 0%, #d1cfc7 25%, #ffffff 50%, #d1cfc7 75%, #bab8b1 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
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
    width: 100%;
  }

  .demo-card {
    text-align: center;
    position: absolute;
    width: 100%;
    max-width: 100%;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    padding: 0 1rem;
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
    background: linear-gradient(90deg, #bab8b1 0%, #d1cfc7 25%, #ffffff 50%, #d1cfc7 75%, #bab8b1 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    font-size: 1.1rem;
    line-height: 1.6;
  }

  .solution-description {
    max-width: 600px;
    margin: 0 auto 2rem auto;
    background: linear-gradient(90deg, #bab8b1 0%, #d1cfc7 25%, #ffffff 50%, #d1cfc7 75%, #bab8b1 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .demo-features {
    width: 100%;
    max-width: 100%;
    overflow: hidden;
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
    color: #B8860B;
    background: rgba(184, 134, 11, 0.05);
    border: 1px solid rgba(184, 134, 11, 0.15);
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
    background: #B8860B;
    border-radius: 50%;
    box-shadow: 0 0 10px rgba(184, 134, 11, 0.5);
    animation: pulse 2s infinite ease-in-out;
  }
  @keyframes pulse {
    0%, 100% { transform: scale(1); opacity: 1; }
    50% { transform: scale(1.2); opacity: 0.7; }
  }
  @keyframes pulse-transform {
    0%, 100% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.15);
    }
  }
  .hidden-layer .neuron { animation-delay: 0.5s; }
  .output-layer .neuron { animation-delay: 1s; }

  /* Solutions Section */
  .solutions-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
    padding: 0 1rem;
  }

  .solution-card {
    padding: 2rem;
    text-align: center;
    width: 100%;
    min-width: 0; /* Prevents flex item overflow */
  }
  .solution-icon {
    width: 60px;
    height: 60px;
    margin: 0 auto 1.5rem auto;
    border-radius: 50%;
    background: rgba(184, 134, 11, 0.1);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .solution-icon img {
    width: 35px;
    height: 35px;
  }

  .solution-card h3 {
    font-size: 1.3rem;
    margin-bottom: 1rem;
  }

  .solution-card p {
    color: #CCCCCC;
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
    background: #333333;
    color: #DDDDDD;
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

  .tech-column {
    display: flex;
    flex-direction: column;
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
    color: #CCCCCC;
    line-height: 1.6;
  }

  /* About Us Section */
  .about-content {
    display: grid;
    grid-template-columns: 1fr;
    gap: 3rem;
    align-items: center;
  }

  .about-text {
    text-align: center;
    max-width: 800px;
    margin: 0 auto;
  }

  .about-text p {
    color: #CCCCCC;
    line-height: 1.7;
    margin-bottom: 1.5rem;
    text-align: left;
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
    background: rgba(13, 13, 13, 0.9);
    padding: 1rem;
    text-align: center;
    color: #E0E0E0;
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
    color: #CCCCCC;
    font-size: 1.1rem;
    line-height: 1.6;
  }
  .cta-action {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }
  .cta-action .cta-primary,
  .cta-action .cta-secondary {
    padding: 1.2rem 2.5rem;
    font-size: 1.1rem;
    text-align: center;
  }

  /* Responsive Styles */
  @media (max-width: 768px) {
    .hero-title-visual {
      height: auto;
      padding: 2rem 0;
    }

    .hero-logo-image {
      width: 90%;
      max-width: 400px;
      padding: 0;
    }

    .hero-tagline {
      font-size: 1.5rem;
    }

    .hero-description {
      font-size: 1.1rem;
      max-width: 90%;
    }

    .hero-actions {
      flex-direction: column;
      align-items: center;
    }

    .solutions-grid {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      padding: 0;
    }

    .solution-card {
      width: calc(100% - 2rem);
      margin: 0 1rem 1rem 1rem;
    }

    .solution-card:last-child {
      margin-bottom: 0;
    }

    .solutions-section {
      padding: 0;
    }

    .section-container {
      padding: 0;
    }

    .solution-features {
      flex-wrap: wrap;
      justify-content: center;
      gap: 0.5rem;
      padding: 0;
    }

    .feature-tag {
      font-size: 0.75rem;
      padding: 0.25rem 0.5rem;
    }

    .solution-card h3 {
      font-size: 1.2rem;
      margin-bottom: 0.75rem;
    }

    .solution-card p {
      font-size: 0.9rem;
      margin-bottom: 1rem;
    }

    .solution-icon {
      width: 50px;
      height: 50px;
      margin-bottom: 1rem;
    }

    .solution-icon img {
      width: 30px;
      height: 30px;
    }

    .rotating-demo-window {
      padding: 1.5rem;
      min-height: 400px;
    }

    .demo-card {
      padding: 0 0.5rem;
    }

    .demo-header {
      margin-bottom: 1rem;
    }

    .demo-title {
      font-size: 1.5rem;
      word-wrap: break-word;
    }

    .demo-description {
      font-size: 0.9rem;
      margin-bottom: 1.5rem;
      padding: 0 0.5rem;
    }

    .demo-features ul {
      flex-direction: column;
      align-items: center;
      gap: 0.5rem;
    }

    .demo-features li {
      width: auto;
      max-width: 90%;
      font-size: 0.9rem;
      padding: 0.4rem 1rem;
    }

    .demo-visual-background {
      transform: translate(-50%, -50%) scale(1);
      opacity: 0.05;
    }

    .neural-network-viz {
      transform: scale(0.8);
    }

    .tech-grid {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      padding: 0;
    }

    .tech-column {
      gap: 1rem;
    }

    .tech-card {
      width: calc(100% - 2rem);
      margin: 0 1rem 1rem 1rem;
      padding: 1.5rem;
    }

    .tech-card:last-child {
      margin-bottom: 0;
    }

    .tech-card h3 {
      font-size: 1.3rem;
      margin-bottom: 0.75rem;
    }

    .tech-card p {
      font-size: 0.9rem;
    }

    .tech-section {
      padding: 0;
    }

    .about-section {
      padding: 0 1rem;
    }

    .about-content {
      gap: 2rem;
    }

    .about-text {
      width: 100%;
      padding: 0;
    }

    .about-text p {
      font-size: 1rem !important;
      padding: 0;
      margin-bottom: 1rem;
      line-height: 1.6;
      width: 100%;
    }

    .section-container {
      padding: 0;
    }
  }
</style>
