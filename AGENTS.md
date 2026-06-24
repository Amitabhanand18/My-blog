1	# AGENTS.md
2	
3	This document provides an overview of the project structure for developers and AI agents working on this codebase.
4	
5	## Project Overview
6	
7	An interactive resume/portfolio application with an AI-powered assistant. Built with TanStack Start and deployed on Netlify.
8	
9	### Tech Stack
10	
11	| Layer | Technology |
12	|-------|------------|
13	| Framework | TanStack Start |
14	| Frontend | React 19, TanStack Router v1 |
15	| Build | Vite 7 |
16	| Styling | Tailwind CSS 4 |
17	| UI Components | Radix UI + custom components |
18	| Content | Content Collections (type-safe markdown) |
19	| AI | TanStack AI with multi-provider support |
20	| Language | TypeScript 5.7 (strict mode) |
21	| Deployment | Netlify |
22	
23	## Directory Structure
24	
25	```
26	├── content
27	│   └── posts
28	│       └── beach.md  # Blog post: beach adventure.
29	├── public
30	│   ├── beach.jpg
31	│   ├── favicon.ico
32	│   ├── tanstack-circle-logo.png
33	│   └── tanstack-word-logo-white.svg  # TanStack wordmark logo (white) used in header/nav.
34	├── src
35	│   ├── components
36	│   │   ├── ui
37	│   │   │   └── card.tsx  # Card UI component.
38	│   │   ├── blog-posts.tsx  # Blog post list/card display component.
39	│   │   ├── Header.tsx  # Site header with nav.
40	│   │   ├── HeaderNav.tsx  # Navigation sidebar template: mobile menu, Home link, add-on routes; EJS-driven for dynamic route generation.
41	│   │   └── VacayAssistant.tsx  # AI assistant for blog Q&A.
42	│   ├── lib
43	│   │   ├── blog-ai-hook.ts  # useBlogChat hook for /api/blog-chat.
44	│   │   ├── blog-tools.ts  # AI tools: getPostBySlug, getAllBlogPosts, getPostsByCategory for VacayAssistant.
45	│   │   └── utils.ts  # cn() helper for conditional Tailwind class merging.
46	│   ├── routes
47	│   │   ├── __root.tsx  # Root layout: Header, styles, TanStack Devtools.
48	│   │   ├── api.blog-chat.ts  # POST handler for blog AI chat with getPostBySlug, getAllBlogPosts tools.
49	│   │   ├── category.$category.tsx  # Category route: posts filtered by category.
50	│   │   ├── index.tsx  # Blog home: post list, VacayAssistant.
51	│   │   └── posts.$slug.tsx  # Post detail route: single post by slug.
52	│   ├── router.tsx  # TanStack Router setup: creates router from generated routeTree with scroll restoration.
53	│   └── styles.css  # Global styles: Tailwind, prose, highlight.js.
54	├── .gitignore  # Template for .gitignore: node_modules, dist, .env, .netlify, .tanstack, etc.
55	├── AGENTS.md  # This document provides an overview of the project structure for developers and AI agents working on this codebase.
56	├── content-collections.ts  # Content Collections config: posts schema (title, summary, categories, slug, image, date).
57	├── netlify.toml  # Netlify deployment config: build command (vite build), publish directory (dist/client), and dev server settings (port 8888, target 3000).
58	├── package.json  # Project manifest with TanStack Start, React 19, Vite 7, Tailwind CSS 4, and Netlify plugin dependencies; defines dev and build scripts.
59	├── pnpm-lock.yaml
60	├── tsconfig.json  # TypeScript config: ES2022 target, strict mode, @/* path alias for src/*, bundler module resolution.
61	└── vite.config.ts  # Vite config template: TanStack Start, React, Tailwind, Netlify plugin, and optional add-on integrations; processed by EJS.
62	```
63	
64	## Key Concepts
65	
66	### File-Based Routing (TanStack Router)
67	
68	Routes are defined by files in `src/routes/`:
69	
70	- `__root.tsx` - Root layout wrapping all pages
71	- `index.tsx` - Route for `/`
72	- `api.*.ts` - Server API endpoints (e.g., `api.resume-chat.ts` → `/api/resume-chat`)
73	
74	### Component Architecture
75	
76	**UI Primitives** (`src/components/ui/`):
77	- Radix UI-based, Tailwind-styled
78	- Card, Badge, Checkbox, Separator, HoverCard
79	
80	**Feature Components** (`src/components/`):
81	- Header, HeaderNav, ResumeAssistant
82	
83	## Configuration Files
84	
85	| File | Purpose |
86	|------|---------|
87	| `vite.config.ts` | Vite plugins: TanStack Start, Netlify, Tailwind, Content Collections |
88	| `tsconfig.json` | TypeScript config with `@/*` path alias for `src/*` |
89	| `netlify.toml` | Build command, output directory, dev server settings |
90	| `content-collections.ts` | Zod schemas for jobs and education frontmatter |
91	| `styles.css` | Tailwind imports + CSS custom properties (oklch colors) |
92	
93	## Development Commands
94	
95	```bash
96	npm run dev      # Start dev server
97	npm run build    # Production build
98	npm run preview  # Preview production build
99	```
100	
101	## Conventions
102	
103	### Naming
104	- Components: PascalCase
105	- Utilities/hooks: camelCase
106	- Routes: kebab-case files
107	
108	### Styling
109	- Tailwind CSS utility classes
110	- `cn()` helper for conditional class merging
111	- CSS variables for theme tokens in `styles.css`
112	
113	### TypeScript
114	- Strict mode enabled
115	- Import paths use `@/` alias
116	- Zod for runtime validation
117	- Type-only imports with `type` keyword
118	
119	### State Management
120	- React hooks for local state
121	- Zustand if you need it for global state
122	### Content Collections
123	
124	Markdown files in `content/posts/` are type-safe blog posts:
125	
126	- Frontmatter validated against Zod schemas in `content-collections.ts`
127	- Imported as typed array: `import { allPosts } from 'content-collections'`
128	- Each post has: `title`, `summary`, `categories[]`, `slug`, `image`, `date`, `content`
129	
130	### VacayAssistant AI Integration
131	
132	**Tools available to AI:**
133	- `getCurrentBlogPost` - Get full content and metadata of the current blog post by slug
134	- `getAllBlogPosts` - List all posts with titles, summaries, categories
135	- `searchBlogPosts` - Search posts by title, summary, or categories
136	
137	## Environment Variables
138	
139	For AI: ANTHROPIC_API_KEY, OPENAI_API_KEY, GEMINI_API_KEY, or OLLAMA_BASE_URL (same as ai add-on).
140	
141	## Application Name
142	
143	This starter uses "Application Name" as a placeholder throughout the UI and metadata. Replace it with the user's desired application name in the following locations:
144	
145	### UI Components
146	- `src/components/Header.tsx` — app name displayed in the header
147	- `src/components/HeaderNav.tsx` — app name in the mobile navigation header
148	
149	### SEO Metadata
150	- `src/routes/__root.tsx` — the `title` field in the `head()` configuration
151	
152	Search for all occurrences of "Application Name" in the `src/` directory and replace with the user's application name.
153
