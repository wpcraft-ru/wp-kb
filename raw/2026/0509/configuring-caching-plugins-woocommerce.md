# How to configure caching plugins for WooCommerce

Source: https://developer.woocommerce.com/docs/best-practices/performance/configuring-caching-plugins/

## Excluding pages from the cache

Cart, My Account, Checkout — these pages need to stay dynamic since they display information specific to the current customer and their cart.

## Excluding WooCommerce session from the cache

Exclude `_wc_session_` from database caching.

## Excluding WooCommerce cookies from the cache

| Cookie | Duration | Purpose |
|--------|----------|---------|
| `woocommerce_cart_hash` | session | Helps WooCommerce determine when cart contents/data changes. |
| `woocommerce_items_in_cart` | session | Helps WooCommerce determine when cart contents/data changes. |
| `wp_woocommerce_session_` | 2 days | Contains a unique code for each customer so that it knows where to find the cart data in the database. |
| `woocommerce_recently_viewed` | session | Powers the Recent Viewed Products widget. |
| `store_notice[notice id]` | session | Allows customers to dismiss the Store Notice. |

### W3 Total Cache

Add `mfunc` to the 'Ignored comment stems' option in Minify settings.

### WP-Rocket

WooCommerce is fully compatible. Ensure Cart, Checkout, My Account are not cached. Avoid JS file minification.

### WP Super Cache

Natively compatible. WooCommerce sends information to WP Super Cache to not cache Cart, Checkout, My Account by default.

### Varnish

```varnish
if (req.url ~ "^/(cart|my-account|checkout|addons)") {
    return (pass);
}
if ( req.url ~ "\\?add-to-cart=" ) {
    return (pass);
}
```

## Troubleshooting

### Varnish configuration

```varnish
# vcl_recv
if (!(req.url ~ "(wp-login|wp-admin|cart|my-account/*|wc-api*|checkout|addons|logout|lost-password|product/*)")) {
    unset req.http.cookie;
}
if (req.url ~ "^/(cart|my-account/*|checkout|wc-api/*|addons|logout|lost-password|product/*)") {
    return (pass);
}
if (req.url ~ "\?add-to-cart=" ) {
    return (pass);
}
if (req.url ~ "\?wc-api=" ) {
    return (pass);
}

# vcl_fetch
if ( (!(req.url ~ "(wp-(login|admin)|login|cart|my-account/*|wc-api*|checkout|addons|logout|lost-password|product/*)")) || (req.request == "GET") ) {
    unset beresp.http.set-cookie;
}
```

### Password Reset loop

My Account page being cached causes password reset loops. Exclude from caching.
