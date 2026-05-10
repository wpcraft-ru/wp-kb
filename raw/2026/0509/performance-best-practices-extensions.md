# Performance Best Practices for WooCommerce — raw source

Source: https://developer.woocommerce.com/docs/best-practices/performance/performance-best-practices/

## Why performance is critical
- User Experience: fast extensions → seamless shopping → completed purchases
- Store Performance: extensions impact overall speed
- SEO and Conversion Rates: speed is critical for rankings and conversions

## Benchmarking
- Chrome Core Web Vitals "Performance" score ≥ 90 on a simple Woo site
- Tool: Chrome Lighthouse
- Integrate Lighthouse into development workflow

## Strategies
- Optimize asset loading: conditional loading, only where needed
- Efficient database queries: indexes, avoid unnecessary retrieval
- Lazy Loading: images and content
- Minification and concatenation: reduce HTTP requests
- Test with and without extension
- Caching support: compatible with popular caching solutions, avoid unnecessary cache clears
