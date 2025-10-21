# Quick Start Guide

## Your Portfolio is Ready! 

Running at: **http://localhost:5173**

## 3 Quick Steps to Customize:

### 1. Update GitHub Username
Edit: `src/routes/projects/+page.svelte` (line 25)
```
let githubUsername = $state('your-github-username');
```

### 2. Update Contact Info  
Edit: `src/routes/contact/+page.svelte`
- Email, phone, location
- GitHub, LinkedIn, Twitter links

### 3. Personalize About Page
Edit: `src/routes/about/+page.svelte`
- Your bio and story
- Skills and tech stack  
- Work experience

## How Projects are Filtered

GitHub repos are auto-selected if they have:
- 2+ stars OR
- Description + updated recently OR  
- Topics/tags OR
- Live demo OR
- Forks

Top 12 projects by score are displayed.

## Commands

```bash
npm run dev      # Development server
npm run build    # Production build
npm run preview  # Preview production build
```

See README.md for full documentation.
