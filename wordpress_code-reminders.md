# Unique WordPress Code Snippets for Troubleshooting

## Handling Consent Cookies with Path Restrictions

**Summary:** Ensuring a consent cookie applies only to a specific path (`/example_path`) and removing any root-level duplicates.

```js
// Delete root-level cookie to prevent duplication
function delete_root_cookie(name) {
    document.cookie = `${name}=;expires=Thu, 01 Jan 1970 00:00:01 GMT;path=/`;
}

// Set cookie scoped explicitly to a subpath
function set_consent_cookie(value) {
    const secure = ";secure";
    const name = "consent-cookie";
    const path = ";path=/example_path";
    const date = new Date();
    date.setTime(date.getTime() + (365 * 24 * 60 * 60 * 1000));
    const expires = ";expires=" + date.toUTCString();
    
    document.cookie = `${name}=${value};SameSite=Lax${secure}${expires}${path}`;
    delete_root_cookie(name);
}
```

## Handling PostMessage Communication with Embedded Iframes

**Summary:** Sending consent status messages to an iframe only when the page path matches a specific condition.

```js
function notify_iframe(consentValue) {
    const iframeElement = document.getElementById("example_iframe");
    if (iframeElement && window.location.pathname.includes("/example_path")) {
        const iframeDomain = "https://example.com";
        iframeElement.contentWindow.postMessage(consentValue === "true" ? "consentTrue" : "consentFalse", iframeDomain);
    }
}
```

## Redirecting Single Products in a WooCommerce Category

**Summary:** If a WooCommerce category contains only one product, redirect to that product's page.

```php
add_action( 'template_redirect', 'redirect_if_single_product_in_category' );
function redirect_if_single_product_in_category() {
    if ( is_product_category() ) {
        $category = sanitize_title( woocommerce_page_title( false ) );
        $args = array(
            "post_type" => "product",
            "posts_per_page" => -1,
            "product_cat" => $category
        );
        $posts = new WP_Query( $args );
        if ( count( $posts->posts ) == 1 ) {
            wp_redirect( get_permalink( $posts->posts[0]->ID ) );
            exit;
        }
    }
}
```

## Overriding WooCommerce Product Price for Custom Post Type

**Summary:** Dynamically fetching product prices from metadata for a custom WooCommerce product type (`example_product`).

```php
add_filter('woocommerce_get_price','override_woocommerce_price',20,2);
function override_woocommerce_price($price, $product){
    if ( $product instanceof WC_Product && 'example_product' === $product->get_type() ) {
        $price = get_post_meta($product->get_id(), "price", true);
    }
    return $price;
}
```

## Scheduling and Fetching API Data in WordPress

**Summary:** Setting up a WordPress cron job to fetch and store external API data into a JSON file.

```php
// Schedule a weekly cron job
add_action('init', 'setup_cron_job');
function setup_cron_job() {
    if (!wp_next_scheduled('fetch_external_data_cron')) {
        wp_schedule_event(time(), 'weekly', 'fetch_external_data_cron');
    }
}

// Fetch data from external API
add_action('fetch_external_data_cron', 'fetch_external_data');
function fetch_external_data() {
    $api_url = 'https://exampleapi.com/data';
    $api_token = 'Bearer example-token';
    $response = wp_remote_get($api_url, [
        'headers' => [ 'Authorization' => $api_token ],
        'timeout' => 60
    ]);
    if (!is_wp_error($response)) {
        $body = wp_remote_retrieve_body($response);
        file_put_contents(get_template_directory() . '/data.json', $body);
    }
}
```

## Modifying WordPress Query Based on Custom Meta Fields

**Summary:** Adding support for querying products by custom meta fields (`example_type` and `example_color`).

```php
function modify_custom_query($query) {
    if (!is_admin() && $query->is_main_query() && is_post_type_archive('example_products')) {
        $query->set('meta_query', [
            ['key' => 'example_type', 'value' => 'ring'],
            ['key' => 'example_color', 'value' => 'gold']
        ]);
    }
}
add_action('pre_get_posts', 'modify_custom_query');
```

## Debugging Loaded WordPress Template

**Summary:** Prints out the currently loaded template for debugging purposes.

```php
add_action( 'wp_footer', 'debug_loaded_template' );
function debug_loaded_template() {
    if ( is_super_admin() ) {
        global $template;
        print_r( $template );
    }
}
```

---



