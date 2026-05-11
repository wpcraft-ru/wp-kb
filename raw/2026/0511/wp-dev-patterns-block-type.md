Block Type Patterns – Theme Handbook | Developer.WordPress.org
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
Block Type Patterns
* [Developer Blog](https://developer.wordpress.org/news/)
* [Code Reference](https://developer.wordpress.org/reference/)
* [WP-CLI Commands](https://developer.wordpress.org/cli/commands/)
* [Developer Blog](https://developer.wordpress.org/news/)
* [Code Reference](https://developer.wordpress.org/reference/)
* [WP-CLI Commands](https://developer.wordpress.org/cli/commands/)
[Home](https://developer.wordpress.org)[Theme Handbook](https://developer.wordpress.org/themes/)[Patterns](https://developer.wordpress.org/themes/patterns/)Block Type Patterns
Search
# Block Type Patterns
## In this article
Table of Contents
* [Block-specific patterns](#block-specific-patterns)
+ [Register a block type pattern](#register-a-block-type-pattern)
+ [Build a block type pattern](#build-a-block-type-pattern)
* [Query Loop patterns](#query-loop-patterns)
* [Template Part patterns](#template-part-patterns)
[↑ Back to top](#wp--skip-link--target)
One of the most useful capabilities of patterns is the ability to tie them directly to one or more block types. For example, you can assign a gallery pattern to the Gallery (`core/gallery`) block type and use that specific pattern whenever you insert a new Gallery.
But the feature offers even more than that. In this article, you will learn how to add a basic connection between a block type and a pattern and how to take advantage of some special cases in WordPress.
Besides the techniques described below, block type patterns are also needed for creating starter page patterns, which you can learn more about in the [Starter Patterns](https://developer.wordpress.org/themes/patterns/starter-patterns/) documentation.
## [Block-specific patterns](#block-specific-patterns)
It’s important to note that patterns that are connected to a block type behave just like other patterns. What you’re doing is making WordPress aware that the pattern is meant for one or more block types.
For many cases, you may simply want to create a pattern that serves as a starting point when inserting a specific block, and that’s where block type patterns are often useful.
### [Register a block type pattern](#register-a-block-type-pattern)
As described in [Registering Patterns](https://developer.wordpress.org/themes/patterns/registering-patterns/), you must assign a value to the `Block Types` header field in your pattern file. This field can accept one or more comma-separated block types, which must include the block’s namespace and slug (e.g., `namespace/slug`).
Here is what the file header of pattern tied to the Cover block might look like:
```
php
/**
* Title: Cover With Contrast Background
* Slug: themeslug/cover-contrast
* Categories: banner
* Block Types: core/cover
* Viewport Width: 1376
*/
?
```
### [Build a block type pattern](#build-a-block-type-pattern)
Let’s take what you’ve learned so far and create a pattern for a specific block. In this example, you will create a simple Cover block with a single nested Heading and a background with the `contrast` [color preset](https://developer.wordpress.org/themes/global-settings-and-styles/settings/color/).
Create a new file named `cover-contrast.php` in your theme’s `/patterns` folder and paste this code inside it:
```
php
/**
* Title: Cover With Contrast Background
* Slug: themeslug/cover-contrast
* Categories: banner
* Block Types: core/cover
* Viewport Width: 1376
*/
?
## php esc_html_e( 'A simple heading', 'themeslug' ); ?
```
Once you’ve saved your pattern file, it should appear as normal in the inserter and Pattern Library. But the most important thing is to check that it’s working as a block type pattern.
Open a new post or page in your WordPress admin and insert a Cover block but don’t make any change to it. Then, click the Cover block’s icon in the toolbar. You should see a **Patterns** menu item, and if you click that, your new pattern will appear:
From there, if you select the pattern that you’ve registered, it will transform your inserted Cover block inside the content area:
And that is the basics of how block type patterns work. But WordPress has some special handling for some blocks, which you will learn about below.
## [Query Loop patterns](#query-loop-patterns)
The Query Loop block is one of the most unique and useful blocks to create custom patterns for. It gives you and your theme users the ability to quickly insert or replace existing Query blocks with an alternative posts layout.
The technique is the same as other block type patterns. You must add the `core/query` block type to the pattern’s `Block Types` file header field.
Let’s create a basic two-column grid that shows each post’s featured image, title, excerpt and date. You can, of course, use whatever layout and design that you want.
For now, create a new `query-two-columns.php` file in your theme’s `/patterns` folder and paste this code into it:
```
php
/**
* Title: Query: Two Columns
* Slug: themeslug/query-two-columns
* Categories: posts
* Block Types: core/query
* Viewport Width: 1376
*/
?
```
There are two ways to take advantage of Query Loop patterns, like the one you just created. The first is to insert a new Query Loop block into a page or template:
You should see a couple of buttons for adding a Query Loop from that point. Click the **Choose** button.
This will bring up the **Choose a pattern** modal. In this modal, your pattern will appear and can be selected:
Once selected, your pattern will be inserted into the canvas of the page or template that you are editing:
The second method of using Query Loop patterns is much the same. You can replace an existing pattern (even the one you just inserted) by clicking the **Replace** button in the toolbar. It will bring up the same **Choose a pattern** modal, and the process of choosing a pattern is the same.
## [Template Part patterns](#template-part-patterns)
Because [template parts](https://developer.wordpress.org/themes/templates/template-parts/) are technically blocks themselves, you can also use them as a pattern’s block types. There is one limitation: only parts that use the Header and Footer [template part areas](https://developer.wordpress.org/themes/templates/template-parts/#template-part-areas) are supported. But these are the most common types anyway.
When targeting a specific template part, its block type name will have a third component: the template part area. So if you want to target the Header part, you will pass a block type of `core/template-part/header`. And for the Footer part, use `core/template-part/footer`.
Let’s create a basic Header template part with a centered logo, title, and nav menu. You will use this to replace the existing header on your site.
Create a new file named `header-centered.php` in your theme’s `/patterns` folder. Then copy and paste this code into it:
```
php
/**
* Title: Header: Centered
* Slug: themeslug/header-centered
* Categories: header
* Block Types: core/template-part/header
* Viewport Width: 1376
*/
?
```
Once you save this file, go to **Appearance > Editor** in your WordPress admin and select the on the Header block.
Now click the **Options** button (**⋮** icon) in the toolbar. You should see a **Replace** option in the dropdown:
Once you click the **Replace** dropdown menu item, a new modal titled **Choose a header** will appear, and you should see your **Header: Centered** pattern in the available patterns grid:
By selecting this pattern or another, you can replace the entire Header template part. This process works the same for Footer template parts.
Parts for custom [template part areas](https://developer.wordpress.org/themes/templates/template-parts/#template-part-areas) cannot yet take advantage of this feature. There is an [open ticket](https://github.com/WordPress/gutenberg/issues/44689) that has not been patched to handle areas other than Header and Footer.
First published
April 30, 2024
[Previous
Starter Patterns
Previous: Starter Patterns](https://developer.wordpress.org/themes/patterns/starter-patterns/)
[Next
Patterns and Block Locking
Next: Patterns and Block Locking](https://developer.wordpress.org/themes/patterns/patterns-and-block-locking/)
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
