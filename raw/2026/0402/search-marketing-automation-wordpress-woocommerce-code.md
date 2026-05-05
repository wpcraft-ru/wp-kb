# Search Results: "marketing automation" AND WordPress AND WooCommerce AND code
Source: Web search (Perplexity Sonar Pro)
Date: 2026-05-05

## Query: `"marketing automation" wordpress woocommerce code`

### Key Plugins
- **AutomateWoo**: WooCommerce-owned tool with 190+ triggers; supports custom email templates via code
- **FunnelKit Automations**: Free core with pro features for dynamic coupons, audience segmentation
- **Uncanny Automator**: No-code recipes integrating WooCommerce with 220+ plugins

### Custom Code Examples

#### Register Custom AutomateWoo Email Template
```php
add_filter( 'automatewoo_email_templates', 'my_automatewoo_email_templates' );
function my_automatewoo_email_templates( $templates ) {
    $templates['custom-1'] = 'Custom Template #1';
    return $templates;
}
```

#### Add Custom Order Field to Emails
```php
add_filter( 'woocommerce_email_order_meta_fields', 'custom_woocommerce_email_order_meta_fields', 10, 3 );
function custom_woocommerce_email_order_meta_fields( $fields, $sent_to_admin, $order ) {
    $fields['meta_key'] = array(
        'label' => __( 'Label' ),
        'value' => get_post_meta( $order->id, 'meta_key', true ),
    );
    return $fields;
}
```

#### Show Used Coupons in Admin New Order Email
```php
add_action( 'woocommerce_email_after_order_table', 'add_coupon_to_admin_new_order', 15, 2 );
function add_coupon_to_admin_new_order( $order, $is_admin_email ) {
    if ( $is_admin_email && $order->get_used_coupons() ) {
        // ... coupon list display
    }
}
```

### Key Citations
- https://woocommerce.com/products/marketing-automation-by-icegram/
- https://wordpress.org/plugins/wp-marketing-automations/
- https://automatorplugin.com/complete-guide-to-woocommerce-automation/
- https://woocommerce.com/document/email-customizer-for-automatewoo/
