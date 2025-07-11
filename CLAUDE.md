# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a SvelteKit website for SPARQ AI Solutions, a business that provides automated solutions for social media engagement, content creation, and customer service. The site features:

- Company marketing site with hero section, services overview, and contact forms
- Interactive chat widget powered by n8n webhook
- Calendly integration for scheduling consultations
- Animated PCB background with particle effects
- Responsive design with gold/dark theme

## Development Commands

### Core Commands
- `pnpm dev` - Start development server
- `pnpm build` - Build for production
- `pnpm preview` - Preview production build

### Quality Assurance
- `pnpm check` - Run Svelte type checking
- `pnpm check:watch` - Run type checking in watch mode

### Package Management
- Uses `pnpm` as the package manager
- Lock file: `pnpm-lock.yaml`

## Architecture

### Framework & Build
- **SvelteKit** with TypeScript
- **Vite** as the build tool
- **Vercel adapter** for deployment
- **Static site generation** capability

### Key Directories
- `src/routes/` - SvelteKit file-based routing
  - `+layout.svelte` - Main layout with navigation, footer, and chat widget
  - `+page.svelte` - Homepage content
  - `contact/+page.svelte` - Contact form with Google Apps Script integration
  - `schedule/+page.svelte` - Calendly scheduling widget
- `src/lib/` - Shared utilities and components
- `static/` - Static assets (logos, icons, favicon)

### Styling Approach
- Component-scoped CSS with Svelte `<style>` blocks
- Global styles defined in layout components
- Consistent design system using CSS custom properties and utility classes:
  - `.gold-gradient` - Gold text gradient effect
  - `.glass-panel` - Glassmorphism background effect
  - `.interactive-card` - Hover animations
  - `.spark-target` - Elements that can have electrical spark effects

### Key Features
1. **Animated PCB Background** - Canvas-based circuit board with moving particles
2. **Chat Widget** - Custom implementation with n8n webhook integration
3. **Smooth Scrolling** - Custom scroll behavior for navigation links
4. **Responsive Design** - Mobile-first approach with breakpoints
5. **Form Handling** - Contact form with Google Apps Script backend
6. **Calendly Integration** - Embedded scheduling widget

## Environment Configuration

### Required Environment Variables
- `PUBLIC_N8N_WEBHOOK_URL` - n8n webhook URL for chat functionality

### External Integrations
- **Google Apps Script** - Contact form submission endpoint
- **Calendly** - Scheduling widget (`https://calendly.com/company-sparqsolutionsai/30min`)
- **n8n** - Chat webhook integration
- **Google Fonts** - Orbitron font family

## Development Notes

### Font Loading
- Uses Google Fonts (Orbitron) for consistent branding
- Preconnect hints for performance optimization

### Performance Considerations
- Canvas animations use requestAnimationFrame for smooth rendering
- Particle system with lifecycle management to prevent memory leaks
- Responsive images and optimized assets

### Browser Compatibility
- Uses modern JavaScript features (crypto.randomUUID, fetch, etc.)
- Graceful fallbacks for older browsers where needed
- Canvas-based animations with proper cleanup

### Common Development Patterns
1. **Component Structure**: Each route component includes script, markup, and scoped styles
2. **Reactive Updates**: Uses Svelte's reactivity system for state management
3. **Event Handling**: Custom event handlers for form submissions and user interactions
4. **Animation Lifecycle**: Proper cleanup of intervals and event listeners in onMount/onDestroy

## Testing & Deployment

### Build Process
- TypeScript compilation with strict type checking
- Vite optimization and bundling
- Static site generation for improved performance

### Deployment
- Configured for Vercel deployment
- Static adapter for hosting on CDN
- Environment variables configured in deployment platform