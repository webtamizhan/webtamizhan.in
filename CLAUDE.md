# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Webtamizhan is a Next.js blog built with TypeScript, Tailwind CSS, and Contentlayer. It's based on the tailwind-nextjs-starter-blog template, featuring a static site generation approach with MDX content management.

## Key Technologies

- **Next.js 15.2** (App Router)
- **TypeScript** with path aliases
- **Contentlayer 2** for content management
- **Tailwind CSS 4.0** for styling
- **MDX** for blog posts with rich plugin ecosystem
- **Pliny** library for analytics, comments, newsletter, and search

## Development Commands

```bash
# Start development server (port 3000)
yarn dev

# Build for production (standard Next.js build)
yarn build

# Build for static export (GitHub Pages / S3 / etc.)
# Sets EXPORT=1 and UNOPTIMIZED=1, generates /out folder
yarn export

# Start production server
yarn serve

# Run linter with auto-fix
yarn lint

# Analyze bundle size
yarn analyze
```

## Architecture

### Content Management (Contentlayer)

The content pipeline is defined in `contentlayer.config.ts`:

- **Blog posts**: `data/blog/**/*.mdx` → `Blog` document type
- **Authors**: `data/authors/**/*.mdx` → `Authors` document type
- **Generated types**: `.contentlayer/generated` (imported as `contentlayer/generated`)

Contentlayer processes MDX files on build, generating:
- Type-safe document objects
- Reading time calculations
- Table of contents (TOC) extraction
- Tag count JSON (`app/tag-data.json`)
- Search index JSON (`public/search.json` for kbar)

**MDX Plugins Pipeline**:
- Remark: GFM, math, code titles, image-to-JSX, GitHub alerts
- Rehype: Slug generation, autolink headings, KaTeX math, Prism+ syntax highlighting, citations, minification

### App Structure (Next.js App Router)

```
app/
├── layout.tsx          # Root layout with providers
├── page.tsx            # Home page
├── Main.tsx            # Home page content component
├── blog/               # Blog routes
│   ├── page.tsx        # Blog listing
│   ├── [...slug]/      # Dynamic blog post routes
│   └── page/[page]/    # Paginated blog listing
├── tags/               # Tag listing and individual tag pages
├── projects/           # Projects page
├── about/              # About page
├── api/                # API routes (newsletter, etc.)
├── seo.tsx             # Metadata generation
├── sitemap.ts          # Dynamic sitemap
└── robots.ts           # Robots.txt
```

### Layouts System

Three post layouts in `layouts/`:
- `PostLayout.tsx` - Default 2-column layout with sidebar (meta, author, TOC)
- `PostSimple.tsx` - Simplified single-column layout
- `PostBanner.tsx` - Layout with banner image

Two blog listing layouts:
- `ListLayout.tsx` - With search bar (v1 style)
- `ListLayoutWithTags.tsx` - With tags sidebar (current default)

Layout selection: Specify in post frontmatter with `layout` field.

### Component Organization

- **`components/`**: Reusable UI components (Header, Footer, Card, Tag, etc.)
- **`layouts/`**: Page layout templates for posts and listings
- **Data components**: Some components directly import from `data/` (siteMetadata, headerNavLinks, projectsData)

### Data & Configuration

- **`data/siteMetadata.js`**: Central site configuration (title, author, social links, analytics, newsletter, comments, search)
- **`data/headerNavLinks.ts`**: Navigation menu items
- **`data/projectsData.ts`**: Projects page content
- **`data/authors/`**: Author profiles (MDX files)
- **`data/blog/`**: Blog posts (MDX files)
- **`data/references-data.bib`**: Bibliography for citations

### TypeScript Path Aliases

Configured in `tsconfig.json`:
- `@/components/*` → `components/*`
- `@/data/*` → `data/*`
- `@/layouts/*` → `layouts/*`
- `@/css/*` → `css/*`
- `contentlayer/generated` → `.contentlayer/generated`
- `pliny/*` → `node_modules/pliny/*`

### Styling

- **Tailwind CSS**: Config file uses Tailwind 4.0 with PostCSS
- **Global styles**: `css/tailwind.css`
- **Prism theme**: `css/prism.css` (customizable syntax highlighting)
- **Theme switching**: Dark/light mode via `next-themes` (components/ThemeSwitch.tsx)

### Static Export Configuration

When building for static hosting (GitHub Pages, S3, etc.):
- Use `yarn export` command
- Environment variables: `EXPORT=1`, `UNOPTIMIZED=1`
- Optional: `BASE_PATH` for subdirectory deployment
- Output directory: `out/`
- Postbuild script generates RSS feed via `scripts/postbuild.mjs`

## Content Workflow

### Creating Blog Posts

1. Create MDX file in `data/blog/` (supports nested folders)
2. Required frontmatter:
   - `title`: Post title
   - `date`: Publication date (YYYY-MM-DD)
3. Optional frontmatter:
   - `tags`: Array of tags
   - `draft`: Set to `true` to exclude from production
   - `summary`: Post excerpt
   - `images`: Array of image URLs
   - `authors`: Array matching filenames in `data/authors/` (defaults to 'default')
   - `layout`: PostLayout | PostSimple | PostBanner
   - `lastmod`: Last modified date
   - `bibliography`: BibTeX file path for citations
   - `canonicalUrl`: Canonical URL for SEO

### Adding Authors

Create MDX file in `data/authors/` with fields: `name`, `avatar`, `occupation`, `company`, `email`, `twitter`, `bluesky`, `linkedin`, `github`, `layout`.

## Code Quality

### Pre-commit Hooks

Husky runs lint-staged on commit:
- ESLint auto-fix on `*.{js,jsx,ts,tsx}`
- Prettier format on `*.{js,jsx,ts,tsx,json,css,md,mdx}`

### ESLint Configuration

Flat config in `eslint.config.mjs`:
- TypeScript ESLint rules
- JSX a11y (accessibility)
- Prettier integration
- Next.js recommended config
- Custom rules: Disabled unused vars, prop-types, explicit module boundaries

## Important Notes

### Security Headers

`next.config.js` configures strict CSP and security headers. When adding external services (analytics, comments), update:
- Content-Security-Policy script-src
- Allowed domains in CSP

### Contentlayer Build Process

Contentlayer runs during Next.js build:
- Reads MDX files from `data/`
- Generates TypeScript types
- Processes frontmatter and content
- Runs `onSuccess` callback to generate tag-data.json and search index

### Search Implementation

Using kbar (command palette):
- Search documents path: `public/search.json`
- Generated during Contentlayer build
- Contains serialized blog post metadata

### Environment Variables

Check `.env.example` for required variables:
- Analytics (NEXT_UMAMI_ID)
- Comments (NEXT_PUBLIC_GISCUS_*)
- Newsletter provider credentials

## Deployment

### Static Export (Current Setup)

This site uses static export for GitHub Pages deployment:
- Build: `EXPORT=1 UNOPTIMIZED=1 yarn build`
- Output: `out/` directory
- Note: Server-side API routes won't work in static export

### Alternative: Vercel/Netlify

For full Next.js features (API routes, ISR):
- Use standard `yarn build`
- Deploy with Vercel or Netlify
- Remove static export configuration

## Key Files to Modify

When customizing the blog:
1. `data/siteMetadata.js` - Site identity, social links, services
2. `data/authors/default.md` - Main author profile
3. `data/headerNavLinks.ts` - Navigation menu
4. `data/projectsData.ts` - Projects page content
5. `data/logo.svg` - Site logo
6. `public/static/` - Favicons, images, assets
7. `contentlayer.config.ts` - Content schema or MDX plugins
8. `tailwind.config.js` & `css/tailwind.css` - Visual styling
9. `next.config.js` - CSP, image domains, build config
