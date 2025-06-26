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
      <div class="calendly-inline-widget" data-url="https://calendly.com/company-sparqsolutionsai/30min?primary_color=e7c63f"></div>
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
    padding: 0 2rem;
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
    margin-bottom: 3rem;
  }

  .scheduling-widget-container {
    /* min-height is no longer needed here */
  }

  :global(.calendly-inline-widget) {
    min-width: 320px;
    height:1000px;
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
</style> 