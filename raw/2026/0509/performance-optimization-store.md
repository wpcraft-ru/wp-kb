# Optimize Store Performance — raw source

Source: https://developer.woocommerce.com/docs/best-practices/performance/performance-optimization/

## Step 1 — Caching
- Server-side: Varnish, NGINX FastCGI Cache, Redis
- WordPress plugins: WP Rocket, W3 Total Cache, WP Super Cache
- WooCommerce-specific: cart/checkout uncached

## Step 2 — Image Optimization
- Right format (JPEG photos, PNG graphics with transparency)
- Compress: TinyPNG, ShortPixel
- Lazy loading
- Responsive images

## Step 3 — Minify and Optimize Code
- Plugins: Autoptimize, WP Rocket, W3 Total Cache
- Combine and inline critical CSS
- Defer non-critical JavaScript

## Step 4 — CDN
- Cloudflare, Fastly, Amazon CloudFront

## Step 5 — Database Optimization
- Plugins: WP-Optimize, WP-Sweep, Advanced Database Cleaner
- Remove spam comments, post revisions, expired transients
- Optimize tables

## Step 6 — Theme and Plugins
- Lightweight WooCommerce-optimized theme
- Query Monitor, WP Hive to evaluate plugin impact

## Step 7 — GZIP Compression
- Via plugin or server config (.htaccess / nginx.conf)

## Step 8 — Monitor and Analyze
- Google PageSpeed Insights, GTmetrix, WebPageTest
- New Relic, Uptime Robot
