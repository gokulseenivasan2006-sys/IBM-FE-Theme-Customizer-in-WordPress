# IBM-FE-Theme-Customizer-in-WordPress<! CTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Document</title>

</head>

<body>

<?php

// functions.php (add these functions)

/**

* Register Theme Customizer settings, controls, and selective refresh.

*/

function mytheme_customize_register( $wp_customize ){

// Section: Header

$wp_customize->add_section( 'mytheme_header_section', array(

=> __( 'Header', 'mytheme'), 'title'

'priority' => 30,

));

// Site title text (example text setting)

$wp_customize->add_setting('mytheme_site_tagline', array(

' default'

=> __( 'Just another WordPress site', 'mytheme'),

'sanitize_callback' => 'sanitize_text_field',

'transport' => 'postMessage', // enables JS live preview

));$wp_customize->add_control('mytheme_site_tagline', array(

'label'

=> __( 'Header Tagline', 'mytheme'),

'section' => 'mytheme_header_section',

'type' => 'text',

));

// Logo (image control)

$wp_customize->add_setting('mytheme_logo', array(

'default'

=>

'sanitize_callback' => 'absint', // store attachment ID

'transport' => 'refresh', // image changes usually refresh

));

$wp_customize->add_control( new WP_Customize_Media_Control($wp_customize, 'mytheme_logo', array(

' label' => __( 'Logo (PNG, JPG, SVG)', 'mytheme'),

section' => 'mytheme_header_section',

'mime_type' => 'image',

// Accent color (color control) with postMessage

$wp_customize->add_setting('mytheme_accent_color', array(

'default'

=> '#0066cc",

'sanitize_callback' => 'sanitize_hex_color',

'transport'

=> 'postMessage'));

$wp_customize->add_control(new WP_Customize_Color_Control($wp_customize, 'mytheme_accent_color', array(

'label' => __( 'Accent Color', 'mytheme'),

'section' => 'colors',

'settings'=> 'mytheme_accent_color',

)));s

// Footer text (selective refresh example)

$wp_customize->add_section('mytheme_footer_section', array(

'title' =>('Footer', 'mytheme'),

'priority' => 160,

));

$wp_customize->add_setting('mytheme_footer_text', array(

'default' => _( ' MySite All rights reserved', 'mytheme'),

'sanitize_callback' => 'wp_kses_post',

'transport' => 'postMessage',

));

$wp_customize->add_control('mytheme_footer_text', array(

'label' =>('Footer Text', 'mytheme'),

'section' => 'mytheme_footer_section',

'type'

=> 'textarea',

I

));)

if (isset($wp_customize->selective_refresh)) {

$wp_customize->selective_refresh->add_partial('mytheme_footer_text', array(

=> '.site-footer .footer-text',

'render_callback' => 'mytheme_render_footer_text',

));

// Partial for site tagline

'selector' $wp_customize->selective_refresh->add_partial('mytheme_site_tagline', array(

=> '.site-header .site-tagline',

'render_callback' => 'mytheme_render_site_tagline',

));

}

}

add_action('customize_register', 'mytheme_customize_register' );

/**

* Render callback for footer partial.

*/

function mytheme_render_footer_text() {

}

echo wp_kses_post(get_theme_mod('mytheme_footer_text', '© MySite All rights reserved'));function mytheme_render_site_tagline() {

echo esc_html(get_theme_mod('mytheme_site_tagline', 'Just another WordPress site')

}

* Enqueue Customizer preview JavaScript (for postMessage transport).

*/

function mytheme_customize_preview_js() {

wp_enqueue_script(

'mytheme-customizer-preview',

get_template_directory_uri() . '/assets/js/customizer.js',

array( 'customize-preview', 'jquery'),

filemtime(get_template_directory() . '/assets/js/customizer.js'),

true

);

}

add_action( 'customize_preview_init', 'mytheme_customize_preview_js' );

* Output dynamic inline styles that will use theme mods (fallback for no-JS preview).

*/

function mytheme_customizer_css() {}

add_action('customize_preview_init', 'mytheme_customize_preview_js' );

**

* Output dynamic inline styles that will use theme mods (fallback for no-JS preview).

*/

function mytheme_customizer_css() {

}

$accent = get_theme_mod('mytheme_accent_color', '#0066cc');

?>

<style type="text/css">

:root { --mytheme-accent: <?php echo esc_attr( $accent ); ?>; } .site-link, .button { background-color: var(--mytheme-accent); }

</style>

<?php

add_action('wp_head', 'mytheme_customizer_css' );

</body>

</html><!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Document</title>

</head>

<body>

(function($) {

wp.customize('mytheme_site_tagline', function (value) { value.bind(function(newVal) { $('.site-header.site-tagline').text(newVal);

});

});

wp.customize('mytheme_footer_text', function(value) {

I

value.bind(function(newVal) {

}); $('.site-footer .footer-text').html(newVal);

});

wp.customize('mytheme_accent_color', function (value) {

value.bind(function(newVal) {

document.documentElement.style.setProperty('--mytheme-accent', new });

});

<img class="site-logo">, swap src

wp.customize('mytheme_logo', function (value) {

value.bind(function(newVal) {value.bind(function(newVal) {

if (typeof newVal === 'string' && newVal.length) {

$('.site-header .site-logo img').attr('src', newVal);

} else if (typeof newVal === 'number')

});

});

I

})(jQuery);

</body>

</html><!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Document</title>

</head>

<body>

<header class="site-header">

<div class="site-branding">

<?php

$logo_id= get_theme_mod('mytheme_logo');

if ($logo_id):

?>

$logo_url = wp_get_attachment_image_url($logo_id, 'full');

<a class="site-logo" href="<?php echo esc_url(home_url('/')); ?>">

<img src="<?php echo esc url($logo url); ?>" alt="<?php bloginfo('name'); ?>">

<?php else: ?>

</a>

<a class="site-title" href="<?php echo esc_url(home_url('/')); ?>"><?php bloginfo('name'); ?></a>

<?php endif; ?>

<div class="site-tagline"><?php echo esc_html(get_theme mod('mytheme_site_tagline, get bloginfo('description')

</div>

</header>

</body>

</html><!DOCTYPE html>

2

<html lang="en">

3

<head>

4

5

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Document</title>

</head>

B

<body>

}

:root {

--mytheme-accent: #0066cc;

a, site-link {

color: var(--mytheme-accent);

}

.button, .btn

background: var(--mytheme-accent);

color: #fff;

</body>

</html>
