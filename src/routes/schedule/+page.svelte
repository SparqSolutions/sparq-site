<script lang="ts">
  // The Calendly script handles widget initialization automatically.
</script>

<svelte:head>
  <title>Schedule a Consultation - SPARQ</title>
  <meta name="description" content="Schedule a free consultation with a SPARQ AI specialist to discuss your business needs." />
  <script type="text/javascript" src="https://assets.calendly.com/assets/external/widget.js" async></script>
</svelte:head>

<div class="schedule-page">
  <div class="section-container">
    <h1 class="section-title gold-gradient">Schedule Your Free Consultation</h1>
    <p class="section-subtitle">
      Choose a time that works for you to speak with one of our AI specialists. We're excited to learn about your business and how we can help you grow.
    </p>

    <div class="scheduling-widget-container">
      <!-- Calendly inline widget begin -->
      <div 
        class="calendly-inline-widget" 
        data-url="https://calendly.com/company-sparqsolutionsai/30min?hide_landing_page_details=1&hide_gdpr_banner=1&background_color=000000&text_color=b8860b&primary_color=b8860b" 
        style="min-width:320px;height:700px;"
      ></div>
      <script type="text/javascript" src="https://assets.calendly.com/assets/external/widget.js" async></script>
      <!-- Calendly inline widget end -->
    </div>

    <div class="back-link-container">
      <a href="/" class="back-link interactive-card">Return to Home</a>
    </div>
  </div>
</div>

<style>
  .schedule-page {
    padding: 4rem 0;
  }

  .section-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 0 1rem;
    text-align: center;
  }

  .section-title {
    font-size: 2.5rem;
    font-weight: 700;
    margin-bottom: 1rem;
  }

  .section-subtitle {
    font-size: 1.2rem;
    color: #666666;
    background: linear-gradient(90deg, #bab8b1 0%, #d1cfc7 25%, #ffffff 50%, #d1cfc7 75%, #bab8b1 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 3rem;
  }

  .scheduling-widget-container {
    position: relative;
    width: 100%;
    overflow: hidden;
  }

  :global(.calendly-inline-widget) {
    position: relative;
    width: 100% !important;
    min-width: 280px;
    height: 100vh !important;
    max-height: 1000px;
  }

  .back-link-container {
    margin-top: 2.5rem;
    text-align: center;
  }

  .back-link {
    display: inline-block;
    background: transparent;
    color: #B8860B;
    border: 1px solid #B8860B;
    padding: 1rem 2rem;
    font-size: 1rem;
    font-weight: 600;
    border-radius: 0.5rem;
    text-decoration: none;
  }

  @media (max-width: 768px) {
    .schedule-page {
      padding: 2rem 0;
    }

    .section-container {
      padding: 0 1rem;
    }

    .section-title {
      font-size: 1.8rem;
      margin-bottom: 0.5rem;
    }

    .section-subtitle {
      font-size: 1rem;
      margin-bottom: 2rem;
    }

    :global(.calendly-inline-widget) {
      margin: 0 -1rem;
      width: calc(100% + 2rem) !important;
      min-height: 140vh;
    }

    .back-link {
      width: 100%;
      text-align: center;
    }
  }
</style> 