Patterns and Block Locking – Theme Handbook | Developer.WordPress.org
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
Patterns and Block Locking
* [Developer Blog](https://developer.wordpress.org/news/)
* [Code Reference](https://developer.wordpress.org/reference/)
* [WP-CLI Commands](https://developer.wordpress.org/cli/commands/)
* [Developer Blog](https://developer.wordpress.org/news/)
* [Code Reference](https://developer.wordpress.org/reference/)
* [WP-CLI Commands](https://developer.wordpress.org/cli/commands/)
[Home](https://developer.wordpress.org)[Theme Handbook](https://developer.wordpress.org/themes/)[Patterns](https://developer.wordpress.org/themes/patterns/)Patterns and Block Locking
Search
# Patterns and Block Locking
## In this article
Table of Contents
* [What is Block Locking?](#what-is-block-locking)
* [Block locking](#block-locking)
+ [Disable block movement](#disable-block-movement)
+ [Prevent block removal](#prevent-block-removal)
* [Template locking](#template-locking)
+ [Locking all nested blocks](#locking-all-nested-blocks)
+ [Preventing new blocks from being inserted](#preventing-new-blocks-from-being-inserted)
+ [Applying content-only editing](#applying-content-only-editing)
[↑ Back to top](#wp--skip-link--target)
The [Block Locking API](https://developer.wordpress.org/block-editor/how-to-guides/curating-the-editor-experience/block-locking/) is a powerful set of features that lets you lock the behavior of blocks and how users interact with them. You can control whether users can remove, move, or customize blocks, which lets you further curate the editing experience of your themes.
This article will explore some of the ways that you can utilize the API within your theme’s patterns. But for a deeper dive into the API, please refer to the [Block Locking documentation](https://developer.wordpress.org/block-editor/how-to-guides/curating-the-editor-experience/block-locking/) in the Block Editor Handbook.
You are not only limited to using Block Locking in patterns. You can also apply the methods in this article to [templates](https://developer.wordpress.org/themes/templates/).
## [What is Block Locking?](#what-is-block-locking)
The Block Locking API lets you control how a user interacts with blocks on their site. The types of locking you can do are:
* Disabling block movement in the editor.
* Preventing a block from being removed.
* Preventing new blocks from being inserted.
* Disabling editing block settings other than content and media.
This can be really useful depending on the scenario. For example, it often makes sense to lock a site header pattern, which can be complex, by disabling the deletion of its inner blocks by accident.
There are also two types of locking the API offers: block and template. Locking at the block level means preventing the user from taking some actions for the individual block. Locking at the template level means preventing the user from some actions at a “group” level.
It’s important to note that “template” here refers to a section of blocks nested within a container block, such as Group, Cover, etc. It’s not referring specifically to template files.
## [Block locking](#block-locking)
In this section, you will learn how to lock individual blocks. Namely, preventing a user from moving a block or removing it altogether.
First, you should familiarize yourself with how to lock blocks from the UI. Let’s start by creating a custom “event” pattern to work for throughout the remainder of this article.
Create a new file in your theme named `/patterns/event.php` and place the following code into it:
```
php
/**
* Title: Event
* Slug: themeslug/event
* Categories: banner
* Viewport Width: 1376
*/
?
![Image of a woman being carried through the air by swans.](https://s.w.org/patterns/files/2021/06/Group-17-scaled.jpg)
php esc_html_e( 'Location:', 'themeslug' ); ?
php esc_html_e( '82 Main St. Brooklyn, NY', 'themeslug' ); ?
php esc_html_e( 'Date:', 'themeslug' ); ?
php esc_html_e( 'October 24, 2021', 'themeslug' ); ?
php esc_html_e( 'Purchase Tickets' ); ?
```
Then go to the post editor and insert your new pattern. Once you’ve done that, you should see the pattern in the content area.
Now select the Image block (or any block) in the pattern. Then click the **Options** button in the block toolbar. You should see a **Lock** option:
This will bring up a modal for various locking options, which you’ll learn about in the following sections.
This type of locking is shown in the UI, and a theme user will be able to override it from the UI. For publicly-distributed themes, this is a good thing. But for client work, this type of access may be undesirable. If you need to control permissions to the locking UI, see the instructions in the Locking API docs: [Change permissions to control locking ability](https://developer.wordpress.org/block-editor/how-to-guides/curating-the-editor-experience/block-locking/#change-permissions-to-control-locking-ability).
### [Disable block movement](#disable-block-movement)
One of the primary use cases of the Block Locking API is to prevent users from accidentally moving blocks around within a pattern, altering the layout. In the **Lock** modal, you will see an option to **Disable movement**. Try selecting this option for the Image block from the event pattern:
Under the hood, when you select this option, WordPress will add a new `lock` attribute to the block markup that looks like this:
Because you’ve selected the **Disable movement** option, the `move` key will have a value of `true` (i.e., movement has been locked).
Now try creating a new version of your pattern with the Image block’s movement disabled. Add a new `/patterns/event-disable-movement.php` file to your theme and paste this code in:
```
php
/**
* Title: Event (Disable Movement)
* Slug: themeslug/event-disable-movement
* Categories: banner
* Viewport Width: 1376
*/
?
![Image of a woman being carried through the air by swans.](https://s.w.org/patterns/files/2021/06/Group-17-scaled.jpg)
php esc_html_e( 'Location:', 'themeslug' ); ?
php esc_html_e( '82 Main St. Brooklyn, NY', 'themeslug' ); ?
php esc_html_e( 'Date:', 'themeslug' ); ?
php esc_html_e( 'October 24, 2021', 'themeslug' ); ?
php esc_html_e( 'Purchase Tickets' ); ?
```
Now, anytime a user adds this pattern to their content, they won’t be able to move the Image block within the pattern.
Just because the Image block’s movement has been disabled, it doesn’t affect other blocks within the pattern. For example, it’s still possible to move the Columns block. You would need to disable both blocks from being moved to stop the user from moving either.
### [Prevent block removal](#prevent-block-removal)
For some patterns, you will want to keep a block from being deleted altogether. For example, in the event pattern, the Image block may be integral to the pattern’s design.
Starting from the original event pattern, repeat the process of opening the **Lock** modal. This time, select the **Prevent removal** option:
Again, WordPress will automatically insert the `lock` attribute in the block markup, which will look like this:
This time, the `remove` option is set to `true` (i.e., removal has been locked).
Now create an alternate version of your pattern with the Image block’s removal disabled. Add a new `/patterns/event-prevent-removal.php` file to your theme and paste this code in:
```
php
/**
* Title: Event (Prevent Removal)
* Slug: themeslug/event-prevent-removal
* Categories: banner
* Viewport Width: 1376
*/
?
![Image of a woman being carried through the air by swans.](https://s.w.org/patterns/files/2021/06/Group-17-scaled.jpg)
php esc_html_e( 'Location:', 'themeslug' ); ?
php esc_html_e( '82 Main St. Brooklyn, NY', 'themeslug' ); ?
php esc_html_e( 'Date:', 'themeslug' ); ?
php esc_html_e( 'October 24, 2021', 'themeslug' ); ?
php esc_html_e( 'Purchase Tickets' ); ?
```
Now, whenever a user inserts this pattern, they will not be able to remove the Image block. They’ll still be able to edit the image itself and any settings, though.
You can combine both of these techniques, locking movement and removal, depending on your use case. For some patterns, this is often needed to keep the design of the pattern intact.
## [Template locking](#template-locking)
Template locking refers to locking all of the nested blocks within a container-type block. There are three types of locking that you can accomplish with template locking:
* **`all`:** Combined with the block locking methods covered earlier, lock all blocks within the container.
* **`insert`:** Prevent users from inserting new blocks within the container.
* **`contentOnly`:** Lock down all block settings within the container, only allowing the user to edit content (e.g., text and media).
Template locking is only possible for a subset of container-type blocks in Core:
* Group
* Cover
* Columns
* Column
* Navigation
### [Locking all nested blocks](#locking-all-nested-blocks)
With the built-in UI controls, you can lock all nested blocks within a pattern if it’s wrapped in one of the supported container-type blocks.
This type of locking builds off the block locking covered earlier in this article. You can choose to disable movement, prevent removal, or both for all nested blocks.
When clicking on the wrapping Group block inside your event pattern and opening the **Lock** modal, select both the **Disable movement** and **Prevent removal** options. You should also see another option labeled **Apply to all blocks**, which will let you apply these rules to every block inside the Group:
If you look at the underlying block markup, you will see a new `templateLock` attribute, which will be set to `all`. You will also see the `lock` attribute with both `move` and `remove` set to `true`:
Like with individual block locking, you can mix and match what you want to lock at the template level.
Now create a new file named `/patterns/event-lock-all.php` in your theme and paste the following code into it:
```
php
/**
* Title: Event (Lock All)
* Slug: themeslug/event-lock-all
* Categories: banner
* Viewport Width: 1376
*/
?
![Image of a woman being carried through the air by swans.](https://s.w.org/patterns/files/2021/06/Group-17-scaled.jpg)
php esc_html_e( 'Location:', 'themeslug' ); ?
php esc_html_e( '82 Main St. Brooklyn, NY', 'themeslug' ); ?
php esc_html_e( 'Date:', 'themeslug' ); ?
php esc_html_e( 'October 24, 2021', 'themeslug' ); ?
php esc_html_e( 'Purchase Tickets' ); ?
```
Whenever a user inserts this new pattern into their content, they will not be able to move or remove any of the blocks within the pattern. But they can still edit each block’s content and settings.
### [Preventing new blocks from being inserted](#preventing-new-blocks-from-being-inserted)
You can also lock your container-type blocks in patterns from having new blocks inserted into them. This is useful when you need to keep tight control over what is allowed within the design.
Unlock previous methods of locking blocks, this method has no UI controls associated with it. This means that you must edit the block markup directly by adding the `templateLock` value.
Take a look at what this looks like when applied to a wrapping Group block:
As you can see, the `templateLock` attribute is set to `insert` (i.e, the Group has locked inserting new blocks).
Now create a new file in your theme named `/patterns/event-prevent-insert.php` and paste the following code into it:
```
php
/**
* Title: Event (Prevent Insert)
* Slug: themeslug/event-prevent-insert
* Categories: banner
* Viewport Width: 1376
*/
?
![Image of a woman being carried through the air by swans.](https://s.w.org/patterns/files/2021/06/Group-17-scaled.jpg)
php esc_html_e( 'Location:', 'themeslug' ); ?
php esc_html_e( '82 Main St. Brooklyn, NY', 'themeslug' ); ?
php esc_html_e( 'Date:', 'themeslug' ); ?
php esc_html_e( 'October 24, 2021', 'themeslug' ); ?
php esc_html_e( 'Purchase Tickets' ); ?
```
When a user adds this pattern into their post or page content, they will be able to customize the blocks within the pattern. But they will not be able to add any new blocks from within the UI.
### [Applying content-only editing](#applying-content-only-editing)
One of the most useful features for patterns is to use template locking to only allow the user to customize the content of a pattern. This can be useful when you need full control over the design but need to allow user input.
When this type of locking is applied to a container-type block, users will see a new **Content** panel when inspecting any block instead of the usual block settings:
This is also a type of locking that has no UI controls and must be manually applied to the block markup. If applied to the outer Group block of your pattern, it will look like this:
Note that the `templateLock` attribute is set to `contentOnly`, which means that everything but content editing is locked.
Now let’s apply this to the event pattern. Create a new file in your theme named `/patterns/event-content-only.php` and paste the following code into it:
```
php
/**
* Title: Event (Content Only)
* Slug: themeslug/event-content-only
* Categories: banner
* Viewport Width: 1376
*/
?
![Image of a woman being carried through the air by swans.](https://s.w.org/patterns/files/2021/06/Group-17-scaled.jpg)
php esc_html_e( 'Location:', 'themeslug' ); ?
php esc_html_e( '82 Main St. Brooklyn, NY', 'themeslug' ); ?
php esc_html_e( 'Date:', 'themeslug' ); ?
php esc_html_e( 'October 24, 2021', 'themeslug' ); ?
php esc_html_e( 'Purchase Tickets' ); ?
```
With this setting in place, the user will be able to edit the Image block’s media and the content of the individual Paragraph and Button blocks. But they won’t be able to make any design changes directly inside of the pattern.
First published
April 30, 2024
[Previous
Block Type Patterns
Previous: Block Type Patterns](https://developer.wordpress.org/themes/patterns/block-type-patterns/)
[Next
Features
Next: Features](https://developer.wordpress.org/themes/features/)
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
