Registering Patterns – Theme Handbook | Developer.WordPress.org
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
Registering Patterns
* [Developer Blog](https://developer.wordpress.org/news/)
* [Code Reference](https://developer.wordpress.org/reference/)
* [WP-CLI Commands](https://developer.wordpress.org/cli/commands/)
* [Developer Blog](https://developer.wordpress.org/news/)
* [Code Reference](https://developer.wordpress.org/reference/)
* [WP-CLI Commands](https://developer.wordpress.org/cli/commands/)
[Home](https://developer.wordpress.org)[Theme Handbook](https://developer.wordpress.org/themes/)[Patterns](https://developer.wordpress.org/themes/patterns/)Registering Patterns
Search
# Registering Patterns
## In this article
Table of Contents
* [Creating patterns](#creating-patterns)
* [Registering patterns](#registering-patterns)
+ [Using the /patterns directory to register patterns](#using-the-patterns-directory-to-register-patterns)
+ [Using PHP to register patterns](#using-php-to-register-patterns)
+ [Conditionally registering patterns](#conditionally-registering-patterns)
* [Unregistering patterns](#unregistering-patterns)
+ [Removing Core patterns](#removing-core-patterns)
+ [Disabling remote patterns](#disabling-remote-patterns)
* [Pattern categories](#pattern-categories)
+ [Registering a pattern category](#registering-a-pattern-category)
+ [Unregistering a pattern category](#unregistering-a-pattern-category)
[↑ Back to top](#wp--skip-link--target)
In [Introduction to Patterns](https://developer.wordpress.org/themes/patterns/introduction-to-patterns/), you learned how to create patterns from the WordPress admin interface. Now it’s time to learn how to add these custom patterns directly to your theme
This article will show you how to take a pattern’s block code and register it with WordPress from within your theme. It will also cover unregistering patterns and registering/unregistering pattern categories.
## [Creating patterns](#creating-patterns)
If you’re unfamiliar with creating custom patterns, take a moment to study the [Introduction to Patterns](https://developer.wordpress.org/themes/patterns/registering-patterns/) guide, which will walk you through the process.
Your pattern can be anything you like, but for the purposes of this article, the code shared below will be of a core Cover block with some blocks nested within it, following this structure:
* Group
+ Heading
+ Paragraph
+ Buttons
- Button
Feel free to try recreating this yourself from the editor, especially if you are just getting used to working with the editor. Here is a screenshot of the pattern from the editor:
You can also customize this however you like, but it’s recommended to keep this relatively simple for your first time creating a pattern.
Here is what the above pattern’s block markup looks like (read the [Introduction to Patterns](https://developer.wordpress.org/themes/patterns/introduction-to-patterns/) article for more information on getting pattern code):
```
## Welcome to My Site
This is my little home away from home. Here, you will get to know me. I'll share my likes, hobbies, and more. Every now and then, I'll even have something interesting to say in a blog post.
See My Popular Posts →︎
```
For the remainder of this article, you will work with this same pattern, learning how to register it, unregister it, and customize it further.
It’s good practice to wrap most patterns in a Group, Cover, or other container block (i.e., a block that allows nested blocks). This makes it easier for your theme users to move the entire pattern around in the editor. You can also add a CSS class to this outer block for any custom styling that applies to the entire pattern.
## [Registering patterns](#registering-patterns)
There are two methods for registering block patterns in WordPress:
* By placing files with block markup in them into the /patterns folder in your theme.
* By manually calling the `register_block_pattern()` function.
The most straightforward route is the first. You’ll learn how to do both in the next sections, but this handbook will recommend using the `/patterns` folder except in some edge cases.
### [Using the /patterns directory to register patterns](#using-the-patterns-directory-to-register-patterns)
WordPress will recognize any pattern file placed in your theme’s `/patterns` folder, and it will automatically register it for you, provided that it has a valid pattern file header.
A [file header](https://codex.wordpress.org/File_Header) is PHP-commented code with a list of key + value pairs at the top of a file. WordPress parses this information to gather metadata. In this case, the metadata is used to register a block pattern.
For pattern file headers, these are the keys that you can set:
* **`Title`:** A human-readable title that can be translated.
* **`Slug`:** A namespaced slug that is unique to your pattern in the form of `namespace/pattern-name`.
* **`Categories`:** A comma-separated list of categories in which the pattern belongs.
* **`Description`:** The long description of the registered pattern. Only shown to screen readers.
* **`Viewport Width`:** The width of the viewport when previewing the pattern (in pixels).
* **`Inserter`:** Whether to show the pattern in the inserter. Defaults to `true`.
* **`Keywords`:** A comma-separated list of keywords related to the pattern. Used when a user searches in the inserter.
* **`Block Types`:** A comma-separated list of block types to associate the pattern with (e.g., `core/cover`, `core/post-content`).
* **`Post Types`:** A comma-separated list of post types in which to limit the pattern (e.g., `post`, `page`). Defaults to all post types.
* **`Template Types`:** A comma-separated list of template types the pattern fits with (e.g., `home`, `single`).
For most patterns, you should at least add the Title, Slug, and Categories fields. Pattern files should look like this:
```
php
/**
* Title: Hero
* Slug: themeslug/hero
* Categories: featured
*/
?
```
Now let’s add the code that you copied from the previous section into the `/patterns/hero.php` file in your theme:
```
php
/**
* Title: Hero
* Slug: themeslug/hero
* Categories: featured
*/
?
## Welcome to My Site
This is my little home away from home. Here, you will get to know me. I'll share my likes, hobbies, and more. Every now and then, I'll even have something interesting to say in a blog post.
See My Popular Posts →︎
```
Now if you add or edit a new post in WordPress, you should see your pattern listed in the inserter. Click the **Patterns** tab and select **Featured**:
You should also be able to see your pattern listed under **Appearance > Editor > Patterns** in the WordPress admin.
### [Using PHP to register patterns](#using-php-to-register-patterns)
Instead of using the `/patterns` folder to auto-register your patterns, you may choose to manually register them via PHP. You can mix and match both registration methods, but an individual pattern must be registered with only one method.
To register block patterns with PHP, you must use the [`register_block_pattern()`](https://developer.wordpress.org/reference/functions/register_block_pattern/) function. Here is what its signature looks like:
```
register_block_pattern(
string $pattern_name,
array $pattern_properties
): bool
```
The function accepts two parameters and returns `true` or `false`, depending on whether the pattern was successfully registered:
* **`$pattern_name`:** A namespaced slug that is unique to your pattern in the form of `namespace/pattern-name`.
* **`$pattern_properties`:**
+ **`content`:** The block markup for the pattern.
+ **`title`:** A human-readable title that can be translated.
+ **`categories`:** An array of categories in which the pattern belongs.
+ **`description`:** The long description of the registered pattern. Only shown to screen readers.
+ **`viewportWidth`:** The width of the viewport when previewing the pattern (in pixels).
+ **`inserter`:** Whether to show the pattern in the inserter. Defaults to `true`.
+ **`keywords`:** An array of keywords related to the pattern. Used when a user searches in the inserter.
+ **`blockTypes`:** An array of block types to associate the pattern with.
+ **`postTypes`:** An array of post types in which to limit the pattern. Defaults to all post types.
+ **`source`:** The source for the registered pattern. This should be set to `theme` since the pattern is coming from a theme.
+ **`templateTypes`:** An array of template types the pattern fits with.
When registering patterns via PHP, you should do so on the `init` hook. Now try registering your custom pattern by adding this code in `functions.php` (be sure to add your custom markup to the `content` property):
```
add_action( 'init', 'themeslug_register_patterns' );
function themeslug_register_patterns() {
register_block_pattern( 'themeslug/hero', array(
'title' => __( 'Hero', 'themeslug' ),
'categories' => array( 'featured' ),
'source' => 'theme',
'content' => ''
) );
}
```
### [Conditionally registering patterns](#conditionally-registering-patterns)
If you need to conditionally register an entire pattern, there are two methods to choose from, depending on how you prefer to register patterns.
#### [Conditionally registering patterns in the /patterns folder](#conditionally-registering-patterns-in-the-patterns-folder)
If you have used the auto-registration method of placing your pattern in your theme’s `/patterns` folder, there is no true way to conditionally register it. What you must do in this case is unregister the pattern.
Suppose that you wanted to only make the hero pattern you registered earlier available if the `core/paragraph` block is registered. In a real-world scenario, you’d likely be checking for a third-party block, and the `core/paragraph` block is used only for the sake of this example.
Try this code inside of your `functions.php` file to unregister your previously-registered pattern:
```
add_action( 'init', 'themeslug_unregister_patterns', 999 );
function themeslug_unregister_patterns() {
if ( WP_Block_Type_Registry::get_instance()->is_registered( 'core/paragraph' ) ) {
unregister_block_pattern( 'themeslug/hero' );
}
}
```
The above code uses the [`WP_Block_Type_Registry`](https://developer.wordpress.org/reference/classes/wp_block_type_registry/) class, which stores all block types that are registered on the server-side with WordPress. Using the class’s [`is_registered()`](https://developer.wordpress.org/reference/classes/wp_block_type_registry/is_registered/) method, you can determine whether a block is registered.
#### [Conditionally registering patterns via register\_block\_pattern()](#conditionally-registering-patterns-via-register_block_pattern)
If you manually registered your patterns via PHP, you only need to wrap your call to `register_block_pattern()` with your conditional check.
Using the same example as above, try only registering your pattern if the `core/paragraph` block is registered by adding this to `functions.php`:
```
add_action( 'init', 'themeslug_register_patterns', 999 );
function themeslug_register_patterns() {
if ( WP_Block_Type_Registry::get_instance()->is_registered( 'core/paragraph' ) ) {
register_block_pattern( 'themeslug/hero', array(
'title' => __( 'Hero', 'themeslug' ),
'categories' => array( 'featured' ),
'source' => 'theme',
'content' => ''
) );
}
}
```
## [Unregistering patterns](#unregistering-patterns)
There are times when you need to unregister block patterns so that they do not appear in the inserter for users. For example, you may want to remove some of the core WordPress patterns, those added by a parent theme (if you’re building a child theme), or those added by third-party plugins.
When you need to unregister a single block pattern, you’ll use the [`unregister_block_pattern()`](https://developer.wordpress.org/reference/functions/unregister_block_pattern/) function:
```
unregister_block_pattern( string $pattern_name ): bool
```
Like when registering patterns, you’ll also want to unregister on the `init` hook. Because patterns are generally (but not always) registered with the default priority `10` on `init`, you’ll want to use a higher priority so that your code runs later.
Try unregistering your custom pattern from earlier by adding this code to your `functions.php` file:
```
add_action( 'init', 'themeslug_unregister_patterns', 999 );
function themeslug_unregister_patterns() {
unregister_block_pattern( 'themeslug/hero' );
}
```
### [Removing Core patterns](#removing-core-patterns)
While you can use `unregister_block_pattern()` to unregister individual core WordPress patterns, you can also choose to get rid of all of them by removing support for the core-block-patterns feature.
To remove all core WordPress patterns, add this code to your theme’s `functions.php` file:
```
add_action( 'after_setup_theme', 'themeslug_remove_core_patterns' );
function themeslug_remove_core_patterns() {
remove_theme_support( 'core-block-patterns' );
}
```
### [Disabling remote patterns](#disabling-remote-patterns)
WordPress also automatically loads some patterns remotely from the official [Pattern Directory](https://wordpress.org/patterns/) on WordPress.org. There are some situations where you might not want to load these patterns, such as when they don’t match your theme design. You may also want to opt out of calling a remote API on a site that you’re building for a client. Whatever the case, you can disable this by filtering the [`should_load_remote_block_patterns`](https://developer.wordpress.org/reference/hooks/should_load_remote_block_patterns/) hook.
To disable remote patterns, add this code to your `functions.php` file:
```
add_filter( 'should_load_remote_block_patterns', '__return_false' );
```
## [Pattern categories](#pattern-categories)
WordPress has several block pattern categories that it registers by default:
* `featured`
* `about`
* `audio` (added in WordPress 6.4)
* `banner`
* `buttons`
* `call-to-action`
* `columns`
* `contact`
* `footer`
* `gallery`
* `header`
* `media`
* `portfolio`
* `posts`
* `query` (use `posts` instead)
* `services`
* `team`
* `testimonials`
* `text`
* `video` (added in WordPress 6.4)
It’s generally best to use these default categories for your theme. This keeps a consistent UI that users will be accustomed to. But there are times when you may want to add your own categories or even remove those that are already registered.
In this section of the article, you’ll learn how to both register and unregister custom block pattern categories.
### [Registering a pattern category](#registering-a-pattern-category)
To register a custom category for putting one or more of your patterns into, you must use the [`register_block_pattern_category()`](https://developer.wordpress.org/reference/functions/register_block_pattern_category/) function:
```
register_block_pattern_category(
string $category_name,
array $category_properties
): bool
```
The function accepts two parameters:
* **`$category_name`:** A unique ID/slug for the category. It’s good practice to prefix this with your theme slug (e.g., `themeslug-category-name`).
* **`$category_properties`:** An array of properties for defining further data about the category:
+ **`label`:** A human-readable label/title for the category that may be translated.
+ **`description`:** A description for the category that may be translated.
Try adding a new custom pattern category for your theme. Add this code to your theme’s `functions.php` file:
```
add_action( 'init', 'themeslug_register_pattern_categories' );
function themeslug_register_pattern_categories() {
register_block_pattern_category( 'themeslug/custom', array(
'label' => __( 'Theme Name: Custom', 'themeslug' ),
'description' => __( 'Custom patterns for Theme Name.', 'themeslug' )
) );
}
```
Now try adding the `themeslug/custom` category to any block pattern:
```
php
/**
* Title: Hero
* Slug: themeslug/hero
* Categories: featured, themeslug/custom
*/
?
```
Any registered patterns for this category will now appear in the inserter and under **Appearance > Editor > Patterns** (**Appearance > Patterns** for classic themes):
### [Unregistering a pattern category](#unregistering-a-pattern-category)
To unregister a registered pattern category, you must use the [`unregister_block_pattern_category()`](https://developer.wordpress.org/reference/functions/unregister_block_pattern_category/) function:
```
unregister_block_pattern_category( string $category_name ): bool
```
The function accepts a single parameter:
* **`$category_name`:** The name (i.e., slug) of the registered category to unregister.
Try adding this code to your `functions.php` to remove the `themeslug/custom` category you registered in the previous step:
```
add_action( 'init', 'themeslug_unregister_pattern_categories' );
function themeslug_unregister_pattern_categories() {
register_block_pattern_category( 'themeslug/custom' );
}
```
First published
April 30, 2024
Last updated
July 15, 2024
[Previous
Introduction to Patterns
Previous: Introduction to Patterns](https://developer.wordpress.org/themes/patterns/introduction-to-patterns/)
[Next
Using PHP in Patterns
Next: Using PHP in Patterns](https://developer.wordpress.org/themes/patterns/using-php-in-patterns/)
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
