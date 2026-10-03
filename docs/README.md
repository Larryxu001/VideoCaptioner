# VideoCaptioner Documentation

This directory contains the source files for the VideoCaptioner documentation, built with [VitePress](https://vitepress.dev/).

## 📚 Read Online

The documentation is automatically deployed to GitHub Pages:

**[https://weifeng2333.github.io/VideoCaptioner/](https://weifeng2333.github.io/VideoCaptioner/)**

## 🚀 Local Development

### Install Dependencies

```bash
npm install
```

### Start the Development Server

```bash
npm run docs:dev
```

Visit http://localhost:5173 to view the documentation

### Build the Documentation

```bash
npm run docs:build
```

Build output is located in `docs/.vitepress/dist/`

### Preview the Build

```bash
npm run docs:preview
```

## 📁 Directory Structure

```
docs/
├── .vitepress/
│   ├── config.mts          # VitePress configuration (including SEO optimization)
│   └── theme/              # Custom theme (optional)
├── public/                 # Static assets (images, logo, robots.txt)
├── guide/                  # Chinese user guides
│   ├── getting-started.md
│   ├── configuration.md
│   └── ...
├── config/                 # Chinese configuration documentation
│   ├── llm.md
│   ├── asr.md
│   └── ...
├── dev/                    # Chinese developer documentation
│   ├── architecture.md
│   └── ...
├── en/                     # English documentation (mirrors the Chinese structure)
│   ├── guide/
│   ├── config/
│   └── dev/
└── index.md                # Chinese home page
```

## ✍️ Contributing Documentation

### Add a New Page

1. Create a Markdown file in the appropriate directory
2. **Add SEO frontmatter** (important!):

```markdown
---
title: Page Title - VideoCaptioner
description: Page description with keywords
head:
  - - meta
    - name: keywords
      content: keyword1,keyword2,keyword3
---

# Page Title

Content...
```

3. Add a link to `sidebar` in `.vitepress/config.mts`
4. Submit a PR

### Edit an Existing Page

Edit the Markdown file directly. Supported features include:

- **Extended Markdown syntax**: Tables, code blocks, callouts, and more
- **Vue components**: Use Vue components in Markdown
- **Custom containers**: `::: tip`, `::: warning`, `::: danger`

Example:

```md
::: tip Tip
This is a tip box
:::

::: warning Warning
This is a warning box
:::

::: danger Danger
This is a danger warning box
:::
```

### Documentation Conventions

- **Filenames**: Use lowercase letters and hyphens (for example, `getting-started.md`)
- **Headings**: Use a clear hierarchy (# → ## → ###)
- **Code blocks**: Specify the language to enable syntax highlighting
- **Images**: Place them in `public/` and reference them as `/image.png`
- **Links**: Use relative paths for internal links (for example, `/guide/getting-started`)
- **SEO**: Add title, description, and keywords to every page

## 🔍 SEO Optimization

This documentation system includes comprehensive SEO optimization. See [SEO_OPTIMIZATION.md](../SEO_OPTIMIZATION.md).

### Implemented SEO Features

✅ **Basic SEO**

- Title tag optimization
- Meta Description and Keywords
- Open Graph (social media cards)
- Twitter Card
- JSON-LD structured data
- Automatic sitemap generation
- robots.txt
- Canonical URL

✅ **Technical SEO**

- Responsive design
- Clean URLs
- Fast loading (Vite optimizations)
- HTTPS (GitHub Pages)

### Submit to Search Engines

After deployment, submit the site to search engines manually:

1. **Google Search Console**
   - Visit https://search.google.com/search-console
   - Add and verify the site
   - Submit the sitemap: `https://weifeng2333.github.io/VideoCaptioner/sitemap.xml`

2. **Bing Webmaster Tools**
   - Visit https://www.bing.com/webmasters
   - Add and verify the site
   - Submit the sitemap

3. **Baidu Webmaster Tools**
   - Visit https://ziyuan.baidu.com/
   - Add and verify the site
   - Submit the sitemap

### SEO Validation Tools

- [Google PageSpeed Insights](https://pagespeed.web.dev/)
- [Google Rich Results Test](https://search.google.com/test/rich-results)
- [Open Graph Debugger](https://developers.facebook.com/tools/debug/)
- [Twitter Card Validator](https://cards-dev.twitter.com/validator)

## 🌐 Multilingual Support

The documentation supports Chinese and English:

- **Chinese**: The `docs/` root directory
- **English**: The `docs/en/` directory

To add a language:

1. Create a language directory under `docs/` (for example, `ja/`)
2. Add the locale configuration to `.vitepress/config.mts`
3. Copy the documentation structure and translate its content

## 🔧 Tech Stack

- **VitePress**: Vite-based static site generator
- **Vue 3**: Component-based development
- **TypeScript**: Type-safe configuration

## 📝 Updating Documentation

Documentation updates automatically trigger deployment through GitHub Actions:

1. Commit documentation changes in `docs/`
2. Push to the `master` or `main` branch
3. GitHub Actions builds and deploys automatically
4. Updates take effect in approximately 2–3 minutes

## ❓ Frequently Asked Questions

### Styles Missing During Local Development?

Make sure dependencies are installed:

```bash
npm install
```

### How Do I Add Custom Styles?

Create a custom theme in `docs/.vitepress/theme/`:

```ts
// docs/.vitepress/theme/index.ts
import DefaultTheme from "vitepress/theme";
import "./custom.css";

export default DefaultTheme;
```

### How Do I Configure Search?

VitePress provides local search, which is already configured in `config.mts`.

### How Do I Optimize Images?

1. Use an image compression tool (such as TinyPNG)
2. Consider using WebP
3. Add the `loading="lazy"` attribute

### How Do I Add Google Analytics?

Add the following to `head` in `config.mts`:

```typescript
([
  "script",
  {
    async: true,
    src: "https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX",
  },
],
  [
    "script",
    {},
    `
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
`,
  ]);
```

---

For more information about VitePress, see the [official documentation](https://vitepress.dev/).

For more SEO optimization details, see [SEO_OPTIMIZATION.md](../SEO_OPTIMIZATION.md).
