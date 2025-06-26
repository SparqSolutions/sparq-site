<script lang="ts">
  let name = '';
  let email = '';
  let company = '';
  let message = '';
  let isSubmitting = false;
  let submissionStatus: 'success' | 'error' | null = null;

  async function handleSubmit() {
    if (!name || !email || !message) {
      submissionStatus = 'error';
      return;
    }
    
    isSubmitting = true;
    submissionStatus = null;
    
    // TODO: Replace with your n8n webhook URL
    const webhookUrl = 'YOUR_N8N_WEBHOOK_URL_HERE';

    try {
      // In a real scenario, you would uncomment this and replace the URL
      /*
      const response = await fetch(webhookUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ name, email, company, message })
      });

      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      */

      // Simulating a successful submission for now
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      submissionStatus = 'success';
      name = '';
      email = '';
      company = '';
      message = '';

    } catch (error) {
      console.error('There was an error submitting the form:', error);
      submissionStatus = 'error';
    } finally {
      isSubmitting = false;
    }
  }
</script>

<svelte:head>
  <title>Contact Us - SPARQ</title>
  <meta name="description" content="Get in touch with the SPARQ team. We're here to answer your questions." />
</svelte:head>

<div class="contact-page">
    <div class="section-container">
      <h1 class="section-title gold-gradient">Contact Us</h1>
      <p class="section-subtitle">
        Have a question or a project in mind? Fill out the form below and we'll get back to you as soon as possible.
      </p>
      <div class="form-container glass-panel">
        <iframe title="Contact Form" src="https://docs.google.com/forms/d/e/1FAIpQLSeSbUIqljnh3KdW1xOLPDRTn_NaKp9p5YHqIZvOXikWy8MpLA/viewform?embedded=true" width="640" height="1200" frameborder="0" marginheight="0" marginwidth="0">Loading…</iframe>
      </div>
    </div>
  </div>

<style>
  .contact-page {
    padding: 4rem 0;
  }

  .section-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 0 2rem;
    text-align: center;
  }

  .form-container {
    padding: 1rem;
    overflow: hidden;
  }
</style> 