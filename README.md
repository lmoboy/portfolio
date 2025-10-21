# Portfolio

A minimalistic black and white portfolio website built with Svelte 5 and SvelteKit.

## Features

- 🎨 **Minimalistic Design**: Clean black and white aesthetic
- 🚀 **Modern Stack**: Built with Svelte 5, SvelteKit, and Tailwind CSS v4
- 📱 **Responsive**: Works seamlessly across all device sizes
- 🔗 **GitHub Integration**: Automatically fetches and displays your GitHub repositories
- 🎯 **Smart Filtering**: Intelligently selects worthy projects to showcase based on:
  - Star count
  - Recent activity
  - Project description
  - Topics/tags
  - Homepage/live demo availability
  - Fork count

## Pages

- **Home**: Landing page with call-to-action buttons
- **About**: Personal information, skills, and experience timeline
- **Projects**: Automatically curated GitHub repositories (top 12)
- **Contact**: Contact information and message form

## Setup Instructions

### 1. Update GitHub Username

Edit `/src/routes/projects/+page.svelte` and replace `'yourusername'` with your actual GitHub username:

```typescript
let githubUsername = $state('your-actual-github-username');
```

### 2. Update Contact Information

Edit `/src/routes/contact/+page.svelte` and update:
- Email address
- Phone number
- Location
- Social media links (GitHub, LinkedIn, Twitter/X)

### 3. Customize About Page

Edit `/src/routes/about/+page.svelte` to:
- Update your bio
- Modify skills and technologies
- Update experience timeline

### 4. Install Dependencies

```bash
npm install
```

### 5. Run Development Server

```bash
npm run dev
```

Your portfolio will be available at `http://localhost:5173`

## Project Filtering Logic

The GitHub integration automatically filters repositories based on a scoring system:

**A repository is considered "worthy" if it has:**
- 2+ stars, OR
- A description and was updated in the last 180 days, OR
- Topics/tags assigned, OR
- A homepage/live demo, OR
- Has been forked

**Scoring system (for sorting):**
- Stars: 10 points each
- Forks: 5 points each
- Recent activity: Up to 20 points (more recent = higher score)
- Description: 5 points
- Topics: 3 points each
- Homepage: 15 points

The top 12 repositories by score are displayed.

## Customization

### Colors

The design uses a strict black and white palette defined in `src/app.css`. To modify:

```css
@theme {
	--color-primary: #000;
	--color-secondary: #fff;
	/* Modify these values as needed */
}
```

### Layout

All pages are contained within a max-width wrapper defined in `src/routes/+layout.svelte`:

```svelte
<main class="mx-auto max-w-6xl px-4 py-8">
```

Adjust `max-w-6xl` to change the maximum content width.

## Building for Production

```bash
npm run build
```

Preview the production build:

```bash
npm run preview
```

## Deployment

This project can be deployed to:
- Vercel
- Netlify
- Cloudflare Pages
- Any static hosting service

The default adapter (`@sveltejs/adapter-auto`) will automatically configure the build for your deployment platform.

## Technologies Used

- **Svelte 5**: Latest version with modern runes API
- **SvelteKit**: Full-stack framework for Svelte
- **TypeScript**: Type-safe JavaScript
- **Tailwind CSS v4**: Utility-first CSS framework
- **GitHub API**: For fetching repository data

## License

MIT

## Credits

Created with ❤️ using Svelte 5
