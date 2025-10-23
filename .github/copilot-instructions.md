# Javier Jia Personal Website - GitHub Copilot Instructions

**Always follow these instructions first** and only fallback to additional search and context gathering if the information in these instructions is incomplete or found to be in error.

This is an Astro-based personal website and blog built with TypeScript, Tailwind CSS, and MDX for blog content. The site is deployed to GitHub Pages.

## Working Effectively

### Bootstrap and Install Dependencies

```bash
npm ci
```

- Takes approximately 30 seconds
- **NEVER CANCEL** - Wait for completion even if it appears to hang

### Build Commands

```bash
npm run build
```

- **NEVER CANCEL** - Takes approximately 10 seconds to complete. Set timeout to 30+ seconds minimum
- Includes both `astro check` (TypeScript checking) and `astro build` (static site generation)
- Output directory: `dist/`
- Generates static HTML files, assets, and sitemap

### Development Server

```bash
npm run dev
# OR
npm start
```

- Starts development server on `http://localhost:4321/`
- **NEVER CANCEL** - Server runs indefinitely until stopped
- Hot reloading enabled for all file changes
- Use Ctrl+C to stop the server

### Preview Production Build

```bash
npm run preview
```

- Starts preview server (usually on `http://localhost:4322/` if 4321 is in use)
- **NEVER CANCEL** - Server runs indefinitely until stopped
- Must run `npm run build` first to generate `dist/` directory

## Linting and Formatting

### Check Formatting

```bash
npm run format:check
```

- Takes approximately 2 seconds
- Verifies all files follow Prettier formatting rules

### Auto-Format Code

```bash
npm run format
```

- Takes approximately 2 seconds
- Auto-formats all files using Prettier with project settings

### Run ESLint

```bash
npm run lint
```

- Takes approximately 2 seconds
- Runs ESLint with strict TypeScript and Astro rules
- **CRITICAL**: Always run before committing changes

### Auto-Fix Linting Issues

```bash
npm run lint:fix
```

- Takes approximately 3 seconds
- Automatically fixes ESLint issues where possible

### TypeScript Checking

```bash
npm run astro check
```

- Takes approximately 5 seconds
- **NEVER CANCEL** - Set timeout to 15+ seconds minimum
- Validates TypeScript types and Astro component syntax

## Validation Workflow

**ALWAYS** run these commands in sequence before considering changes complete:

1. **Format Check**: `npm run format:check`
2. **Lint Check**: `npm run lint`
3. **Type Check**: `npm run astro check`
4. **Build**: `npm run build`
5. **Manual Validation**: Start dev server and test functionality

### Manual Validation Scenarios

After making changes, **ALWAYS** test these user scenarios:

1. **Homepage Navigation**:
   - Start dev server: `npm run dev`
   - Navigate to `http://localhost:4321/`
   - Verify homepage loads with personal info, recent posts, and links
   - Test navigation to Blog and external Playground link

2. **Blog Functionality**:
   - Click "Read All" or navigate to `/blog`
   - Verify blog listing page shows all posts with images and dates
   - Click on a blog post
   - Verify individual post loads with content and table of contents

3. **Theme Switching**:
   - Test Auto/Light/Dark theme buttons in navigation
   - Verify theme changes apply correctly

## CI/CD Requirements

The repository has GitHub Actions workflows that **MUST** pass:

### Lint Workflow (`.github/workflows/lint.yml`)

Runs on push/PR to main and develop branches with Node.js 18.x and 20.x:

- `npm ci`
- `npm run format:check`
- `npm run lint`
- `npm run astro check`
- `npm run build`

### Deploy Workflow (`.github/workflows/deploy.yml`)

Runs on push to main branch using `withastro/action` for building and deployment.

**Before committing**: Always run `npm run lint` and `npm run format:check` or the CI will fail.

## Repository Structure

### Key Directories

- `src/` - All source code
  - `src/components/` - Reusable Astro components
  - `src/content/` - Content collections (blog posts in MDX format)
  - `src/layouts/` - Page layout components
  - `src/pages/` - Page routes (index.astro, blog/, etc.)
  - `src/styles/` - Global CSS styles
- `public/` - Static assets (images, favicon, etc.)
- `dist/` - Build output directory (generated, not committed)

### Key Files

- `package.json` - Dependencies and npm scripts
- `astro.config.mjs` - Astro configuration with integrations
- `tailwind.config.js` - Tailwind CSS configuration
- `tsconfig.json` - TypeScript configuration (extends Astro strict)
- `eslint.config.js` - ESLint configuration with strict rules
- `.prettierrc` - Prettier formatting rules
- `src/consts.ts` - Site constants (title, social links, etc.)
- `src/content/config.ts` - Content collection schemas

### Important Content Files

- `src/content/blog/` - Blog post MDX files with frontmatter
- `src/components/BaseHead.astro` - HTML head component with meta tags
- `src/layouts/BaseLayout.astro` - Main page layout
- `src/layouts/BlogPostLayout.astro` - Blog post layout with TOC

## Configuration Details

### Package Manager

- Uses npm (not yarn or pnpm)
- Lock file: `package-lock.json`
- Node.js compatibility: 18.x and 20.x

### Build Target

- Static site generation (SSG)
- Output: `static` mode in `dist/` directory
- Site URL: `https://javierjia.github.io`

### Styling

- Tailwind CSS with `@tailwindcss/vite` plugin
- Custom global styles in `src/styles/global.css`
- Responsive design with dark/light theme support

### Content Management

- Blog posts written in MDX format
- Content collections with Zod schema validation
- Automatic sitemap generation
- RSS feed support

## Common Tasks

### Adding a New Blog Post

1. Create new `.md` file in `src/content/blog/`
2. Include required frontmatter: `title`, `description`, `pubDate`
3. Optional: `heroImage`, `heroImageCredit`, `updatedDate`
4. Run `npm run astro check` to validate schema
5. Test locally with `npm run dev`

### Modifying Styles

1. Edit Tailwind classes in `.astro` files
2. For global styles, edit `src/styles/global.css`
3. Run `npm run dev` for hot reloading
4. Always run `npm run format` before committing

### Updating Dependencies

1. Use `npm update` for minor updates
2. For major updates, test thoroughly with full build and validation
3. Ensure CI workflow still passes

### Performance Considerations

- Site generates static files, so build time is more important than runtime
- Images should be optimized and placed in `public/` directory
- Bundle size is monitored through Astro's build output

## Debugging Tips

### Build Failures

- Check `npm run astro check` for TypeScript errors
- Verify all imports exist and are correctly typed
- Check content collection schema validation

### Styling Issues

- Use browser dev tools with `npm run dev`
- Verify Tailwind classes are being applied
- Check for CSS conflicts in global styles

### Content Issues

- Validate MDX syntax in blog posts
- Ensure frontmatter matches schema in `src/content/config.ts`
- Check for broken internal links

## Time Expectations

- **NEVER CANCEL** any command that takes more than 2 minutes
- Dependencies install: ~30 seconds
- Full build: ~10 seconds
- Linting: ~2 seconds
- Formatting: ~2 seconds
- Type checking: ~5 seconds
- Development server startup: ~2 seconds

**Set appropriate timeouts** (30+ seconds for builds, 15+ seconds for type checking) to avoid premature cancellation.
