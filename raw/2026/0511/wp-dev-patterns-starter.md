Starter Patterns – Theme Handbook | Developer.WordPress.org
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
Starter Patterns
* [Developer Blog](https://developer.wordpress.org/news/)
* [Code Reference](https://developer.wordpress.org/reference/)
* [WP-CLI Commands](https://developer.wordpress.org/cli/commands/)
* [Developer Blog](https://developer.wordpress.org/news/)
* [Code Reference](https://developer.wordpress.org/reference/)
* [WP-CLI Commands](https://developer.wordpress.org/cli/commands/)
[Home](https://developer.wordpress.org)[Theme Handbook](https://developer.wordpress.org/themes/)[Patterns](https://developer.wordpress.org/themes/patterns/)Starter Patterns
Search
# Starter Patterns
## In this article
Table of Contents
* [Starter page patterns](#starter-page-patterns)
+ [How to create a page pattern](#how-to-create-a-page-pattern)
* [Starter template patterns](#starter-template-patterns)
+ [How to create template type patterns](#how-to-create-template-type-patterns)
+ [Supported template types](#supported-template-types)
+ [Using template patterns in templates](#using-template-patterns-in-templates)
[↑ Back to top](#wp--skip-link--target)
The WordPress block editor is powerful and can handle many different layouts and designs. However, when building a theme, you must remember that not all of your users will be skilled designers or even have much interest in piecing together their own layouts. This is where starter patterns can be beneficial to them.
WordPress supports two types of starter patterns for pages (or any post type) and templates. This feature lets you create starting points for your theme users to build out their pages and templates with minimal skills.
## [Starter page patterns](#starter-page-patterns)
Page patterns let you create custom patterns your theme users can access when adding a new page via **Pages > Add New** in their WordPress admin. From there, a modal will pop up and show them a selection of patterns if any are registered.
Here is what the screen looks like when creating a new page with the default Twenty Twenty-Four theme installed:
This is a powerful feature because it means that you can provide well-designed starting points for your theme’s users or clients. And they don’t need to build from scratch. They only need to select a pattern, and it will automatically be inserted into the content area. From there, they can make any customizations they want.
### [How to create a page pattern](#how-to-create-a-page-pattern)
Technically, any pattern in your theme can be converted to a page pattern. All you need to do is define the correct parameters to mark it as such. As described in [Registering Patterns](https://developer.wordpress.org/themes/patterns/registering-patterns/), there are two available file header fields that you must set to make this happen:
* **`Block Types`:** Adding `core/post-content` as one of the block types for the pattern tells WordPress that the pattern should be used for the post content.
* **`Post Types`:** You can add one or more post types (separated by commas) to connect the pattern.
With these two fields combined, you create a starter page pattern. Here is what the file header looks like for a fictional `/patterns/example-page.php` pattern file:
```
php
/**
* Title: Example Page
* Slug: themeslug/example-page
* Categories: page
* Block Types: core/post-content
* Post Types: page
* Viewport width: 1376
*/
?
```
And that’s literally all you must do. Define the pattern’s `Block Types` and `Post Types` parameters and you have a starter page pattern.
This will work with any post type that has opted into the block editor, including both the default `post` and `page` post types.
Because page patterns are actually tied to the content, you wouldn’t typically include something like a site header or footer here. The pattern is output as post content.
## [Starter template patterns](#starter-template-patterns)
Like page patterns, template patterns give users a starting point when building a new template. The difference is that template patterns work from the Site Editor.
If you visit **Appearance > Editor > Templates** in your WordPress admin and click the **+** icon button for creating a new template, you should see a new **Add template** modal:
From there, you can select the template that you want to create. If the template type has any patterns registered for it, a new **Choose a pattern** modal will appear, overlaying the template-editing interface.
For example, when choosing the **Front Page** option with the default Twenty Twenty-Four theme, you will see this:
This gives your theme users a really nice onramp for building templates. It means they don’t have to create everything from the ground up and can get started down the right path.
### [How to create template type patterns](#how-to-create-template-type-patterns)
Unlike page patterns, you will usually not make any of your patterns a template pattern. These are starting points for an entire template, so their use cases are much more limited and specific.
Because template patterns represent an entire template, you would typically include global elements like the header, footer, sidebar, and other sections that your theme’s templates display.
As noted in [Registering Patterns](https://developer.wordpress.org/themes/patterns/registering-patterns/), there is a single required header field needed for your template pattern (and one optional field):
* **`Template Types`:** One or more template types, separated by comma, that the pattern should be associated with.
* **`Inserter`:** *(Optional)* Often, you will not want template patterns to be available via the inserter. To disable this, set it to `no` or `false`.
Suppose that you created a pattern that would work well for the Front Page or Home templates. Here is what a fictional `/patterns/home-template.php` file would look like:
```
php
/**
* Title: Home Template
* Slug: themeslug/home-template
* Template Types: front-page, home
* Viewport width: 1376
* Inserter: no
*/
?
```
From that point, anytime a user attempted to create a new Front Page or Home template from the WordPress Site Editor, they would be presented with your template pattern as a starting point.
### [Supported template types](#supported-template-types)
The following list includes the templates that you can define via the `Template Types` field, but you can always reference the [Template Hierarchy](https://developer.wordpress.org/themes/templates/template-hierarchy/) documentation for a full overview of templates:
* `index`
* `home`
* `front-page`
* `singular`
* `single`
* `page`
* `archive`
* `author`
* `category`
* `taxonomy`
* `date`
* `tag`
* `attachment`
* `search`
* `privacy-policy`
* `404`
### [Using template patterns in templates](#using-template-patterns-in-templates)
When creating custom template patterns, it also makes sense to reuse those patterns within your templates. *Why rewrite code?*
Imagine that you created a template pattern that would work as both a Home and Index template. It would look like this:
```
php
/**
* Title: Index Template
* Slug: themeslug/index-template
* Template Types: home, index
* Viewport width: 1376
* Inserter: no
*/
?
```
Now suppose that you included a `/templates/index.html` template in your theme. Instead of adding the code in two places, you can simply call the pattern from the template file:
To learn more about including patterns in templates, check out the [Usage in Templates](https://developer.wordpress.org/themes/patterns/usage-in-templates/) documentation.
First published
April 30, 2024
[Previous
Usage in Templates
Previous: Usage in Templates](https://developer.wordpress.org/themes/patterns/usage-in-templates/)
[Next
Block Type Patterns
Next: Block Type Patterns](https://developer.wordpress.org/themes/patterns/block-type-patterns/)
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
