<script lang="ts">
  let name = '';
  let email = '';
  let phone = '';
  let preferredContact = '';
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
    
    // Replace this URL with your Google Apps Script web app URL
    const apiEndpoint = 'YOUR_GOOGLE_APPS_SCRIPT_URL';

    try {
      const response = await fetch(apiEndpoint, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ 
          name, 
          email, 
          phone, 
          preferredContact, 
          message 
        })
      });

      if (!response.ok) {
        throw new Error('Network response was not ok');
      }

      const result = await response.json();
      
      if (result.status === 'success') {
        submissionStatus = 'success';
        // Clear form
        name = '';
        email = '';
        phone = '';
        preferredContact = '';
        message = '';
      } else {
        throw new Error('Submission failed');
      }

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
    <div class="form-container glass-panel interactive-card">
      <form on:submit|preventDefault={handleSubmit} class="contact-form">
        <div class="form-group">
          <label for="name">Name <span class="required">*</span></label>
          <input
            type="text"
            id="name"
            bind:value={name}
            required
            placeholder="Your name"
            class="form-input"
          />
        </div>

        <div class="form-group">
          <label for="email">Email <span class="required">*</span></label>
          <input
            type="email"
            id="email"
            bind:value={email}
            required
            placeholder="your.email@example.com"
            class="form-input"
          />
        </div>

        <div class="form-group">
          <label for="phone">Phone Number</label>
          <input
            type="tel"
            id="phone"
            bind:value={phone}
            placeholder="(123) 456-7890"
            class="form-input"
          />
        </div>

        <div class="form-group">
          <label for="preferredContact">Preferred Mode of Contact</label>
          <select
            id="preferredContact"
            bind:value={preferredContact}
            class="form-input"
          >
            <option value="">Select preferred contact method</option>
            <option value="email">Email</option>
            <option value="phone">Phone</option>
          </select>
        </div>

        <div class="form-group">
          <label for="message">Message <span class="required">*</span></label>
          <textarea
            id="message"
            bind:value={message}
            required
            placeholder="Tell us about your project or questions..."
            class="form-input"
            rows="5"
          ></textarea>
        </div>

        {#if submissionStatus === 'success'}
          <div class="alert success">
            Thank you for your message! We'll get back to you soon.
          </div>
        {/if}

        {#if submissionStatus === 'error'}
          <div class="alert error">
            Please fill in all required fields marked with *.
          </div>
        {/if}

        <button
          type="submit"
          class="submit-button"
          disabled={isSubmitting}
        >
          {isSubmitting ? 'Sending...' : 'Send Message'}
        </button>
      </form>
    </div>
    <div class="back-link-container">
      <a href="/" class="back-link glass-panel interactive-card">Return to Home</a>
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
    padding: 0 1rem;
    text-align: center;
  }

  .form-container {
    padding: 2rem;
    margin-bottom: 2rem;
    text-align: left;
  }

  .contact-form {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
    max-width: 600px;
    margin: 0 auto;
  }

  .form-group {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  label {
    color: #DAA520;
    font-weight: 600;
    font-size: 0.9rem;
  }

  .required {
    color: #DAA520;
    margin-left: 0.2rem;
  }

  .form-input {
    padding: 0.8rem 1rem;
    border-radius: 0.5rem;
    border: 1px solid rgba(184, 134, 11, 0.2);
    background: rgba(20, 20, 20, 0.6);
    color: #E0E0E0;
    font-family: inherit;
    transition: all 0.3s ease;
  }

  .form-input:focus {
    outline: none;
    border-color: #DAA520;
    box-shadow: 0 0 0 2px rgba(218, 165, 32, 0.2);
  }

  textarea.form-input {
    resize: vertical;
    min-height: 120px;
  }

  .submit-button {
    background: linear-gradient(135deg, #D4AF37 0%, #B48811 100%);
    color: #0D0D0D;
    border: none;
    padding: 1rem 2rem;
    border-radius: 8px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.3s ease;
    font-size: 1rem;
    margin-top: 1rem;
    width: 100%;
    font-family: 'Orbitron', sans-serif;
  }

  .submit-button:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 10px 20px rgba(212, 175, 55, 0.2);
  }

  .submit-button:disabled {
    opacity: 0.7;
    cursor: not-allowed;
  }

  .alert {
    padding: 1rem;
    border-radius: 0.5rem;
    text-align: center;
    font-weight: 600;
  }

  .alert.success {
    background: rgba(0, 255, 0, 0.1);
    border: 1px solid rgba(0, 255, 0, 0.2);
    color: #00FF00;
  }

  .alert.error {
    background: rgba(255, 0, 0, 0.1);
    border: 1px solid rgba(255, 0, 0, 0.2);
    color: #FF0000;
  }

  .back-link-container {
    margin-top: 2.5rem;
    text-align: center;
    padding: 0 1rem;
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
    transition: all 0.3s ease;
  }

  .back-link:hover {
    background: rgba(184, 134, 11, 0.1);
    transform: translateY(-2px);
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

  @media (max-width: 768px) {
    .contact-page {
      padding: 2rem 0;
    }

    .section-container {
      padding: 0 1rem;
    }

    .form-container {
      padding: 1.5rem;
      margin: 0 1rem;
      width: calc(100% - 2rem);
    }

    .section-title {
      font-size: 1.8rem;
      margin-bottom: 0.5rem;
    }

    .section-subtitle {
      font-size: 1rem;
      margin-bottom: 2rem;
      background: linear-gradient(90deg, #bab8b1 0%, #d1cfc7 25%, #ffffff 50%, #d1cfc7 75%, #bab8b1 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .submit-button {
      padding: 0.8rem 1.5rem;
    }

    .back-link {
      width: 100%;
      text-align: center;
    }
  }
</style> 