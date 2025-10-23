# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is Javier Jia's personal blog - an Astro-based website built with TypeScript, Tailwind CSS, and MDX. The site is deployed to GitHub Pages at https://javierjia.github.io and features automatic reading time calculation, last-modified timestamps via git history, RSS feeds, and dark mode support.

## Essential Commands

### Development
```bash
npm ci                    # Install dependencies (takes ~30s, never cancel)
npm run dev              # Start dev server at http://localhost:4321/
npm start                # Alias for npm run dev
npm run preview          # Preview production build (must run build first)
```

### Build and Validation
```bash
npm run build            # Type check + build (takes ~10s, never cancel)
npm run astro check      # Run TypeScript validation only
```

### Code Quality
```bash
npm run lint             # Run ESLint
npm run lint:fix         # Auto-fix ESLint issues
npm run format           # Auto-format with Prettier
npm run format:check     # Check formatting without modifying
```

### Pre-Commit Workflow
Always run these commands in sequence before committing:
1. `npm run format:check`
2. `npm run lint`
3. `npm run astro check`
4. `npm run build`

## Architecture

### Content Collections System
- Blog posts are defined as content collections in [src/content/config.ts](src/content/config.ts) using Zod schemas
- All blog posts live in [src/content/blog/](src/content/blog/) as MDX files with frontmatter
- Required frontmatter: `title`, `description`, `pubDate`
- Optional frontmatter: `updatedDate`, `coverImageCredit`

### Remark/Rehype Plugin Pipeline
The site uses custom Remark and Rehype plugins configured in [astro.config.mjs](astro.config.mjs):
- **remarkReadingTime** ([src/plugins/remark-reading-time.mjs](src/plugins/remark-reading-time.mjs)): Calculates reading time and injects `minutesRead` into frontmatter
- **remarkModifiedTime** ([src/plugins/remark-modified-time.mjs](src/plugins/remark-modified-time.mjs)): Uses git history to extract last commit date and injects `lastModified` into frontmatter
- **rehypeFigureTitle**: Adds figure captions support
- **rehypeAccessibleEmojis**: Makes emojis accessible

These plugins automatically enhance blog posts with computed metadata during the build process.

### Routing Structure
- [src/pages/index.astro](src/pages/index.astro) - Homepage
- [src/pages/blog/index.astro](src/pages/blog/index.astro) - Blog listing page
- [src/pages/blog/[...slug].astro](src/pages/blog/[...slug].astro) - Dynamic route for individual blog posts
- [src/pages/rss.xml.js](src/pages/rss.xml.js) - RSS feed generation

### Layout Components
- [src/layouts/BaseLayout.astro](src/layouts/BaseLayout.astro) - Main page layout with navbar/footer
- [src/layouts/BlogPostLayout.astro](src/layouts/BlogPostLayout.astro) - Blog post layout with table of contents, reading time, and last modified date

### Key Components
- [src/components/BaseHead.astro](src/components/BaseHead.astro) - HTML head with meta tags and SEO
- [src/components/Navbar.astro](src/components/Navbar.astro) - Navigation with theme selector
- [src/components/ThemeSelector.astro](src/components/ThemeSelector.astro) - Auto/Light/Dark theme switcher
- [src/components/TableOfContents.astro](src/components/TableOfContents.astro) - Auto-generated TOC for blog posts
- [src/components/BlogCard.astro](src/components/BlogCard.astro) - Blog post preview cards

### Constants and Configuration
- [src/consts.ts](src/consts.ts) - Site constants including `SITE_TITLE`, `SITE_DESCRIPTION`, `SocialLinks`, and `WebsiteLinks`
- [astro.config.mjs](astro.config.mjs) - Astro configuration with integrations (MDX, Sitemap, Partytown, astro-icon)
- [tailwind.config.js](tailwind.config.js) - Tailwind CSS configuration
- [tsconfig.json](tsconfig.json) - TypeScript configuration (extends Astro strict)

## Adding a Blog Post

1. Create a new `.md` file in [src/content/blog/](src/content/blog/) (filename becomes URL slug)
2. Add required frontmatter:
   ```yaml
   ---
   title: 'Your Post Title'
   description: 'Brief description'
   pubDate: 'Jan 01 2025'
   coverImageCredit: 'Photographer Name, Source' # optional
   ---
   ```
3. Add cover image at `src/assets/blogimages/<your-slug>/cover.jpg`
   - **Required aspect ratio**: 16:9 (e.g., 1920x1080, 1280x720, 853x480)
   - Images are automatically cropped using `aspect-video` and `object-cover`
   - Recommended minimum width: 1280px for high-quality displays
   - Supported formats: JPG, PNG, GIF
4. For images with captions: `![Alt text](../../assets/blogimages/<your-slug>/image.ext)`
5. Reading time and last modified date are automatically calculated
6. Run `npm run astro check` to validate schema
7. Test locally with `npm run dev`

## CI/CD

### GitHub Actions Workflows
- **Lint workflow** ([.github/workflows/lint.yml](.github/workflows/lint.yml)): Runs on push/PR to main and develop branches with Node.js 18.x and 20.x, executes format check, lint, type check, and build
- **Deploy workflow** ([.github/workflows/deploy.yml](.github/workflows/deploy.yml)): Deploys to GitHub Pages on push to main using `withastro/action`

Both workflows must pass for changes to be merged.

## Important Notes

### Command Timeouts
- **NEVER CANCEL** commands prematurely
- `npm ci`: ~30 seconds
- `npm run build`: ~10 seconds (includes type checking)
- `npm run astro check`: ~5 seconds
- Set timeouts to 30+ seconds for builds, 15+ seconds for type checking

### RSS Feed Implementation
The RSS feed at [src/pages/rss.xml.js](src/pages/rss.xml.js) strips the `.md` extension from post IDs to generate correct URLs: `/blog/${post.id.replace('.md', '')}/`

### Package Manager
- Uses **npm** (not yarn or pnpm)
- Lock file: `package-lock.json`
- Node.js compatibility: 18.x and 20.x

### Styling
- Tailwind CSS with `@tailwindcss/vite` plugin
- Custom global styles in [src/styles/global.css](src/styles/global.css)
- Responsive design with theme switching (auto/light/dark)
