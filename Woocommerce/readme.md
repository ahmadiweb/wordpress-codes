# Woocommerce Codes

> Show More

```
<script>
var $ = jQuery;
$(document).ready(function () {
    $('.has-show-more-2').css('max-height', '300px');
    $('.show-more-btn-2 .elementor-heading-title').on('click', function (e) {
        var txt = $(this).text();
        $(this).text(txt == "مشاهده بیشتر" ? "مشاهده کمتر" : "مشاهده بیشتر")
        if ($('.has-show-more-2').css('max-height') == '300px') {
            $('.has-show-more-2').css('max-height', '100%');
            $('.has-show-more-2:after').css('background-image', 'linear-gradient(#ffffff00, #ffffff)')

        } else if ($('.has-show-more-2').css('max-height') == '100%') {
            $('.has-show-more-2').css('max-height', '300px');
            $('.has-show-more-2:after').css('background-image', 'linear-gradient(#ffffff00, #ffffff00)')
            document.querySelector('.has-show-more-2').scrollIntoView({
                behavior: 'smooth',
            })
        }
    })
})
</script>

```

> Change price text in wpml

```
add_filter('woocommerce_currency_symbol', 'change_existing_currency_symbol', 10, 2);
     
function change_existing_currency_symbol( $currency_symbol, $currency ) {
    $my_current_lang = apply_filters( 'wpml_current_language', NULL );
    switch( $currency ) {
        case 'IRT':     	
        if($my_current_lang == 'en'){    	
        $currency_symbol = 'IRT';
        } else {
                 $currency_symbol = 'تومان';
            }
        break;      	
    }
    return $currency_symbol;
}
```

> Realated Product

```
function get_last_category_id($product_id = null)
{
    if (!$product_id) {
        global $post;
        $product_id = $post->ID;
    }

    $terms = get_the_terms($product_id, 'product_cat');

    if ($terms && !is_wp_error($terms)) {
        usort($terms, function ($a, $b) {
            return $b->term_id - $a->term_id;
        });
        return $terms[0]->term_id;
    }

    return null;
}

function last_category_posts_shortcode($atts)
{
    $atts = shortcode_atts(array(
        'product_id' => null,
        'post_type'  => 'product',
    ), $atts, 'last_category_posts');

    if (!$atts['product_id']) {
        global $post;
        $atts['product_id'] = $post->ID;
    }

    $last_category_id = get_last_category_id($atts['product_id']);

    if (!$last_category_id) {
        return 'No category found';
    }
    $args = array(
        'posts_per_page' => 4,
        'post_type' => $atts['post_type'],
        'post__not_in' => array($atts['product_id']),
        'tax_query' => array(
            array(
                'taxonomy' => 'product_cat',
                'field' => 'term_id',
                'terms' => $last_category_id,
            ),
        ),
    );

    $query = new WP_Query($args);

    $output = '';

    if ($query->have_posts()) {
        while ($query->have_posts()) {
            $query->the_post();
            $output .= get_post_field('post_name') . ',';
        }
        $output = rtrim($output, ',');
        wp_reset_postdata();
        return $output;
    } else {
        return 'No posts found';
    }
}

add_shortcode('last_category_posts', 'last_category_posts_shortcode');
```

