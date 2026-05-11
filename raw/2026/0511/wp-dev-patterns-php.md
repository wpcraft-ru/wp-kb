Using PHP in Patterns – Theme Handbook | Developer.WordPress.org
[Skip to content](#wp--skip-link--target)
* [Showcase](https://wordpress.org/showcase/)
* [Plugins](https://wordpress.org/plugins/)
* [Themes](https://wordpress.org/themes/)
* [Hosting](https://wordpress.org/hosting/)
* [News](https://wordpress.org/news/)
* Resources
+ [Learn WordPress](https://learn.wordpress.org/)
+ [Documentation](https://wordpress.org/documentation/)
+ [Education](https://wordpress.org/education/)
+ [Forums](https://wordpress.org/support/forums/)
+ [Developers](https://developer.wordpress.org/)
+ [Blocks](https://wordpress.org/blocks/)
+ [Patterns](https://wordpress.org/patterns/)
+ [Photos](https://wordpress.org/photos/)
+ [Openverse ↗︎](https://openverse.org/)
+ [WordPress.tv ↗︎](https://wordpress.tv/)
* About
+ [About WordPress](https://wordpress.org/about/)
+ [Make WordPress](https://make.wordpress.org/)
+ [Events](https://events.wordpress.org/)
+ [Five for the Future](https://wordpress.org/five-for-the-future/)
+ [Enterprise](https://wordpress.org/enterprise/)
+ [Gutenberg ↗︎](https://wordpress.org/gutenberg/)
+ [Job Board ↗︎](https://jobs.wordpress.net/)
+ [Swag Store ↗︎](https://mercantile.wordpress.org/)
* [Get WordPress](https://wordpress.org/download/)
Search in WordPress.org
[Get WordPress](https://wordpress.org/download/)
[WordPress Developer Resources](https://developer.wordpress.org)
Using PHP in Patterns
* [Developer Blog](https://developer.wordpress.org/news/)
* [Code Reference](https://developer.wordpress.org/reference/)
* [WP-CLI Commands](https://developer.wordpress.org/cli/commands/)
* [Developer Blog](https://developer.wordpress.org/news/)
* [Code Reference](https://developer.wordpress.org/reference/)
* [WP-CLI Commands](https://developer.wordpress.org/cli/commands/)
[Home](https://developer.wordpress.org)[Theme Handbook](https://developer.wordpress.org/themes/)[Patterns](https://developer.wordpress.org/themes/patterns/)Using PHP in Patterns
Search
# Using PHP in Patterns
## In this article
Table of Contents
* [Patterns are registered on init](#patterns-are-registered-on-init)
* [An example pattern](#an-example-pattern)
* [Internationalizing text in patterns](#internationalizing-text-in-patterns)
* [Using images and other assets](#using-images-and-other-assets)
* [foreach() loops](#foreach-loops)
[↑ Back to top](#wp--skip-link--target)
One of the biggest advantages of patterns over features like templates and template parts is that you can use PHP in them, which opens a world of possibilities for what you can accomplish. However, there are limits on what functionality is available to your patterns.
In this article, you will learn the limitations of the patterns system, when and how you can use PHP, and some of the most common use cases for dynamic functionality.
## [Patterns are registered on init](#patterns-are-registered-on-init)
When using PHP in patterns, it’s important to understand when the pattern’s block markup is actually compiled versus when it is rendered.
Pattern registration happens on the [`init`](https://developer.wordpress.org/reference/hooks/init/) hook. At this point, WordPress compiles the content of the pattern and saves a copy of it as HTML-based block markup. This allows it to be used in both the editor and the front end. It’s during this registration process where the pattern can execute PHP code.
A pattern’s block markup is not actually rendered until it is used either in the editor or on the front end. However, the block markup is plain HTML at the time it is rendered.
At a practical level, this means that a lot of data is unavailable during the registration process on `init`. Your patterns cannot access things like the global query or post. They also cannot use functions associated with that data. So this means that WordPress functions like `is_home()`, `is_single()`, `get_post()`, and others are simply not ready yet.
For example, this PHP wouldn’t execute correctly in a pattern:
```
php if ( is_page() ) : ?
php the_title(); ?
php endif; ?
```
Neither the `is_page()` nor the `the_title()` functions have the global data they need when a pattern is registered, so they won’t work.
If you’re accustomed to building classic themes, this can feel very unintuitive, especially if you’ve been equating PHP-based patterns to classic, PHP-based template parts.
There are other methods for executing PHP at the time of render, such as using the [Block Bindings API](https://developer.wordpress.org/news/tag/block-bindings/). But those methods are outside the scope of pattern documentation.
You still have a wide and wonderful range of PHP functions and many WordPress functions available to you. And you’ll learn more about what you *can* use in the upcoming sections of this article.
## [An example pattern](#an-example-pattern)
For the rest of this article, let’s use a single pattern that begins as mostly HTML. Then, you’ll walk through adding PHP to dynamically handle some portions of it.
Create a new file named `patterns/hero.php` in your theme and put this code into it:
```
php
/**
* Title: Hero
* Slug: themeslug/hero
* Categories: featured
*/
?
## Welcome to My Site
This is my little home away from home.
Button A
Button B
```
If you insert it the pattern into the editor, it should look like this:
## [Internationalizing text in patterns](#internationalizing-text-in-patterns)
One of the primary use cases for block patterns is [internationalizing text](https://developer.wordpress.org/themes/functionality/internationalization/) (making it ready for translation). For example, if you have text in a template or template part, the only way to internationalize that text is to move it into a pattern, where you have full access to PHP.
Because pattern files are just PHP, you can use any of the internationalization functions within WordPress. In the below example, you’ll use [`esc_html_e()`](https://developer.wordpress.org/reference/functions/esc_html_e/) to both escape the text for security and make it ready for translators.
Let’s look at the Heading block from your Hero pattern:
```
## Welcome to My Site
```
As you can see, the “Welcome to My Site” text in the Heading block is not internationalized. When shipping your theme to users all around the world who speak many different languages, it’s important to ensure this text can be translated.
To internationalize the Heading block’s text, you must wrap it in an internationalization function (`esc_html_e()` in this case). So you’d change the block markup to look like this:
```
## php esc_html_e( 'Welcome to My Site', 'themeslug' ); ?
```
There are three other places where you should also make these changes. The first is to the Paragraph block text, and then the two Button blocks. Your new `patterns/hero.php` file should look like this:
```
php
/**
* Title: Hero
* Slug: themeslug/hero
* Categories: featured
*/
?
## php esc_html_e( 'Welcome to My Site', 'themeslug' ); ?
php esc_html_e( 'This is my little home away from home.', 'themeslug' ); ?
php esc_html_e( 'Button A', 'themeslug' ); ?
php esc_html_e( 'Button B', 'themeslug' ); ?
```
## [Using images and other assets](#using-images-and-other-assets)
Adding dynamic URLs for images and other assets is another important feature of patterns. Because you have PHP access, you can use functions like [`get_theme_file_uri()`](https://developer.wordpress.org/reference/functions/get_theme_file_uri/) to output an image URL that’s bundled with your theme, for example.
Try adding an image background to your “hero” pattern’s Cover block. If you need an image, try grabbing one from the WordPress [Photo Directory](https://wordpress.org/photos/) (the example below uses this [Nepal night scene photo](https://wordpress.org/photos/photo/41664fab9b/)).
Download an image and save it to your theme’s `/assets/images` folder (or a folder of your choice) and name it `hero-background.jpg`.
Then open your pattern in the editor and upload the image as the background for the Cover block in your Hero pattern, which should look like this:
If you copy the blocks that make up the pattern, you’ll notice that you have two instances of the image URL in your pattern code. It will be a hardcoded URL similar to this:
```
http://localhost/wp-content/uploads/2023/10/hero-background.jpg
```
You need to change both of those instances so that they point to the image that is located in your theme’s `/assets/images` folder. For this, you need do two things:
* Get the correct URL using a function like [`get_theme_file_uri()`](https://developer.wordpress.org/reference/functions/get_theme_file_uri/).
* Escape the URL so that it’s safe for output with [`esc_url()`](https://developer.wordpress.org/reference/functions/esc_url/).
Change the hard coded image URLs to this:
```
php echo esc_url( get_theme_file_uri( 'assets/images/hero-background.jpg' ) ); ?
```
Your final pattern code should look like this (note that this example includes internationalized text as explained in the previous section):
```
php
/**
* Title: Hero
* Slug: themeslug/hero
* Categories: featured
*/
?
![](<?php echo esc_url( get_theme_file_uri( 'assets/images/hero-background.jpg' ) ); ?>)
## php esc_html_e( 'Welcome to My Site', 'themeslug' ); ?
php esc_html_e( 'This is my little home away from home.', 'themeslug' ); ?
php esc_html_e( 'Button A', 'themeslug' ); ?
php esc_html_e( 'Button B', 'themeslug' ); ?
```
## [foreach() loops](#foreach-loops)
You have access to nearly every PHP function and feature at your disposal, and many things will undoubtedly be useful to you as you build themes. But `foreach()` loops are likely one of the features that you will often reach for.
`foreach()` loops are particularly useful in patterns that have repeated blocks. For example, in your `patterns/hero.php` file, you have a set of two Button blocks:
```
php esc_html_e( 'Button A', 'themeslug' ); ?
php esc_html_e( 'Button B', 'themeslug' ); ?
```
One of the foundational principles of development is DRY (Don’t Repeat Yourself). And this code breaks that principle. With a single `foreach()` loop, you can create the block markup once and loop through it.
First, you need an array of items to loop through, which is the Button content itself. At the top of your `patterns/hero.php` file before the closing `?>` tag, add an array of labels:
```
$buttons = array(
__( 'Button A', 'themeslug' ),
__( 'Button B', 'themeslug' )
);
```
Then replace the markup for your two Button blocks with this code:
```
php foreach ( $buttons as $button ) : ?
php echo esc_html( $button ); ?
php endforeach; ?
```
Your full `patterns/hero.php` file should now look like the following (note this includes both the internationalized text and dynamic image example code from earlier):
```
php
/**
* Title: Hero
* Slug: themeslug/hero
* Categories: featured
*/
$buttons = array(
__( 'Button A', 'themeslug' ),
__( 'Button B', 'themeslug' )
);
?
![](<?php echo esc_url( get_theme_file_uri( 'assets/images/hero-background.jpg' ) ); ?>)
## php esc_html_e( 'Welcome to My Site', 'themeslug' ); ?
php esc_html_e( 'This is my little home away from home.', 'themslug' ); ?
php foreach ( $buttons as $button ) : ?
php echo esc_html( $button ); ?
php endforeach; ?
```
At this point, your pattern is fully dynamic. As you dive deeper into pattern development, you’ll come up with even more use cases for using PHP. Think of this article as just the foundation that you’ll build from.
First published
April 30, 2024
[Previous
Registering Patterns
Previous: Registering Patterns](https://developer.wordpress.org/themes/patterns/registering-patterns/)
[Next
Usage in Templates
Next: Usage in Templates](https://developer.wordpress.org/themes/patterns/usage-in-templates/)
## Chapters
Chapter list
* [Theme Handbook](https://developer.wordpress.org/themes/)
* [Getting Started](https://developer.wordpress.org/themes/getting-started/)
+ [What Is a Theme?](https://developer.wordpress.org/themes/getting-started/what-is-a-theme/)
+ [Who Is This Handbook For?](https://developer.wordpress.org/themes/getting-started/who-is-this-handbook-for/)
+ [Reading This Handbook](https://developer.wordpress.org/themes/getting-started/reading-this-handbook/)
+ [Tools and Setup](https://developer.wordpress.org/themes/getting-started/tools-and-setup/)
+ [Quick-Start Guide](https://developer.wordpress.org/themes/getting-started/quick-start-guide/)
* [Core Concepts](https://developer.wordpress.org/themes/core-concepts/)
+ [Theme Structure](https://developer.wordpress.org/themes/core-concepts/theme-structure/)
+ [Main Stylesheet (style.css)](https://developer.wordpress.org/themes/core-concepts/main-stylesheet/)
+ [Templates](https://developer.wordpress.org/themes/core-concepts/templates/)
+ [Custom Functionality (functions.php)](https://developer.wordpress.org/themes/core-concepts/custom-functionality/)
+ [Including Assets](https://developer.wordpress.org/themes/core-concepts/including-assets/)
+ [Global Settings and Styles](https://developer.wordpress.org/themes/core-concepts/global-settings-and-styles/)
* [Global Settings and Styles (theme.json)](https://developer.wordpress.org/themes/global-settings-and-styles/)
+ [Introduction to theme.json](https://developer.wordpress.org/themes/global-settings-and-styles/introduction-to-theme-json/)
+ [Settings](https://developer.wordpress.org/themes/global-settings-and-styles/settings/)
- [Appearance Tools](https://developer.wordpress.org/themes/global-settings-and-styles/settings/appearance-tools/)
- [Blocks](https://developer.wordpress.org/themes/global-settings-and-styles/settings/blocks/)
- [Border](https://developer.wordpress.org/themes/global-settings-and-styles/settings/border/)
- [Color](https://developer.wordpress.org/themes/global-settings-and-styles/settings/color/)
- [Custom](https://developer.wordpress.org/themes/global-settings-and-styles/settings/custom/)
- [Dimensions](https://developer.wordpress.org/themes/global-settings-and-styles/settings/dimensions/)
- [Layout](https://developer.wordpress.org/themes/global-settings-and-styles/settings/layout/)
- [Lightbox](https://developer.wordpress.org/themes/global-settings-and-styles/settings/lightbox/)
- [Position](https://developer.wordpress.org/themes/global-settings-and-styles/settings/position/)
- [Shadow](https://developer.wordpress.org/themes/global-settings-and-styles/settings/shadow/)
- [Spacing](https://developer.wordpress.org/themes/global-settings-and-styles/settings/spacing/)
- [Typography](https://developer.wordpress.org/themes/global-settings-and-styles/settings/typography/)
- [Use Root Padding Aware Alignments](https://developer.wordpress.org/themes/global-settings-and-styles/settings/use-root-padding-aware-alignments/)
- [Settings Reference](https://developer.wordpress.org/themes/global-settings-and-styles/settings/settings-reference/)
+ [Styles](https://developer.wordpress.org/themes/global-settings-and-styles/styles/)
- [Applying Styles](https://developer.wordpress.org/themes/global-settings-and-styles/styles/applying-styles/)
- [Using Presets](https://developer.wordpress.org/themes/global-settings-and-styles/styles/using-presets/)
- [Styles Reference](https://developer.wordpress.org/themes/global-settings-and-styles/styles/styles-reference/)
+ [Custom Templates](https://developer.wordpress.org/themes/global-settings-and-styles/custom-templates/)
+ [Patterns](https://developer.wordpress.org/themes/global-settings-and-styles/patterns/)
+ [Template Parts](https://developer.wordpress.org/themes/global-settings-and-styles/template-parts/)
+ [Style Variations](https://developer.wordpress.org/themes/global-settings-and-styles/style-variations/)
* [Templates](https://developer.wordpress.org/themes/templates/)
+ [Introduction to Templates](https://developer.wordpress.org/themes/templates/introduction-to-templates/)
+ [Templates](https://developer.wordpress.org/themes/templates/templates/)
+ [Template Hierarchy](https://developer.wordpress.org/themes/templates/template-hierarchy/)
+ [Template Parts](https://developer.wordpress.org/themes/templates/template-parts/)
* [Patterns](https://developer.wordpress.org/themes/patterns/)
+ [Introduction to Patterns](https://developer.wordpress.org/themes/patterns/introduction-to-patterns/)
+ [Registering Patterns](https://developer.wordpress.org/themes/patterns/registering-patterns/)
+ [Using PHP in Patterns](https://developer.wordpress.org/themes/patterns/using-php-in-patterns/)
+ [Usage in Templates](https://developer.wordpress.org/themes/patterns/usage-in-templates/)
+ [Starter Patterns](https://developer.wordpress.org/themes/patterns/starter-patterns/)
+ [Block Type Patterns](https://developer.wordpress.org/themes/patterns/block-type-patterns/)
+ [Patterns and Block Locking](https://developer.wordpress.org/themes/patterns/patterns-and-block-locking/)
* [Features](https://developer.wordpress.org/themes/features/)
+ [Block Style Variations](https://developer.wordpress.org/themes/features/block-style-variations/)
+ [Block Stylesheets](https://developer.wordpress.org/themes/features/block-stylesheets/)
+ [Block Variations](https://developer.wordpress.org/themes/features/block-variations/)
+ [Block Patterns (Archived)](https://developer.wordpress.org/themes/features/block-patterns/)
* [Advanced Topics](https://developer.wordpress.org/themes/advanced-topics/)
+ [Internationalization](https://developer.wordpress.org/themes/advanced-topics/internationalization/)
+ [Child Themes](https://developer.wordpress.org/themes/advanced-topics/child-themes/)
+ [Build Process](https://developer.wordpress.org/themes/advanced-topics/build-process/)
+ [Privacy](https://developer.wordpress.org/themes/advanced-topics/privacy/)
+ [Testing](https://developer.wordpress.org/themes/advanced-topics/testing/)
+ [Debugging](https://developer.wordpress.org/themes/advanced-topics/debugging/)
+ [Security](https://developer.wordpress.org/themes/advanced-topics/security/)
+ [Publishing Themes](https://developer.wordpress.org/themes/advanced-topics/publishing-themes/)
+ [Theme Testing](https://developer.wordpress.org/themes/advanced-topics/theme-testing/)
+ [Plugin API Hooks](https://developer.wordpress.org/themes/advanced-topics/plugin-api-hooks/)
+ [UI Best Practices](https://developer.wordpress.org/themes/advanced-topics/ui-best-practices/)
+ [JavaScript Best Practices](https://developer.wordpress.org/themes/advanced-topics/javascript-best-practices/)
* [Releasing Your Theme](https://developer.wordpress.org/themes/releasing-your-theme/)
+ [Required Theme Files](https://developer.wordpress.org/themes/releasing-your-theme/required-theme-files/)
+ [Submitting Your Theme to WordPress.org](https://developer.wordpress.org/themes/releasing-your-theme/submitting-your-theme-to-wordpress-org/)
+ [Testing](https://developer.wordpress.org/themes/releasing-your-theme/testing/)
+ [Theme Review Guidelines](https://developer.wordpress.org/themes/releasing-your-theme/theme-review-guidelines/)
+ [Updating Your Theme](https://developer.wordpress.org/themes/releasing-your-theme/updating-your-theme/)
+ [Writing Documentation](https://developer.wordpress.org/themes/releasing-your-theme/writing-documentation/)
* [Classic themes](https://developer.wordpress.org/themes/classic-themes/)
+ [Your First Theme](https://developer.wordpress.org/themes/classic-themes/your-first-theme/)
+ [Theme Basics](https://developer.wordpress.org/themes/classic-themes/basics/)
- [Categories, Tags, & Custom Taxonomies](https://developer.wordpress.org/themes/classic-themes/basics/categories-tags-custom-taxonomies/)
- [Conditional Tags](https://developer.wordpress.org/themes/classic-themes/basics/conditional-tags/)
- [Including CSS & JavaScript (Archived)](https://developer.wordpress.org/themes/classic-themes/basics/including-css-javascript/)
- [Linking Theme Files & Directories](https://developer.wordpress.org/themes/classic-themes/basics/linking-theme-files-directories/)
- [Main Stylesheet (style.css)](https://developer.wordpress.org/themes/classic-themes/basics/main-stylesheet-style-css/)
- [Organizing Theme Files](https://developer.wordpress.org/themes/classic-themes/basics/organizing-theme-files/)
- [Post Types](https://developer.wordpress.org/themes/classic-themes/basics/post-types/)
- [Reworking Theme Files & Organization](https://developer.wordpress.org/themes/classic-themes/basics/reworking-theme-files-organization/)
- [Template Files](https://developer.wordpress.org/themes/classic-themes/basics/template-files/)
- [Template Hierarchy](https://developer.wordpress.org/themes/classic-themes/basics/template-hierarchy/)
- [Template Tags](https://developer.wordpress.org/themes/classic-themes/basics/template-tags/)
- [The Loop](https://developer.wordpress.org/themes/classic-themes/basics/the-loop/)
- [Theme Functions](https://developer.wordpress.org/themes/classic-themes/basics/theme-functions/)
- [Tools & Resources](https://developer.wordpress.org/themes/classic-themes/basics/tools-resources/)
+ [Theme Functionality](https://developer.wordpress.org/themes/classic-themes/functionality/)
- [404 Pages](https://developer.wordpress.org/themes/classic-themes/functionality/404-pages/)
- [Accessibility](https://developer.wordpress.org/themes/classic-themes/functionality/accessibility/)
- [Administration Menus](https://developer.wordpress.org/themes/classic-themes/functionality/administration-menus/)
- [Block theme accessibility](https://developer.wordpress.org/themes/classic-themes/functionality/block-theme-accessibility/)
- [Core-Supported Features](https://developer.wordpress.org/themes/classic-themes/functionality/core-supported/)
- [Custom Backgrounds](https://developer.wordpress.org/themes/classic-themes/functionality/custom-backgrounds/)
- [Custom Front Page Templates](https://developer.wordpress.org/themes/classic-themes/functionality/custom-front-page-templates/)
- [Custom Headers](https://developer.wordpress.org/themes/classic-themes/functionality/custom-headers/)
- [Custom Logo](https://developer.wordpress.org/themes/classic-themes/functionality/custom-logo/)
- [Featured Images & Post Thumbnails](https://developer.wordpress.org/themes/classic-themes/functionality/featured-images-post-thumbnails/)
- [Internationalization](https://developer.wordpress.org/themes/classic-themes/functionality/internationalization/)
- [Localization](https://developer.wordpress.org/themes/classic-themes/functionality/localization/)
- [Media](https://developer.wordpress.org/themes/classic-themes/functionality/media/)
* [Audio](https://developer.wordpress.org/themes/classic-themes/functionality/media/audio/)
* [Galleries](https://developer.wordpress.org/themes/classic-themes/functionality/media/galleries/)
* [Images](https://developer.wordpress.org/themes/classic-themes/functionality/media/images/)
* [Video](https://developer.wordpress.org/themes/classic-themes/functionality/media/video/)
- [Navigation Menus](https://developer.wordpress.org/themes/classic-themes/functionality/navigation-menus/)
- [Pagination](https://developer.wordpress.org/themes/classic-themes/functionality/pagination/)
- [Post Formats](https://developer.wordpress.org/themes/classic-themes/functionality/post-formats/)
- [Sidebars](https://developer.wordpress.org/themes/classic-themes/functionality/sidebars/)
- [Widgets](https://developer.wordpress.org/themes/classic-themes/functionality/widgets/)
- [Sticky Posts](https://developer.wordpress.org/themes/classic-themes/functionality/sticky-posts/)
+ [Theme Options – The Customize API](https://developer.wordpress.org/themes/classic-themes/customize-api/)
- [Customizer Objects](https://developer.wordpress.org/themes/classic-themes/customize-api/customizer-objects/)
- [Other Resources](https://developer.wordpress.org/themes/classic-themes/customize-api/other-resources/)
- [Tools for Improved User Experience](https://developer.wordpress.org/themes/classic-themes/customize-api/tools-for-improved-user-experience/)
- [The Customizer JavaScript API](https://developer.wordpress.org/themes/classic-themes/customize-api/the-customizer-javascript-api/)
- [JavaScript/Underscore.js-Rendered Custom Controls](https://developer.wordpress.org/themes/classic-themes/customize-api/javascriptunderscore-js-rendered-custom-controls/)
- [Advanced Usage](https://developer.wordpress.org/themes/classic-themes/customize-api/advanced-usage/)
+ [Template Files](https://developer.wordpress.org/themes/classic-themes/templates/)
- [Attachment Template Files](https://developer.wordpress.org/themes/classic-themes/templates/attachment-template-files/)
- [Custom Post Type Template Files](https://developer.wordpress.org/themes/classic-themes/templates/custom-post-type-template-files/)
- [Partial and Miscellaneous Template Files](https://developer.wordpress.org/themes/classic-themes/templates/partial-and-miscellaneous-template-files/)
* [Comment Template](https://developer.wordpress.org/themes/classic-themes/templates/partial-and-miscellaneous-template-files/comment-template/)
- [Post Template Files](https://developer.wordpress.org/themes/classic-themes/templates/post-template-files/)
- [Taxonomy Templates](https://developer.wordpress.org/themes/classic-themes/templates/taxonomy-templates/)
- [Page Templates](https://developer.wordpress.org/themes/classic-themes/templates/page-template-files/)
+ [References](https://developer.wordpress.org/themes/classic-themes/references/)
- [List of Template Tags](https://developer.wordpress.org/themes/classic-themes/references/list-of-template-tags/)
- [List of Conditional Tags](https://developer.wordpress.org/themes/classic-themes/references/list-of-conditional-tags/)
* [Credits](https://developer.wordpress.org/themes/credits/)
+ [Feedback](https://developer.wordpress.org/themes/credits/feedback/)
* [About](https://wordpress.org/about/)
* [News](https://wordpress.org/news/)
* [Hosting](https://wordpress.org/hosting/)
* [Privacy](https://wordpress.org/about/privacy/)
* [Showcase](https://wordpress.org/showcase/)
* [Themes](https://wordpress.org/themes/)
* [Plugins](https://wordpress.org/plugins/)
* [Patterns](https://wordpress.org/patterns/)
* [Learn](https://learn.wordpress.org/)
* [Documentation](https://wordpress.org/documentation/)
* [Developers](https://developer.wordpress.org/)
* [WordPress.tv ↗](https://wordpress.tv/)
* [Get Involved](https://make.wordpress.org/)
* [Events](https://events.wordpress.org/)
* [Donate ↗](https://wordpressfoundation.org/donate/)
* [Five for the Future](https://wordpress.org/five-for-the-future/)
* [WordPress.com ↗](https://wordpress.com/?ref=wporg-footer)
* [Matt ↗](https://ma.tt/)
* [bbPress ↗](https://bbpress.org/)
* [BuddyPress ↗](https://buddypress.org/)
* [Visit our X (formerly Twitter) account](https://www.x.com/WordPress)
* [Visit our Bluesky account](https://bsky.app/profile/wordpress.org)
* [Visit our Mastodon account](https://mastodon.world/%40WordPress)
* [Visit our Threads account](https://www.threads.net/%40wordpress)
* [Visit our Facebook page](https://www.facebook.com/WordPress/)
* [Visit our Instagram account](https://www.instagram.com/wordpress/)
* [Visit our LinkedIn account](https://www.linkedin.com/company/wordpress)
* [Visit our TikTok account](https://www.tiktok.com/%40wordpress)
* [Visit our YouTube channel](https://www.youtube.com/wordpress)
* [Visit our Tumblr account](https://wordpress.tumblr.com/)
The WordPress® trademark is the intellectual property of the WordPress Foundation.
