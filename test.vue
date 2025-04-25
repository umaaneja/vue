Conversion Objective:
Transform a Next.js theme into a Hugo theme with 1:1 visual and structural parity, preserving:

Exact CSS class hierarchy and IDs

Identical DOM structure

Same asset paths and routing

Equivalent layout organization

Step-by-Step Requirements:

Project Structure Conversion

Create Hugo theme structure: layouts/, static/, archetypes/, content/, assets/

Convert Next.js /pages to Hugo content files and section templates

Replicate Next.js public/static files in Hugo's /static directory

Layout Preservation

Convert Next.js _app.js to Hugo's baseof.html

Transform React components to Hugo partials:

components/Header.js → layouts/partials/header.html

components/Footer.js → layouts/partials/footer.html

Maintain identical HTML structure with Hugo template syntax:

html
<!-- Next.js -->
<div className="container">{children}</div>

<!-- Hugo -->
<div class="container">{{ block "main" . }}{{ end }}</div>
Styling Continuity

Directly copy CSS files to /assets/css/

Convert CSS Modules to Hugo's resource system:

html
<!-- Was: import styles from './Layout.module.css' -->
{{ $styles := resources.Get "css/Layout.css" | minify }}
<link rel="stylesheet" href="{{ $styles.Permalink }}">
Data Handling Conversion

Convert getStaticProps to Hugo front matter:

yaml
# content/post.md
---
title: "My Post"
date: 2023-09-20
---
Replace GraphQL/fetch calls with Hugo's data templates:

go
{{ range where .Site.Pages "Section" "blog" }}
  <article>{{ .Content }}</article>
{{ end }}
Dynamic Routing

Convert Next.js dynamic routes ([slug].js) to Hugo content sections:

/content/blog/post-1.md
/layouts/blog/single.html
Implement Hugo's lookup order for templates

Asset Pipeline

Recreate Next.js image optimization using Hugo Pipes:

go
{{ $image := resources.Get "images/hero.jpg" }}
{{ $image = $image.Resize "1200x webp" }}
<img src="{{ $image.RelPermalink }}" alt="Hero Image">
Client-Side JS Handling

Convert React state management to vanilla JS or Alpine.js

Preserve event handlers by maintaining DOM IDs:

html
<!-- Keep identical ID -->
<button id="themeToggle" onclick="toggleTheme()">Toggle</button>
Critical Preservation Rules:

Never modify original CSS selectors

Keep all HTML element IDs unchanged

Maintain exact DOM nesting structure

Preserve media query breakpoints

Mirror all animation timings and easing functions

Verification Steps:

Use diff tool to compare original and converted HTML output

Maintain identical Chrome DevTools computed styles

Replicate all hover/focus states

Match scroll behavior and responsive breakpoints

Example Conversion Snippet:

html
<!-- Next.js Original -->
<main className="bg-white dark:bg-gray-800">
  <Header />
  <div className="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
    {children}
  </div>
</main>

<!-- Hugo Converted -->
<main class="bg-white dark:bg-gray-800">
  {{ partial "header.html" . }}
  <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
    {{ block "main" . }}{{ end }}
  </div>
</main>
Special Considerations:

Convert process.env variables to Hugo config.toml

Replace next/router with Hugo's {{ .RelPermalink }}

Implement dark mode using Hugo's .Store instead of React state

Convert Next.js API routes to Hugo's custom output formats if needed
