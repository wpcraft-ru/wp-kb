Border radius size presets in WordPress 6.9 – WordPress Developer Blog
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
[WordPress Developer Blog](https://developer.wordpress.org/news)
Border radius size presets in WordPress 6.9
* [Snippets](https://developer.wordpress.org/news/snippets/)
* Categories
+ [Blocks](https://developer.wordpress.org/news/category/blocks/)
+ [Concepts](https://developer.wordpress.org/news/category/concepts/)
+ [Common APIs](https://developer.wordpress.org/news/category/common-apis/)
+ [Design](https://developer.wordpress.org/news/category/design/)
+ [Playground](https://developer.wordpress.org/news/category/playground/)
+ [Plugins](https://developer.wordpress.org/news/category/plugins/)
+ [Themes](https://developer.wordpress.org/news/category/themes/)
* About
+ [About this site](https://developer.wordpress.org/news/about/)
+ [Updates](https://developer.wordpress.org/news/category/updates/)
+ [How to contribute](https://developer.wordpress.org/news/how-to-contribute/)
+ [Tips and guidelines](https://developer.wordpress.org/news/tips-and-guidelines-for-writers/)
* [Subscribe](https://developer.wordpress.org/news/subscribe/)
* [Developer Resources](https://developer.wordpress.org/)
* [Snippets](https://developer.wordpress.org/news/snippets/)
* Categories
+ [Blocks](https://developer.wordpress.org/news/category/blocks/)
+ [Concepts](https://developer.wordpress.org/news/category/concepts/)
+ [Common APIs](https://developer.wordpress.org/news/category/common-apis/)
+ [Design](https://developer.wordpress.org/news/category/design/)
+ [Playground](https://developer.wordpress.org/news/category/playground/)
+ [Plugins](https://developer.wordpress.org/news/category/plugins/)
+ [Themes](https://developer.wordpress.org/news/category/themes/)
* About
+ [About this site](https://developer.wordpress.org/news/about/)
+ [Updates](https://developer.wordpress.org/news/category/updates/)
+ [How to contribute](https://developer.wordpress.org/news/how-to-contribute/)
+ [Tips and guidelines](https://developer.wordpress.org/news/tips-and-guidelines-for-writers/)
* [Subscribe](https://developer.wordpress.org/news/subscribe/)
* [Developer Resources](https://developer.wordpress.org/)
# Border radius size presets in WordPress 6.9
By [Justin Tadlock](https://profiles.wordpress.org/greenshady/)
September 29, 2025
[Themes](https://developer.wordpress.org/news/category/themes/)
With WordPress 6.9 just around the corner, it’s time to look at one of my favorite new theme developer tools: border radius size presets.
[Added in Gutenberg 21.5](https://github.com/WordPress/gutenberg/pull/67544), these size presets let you define an array of sizes that users can apply to blocks that support border radius. You can also reuse them within your own theme stylesheets and `theme.json` file.
Let’s take a dive into this new design tool.
Table of Contents
1. [Defining radius size presets](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/#defining-radius-size-presets)
2. [Defining multiple presets](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/#defining-multiple-presets)
1. [UI differences when defining many presets](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/#ui-differences-when-defining-many-presets)
3. [Styling blocks with presets](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/#styling-blocks-with-presets)
4. [Hand-drawn and other unique styles](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/#hand-drawn-and-other-unique-styles)
5. [Notable limitations](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/#notable-limitations)
1. [Not all valid CSS values are allowed](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/#not-all-valid-css-values-are-allowed)
2. [No way to disable custom radius sizes](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/#no-way-to-disable-custom-radius-sizes)
## Defining radius size presets
Like other presets, you can define border radius sizes via the standard `theme.json` file in your theme. The new property can be defined via the `settings.border.radiusSizes` property (read the [border documentation](https://developer.wordpress.org/themes/global-settings-and-styles/settings/border/) for `theme.json` for more information).
`radiusSizes` is an array of size objects, each accepting three properties:
* **`name`:** A human-readable label for the size.
* **`slug`:** The machine-readable name for the size, which is used to generate a CSS custom property for the preset (e.g., `--wp--preset--border-radius--{slug}`).
* **`size`:** The CSS value for the size.
Here is a simple example of a size named `xs` defined in `theme.json`:
```
{
"$schema": "https://schemas.wp.org/trunk/theme.json",
"version": 3,
"settings": {
"border": {
"radiusSizes": [
{
"name": "XS",
"slug": "xs",
"size": "0.125rem"
}
]
}
}
}
```
With the basics of how to define border radius size presets, let’s see what’s possible with the new feature.
Tip: I generally prefer a t-shirt naming convention when my radius presets scale from smaller sizes to larger.
## Defining multiple presets
When building a theme, you most likely want to offer a range of standard size presets that match your theme’s design. For this tutorial, let’s assume you want seven sizes that scale from `0.125rem` up to `1.5rem`.
Use this code in your `theme.json`:
```
{
"$schema": "https://schemas.wp.org/trunk/theme.json",
"version": 3,
"settings": {
"border": {
"radius": true,
"radiusSizes": [
{
"name": "XS",
"slug": "xs",
"size": "0.125rem"
},
{
"name": "Small",
"slug": "sm",
"size": "0.25rem"
},
{
"name": "Medium",
"slug": "md",
"size": "0.375rem"
},
{
"name": "Large",
"slug": "lg",
"size": "0.5rem"
},
{
"name": "XL",
"slug": "xl",
"size": "0.75rem"
},
{
"name": "2XL",
"slug": "2-xl",
"size": "1rem"
},
{
"name": "3XL",
"slug": "3-xl",
"size": "1.5rem"
}
]
}
}
}
```
Note that the code also sets `radius` to `true`, which enables the border radius selector in the UI for blocks that support it.
To test your new sizes, add a Group, Cover, or some other block in the editor and look for the **Styles → Radius** panel in the sidebar. Try moving the slider and seeing how the block reacts to the chosen size:
You can also unlink each of the corners and define individual radii for each. For example, the following screenshot shows selecting the `3-xl` radius for the top right and bottom left corners but no radius for the top left and bottom right:
### UI differences when defining many presets
It’s important to note that if you add more than seven presets, the range slider will change to a select dropdown in the UI.
If you want to give this a try, add the following `full` size to your existing `radiusSizes` array:
```
{
"name": "Full",
"slug": "full",
"size": "9999em"
}
```
Then refresh your editor and see the change:
## Styling blocks with presets
Like all other presets that you can define via `theme.json`, WordPress automatically outputs these as CSS custom properties on the `:root` element. Here’s the CSS generated for the custom sizes you defined earlier:
```
:root {
--wp--preset--border-radius--xs: 0.125rem;
--wp--preset--border-radius--sm: 0.25rem;
--wp--preset--border-radius--md: 0.375rem;
--wp--preset--border-radius--lg: 0.5rem;
--wp--preset--border-radius--xl: 0.75rem;
--wp--preset--border-radius--2-xl: 1rem;
--wp--preset--border-radius--3-xl: 1.5rem;
}
```
This means that you can use these presets just like any other CSS custom property, accessing them via `var(--wp--preset--border-radius--{slug})`.
Let’s suppose you wanted Image blocks in your theme to have this design by default:
To do this, you would need to apply the radius to the Image block in your `theme.json` file under the `styles` property:
```
{
"styles": {
"blocks": {
"core/image": {
"border": {
"radius": {
"topLeft": "0px",
"topRight": "var(--wp--preset--border-radius--3-xl)",
"bottomLeft": "var(--wp--preset--border-radius--3-xl)",
"bottomRight": "0px"
}
}
}
}
}
}
```
To learn more about applying presets to blocks, check out the [Using Presets](https://developer.wordpress.org/themes/global-settings-and-styles/styles/using-presets/) documentation in the Theme Handbook.
Of course, you’re not limited to using presets in `theme.json`. You can apply them to [global style variations](https://developer.wordpress.org/themes/global-settings-and-styles/style-variations/), [block style variations](https://developer.wordpress.org/themes/features/block-style-variations/), and even use them directly in [custom stylesheets](https://developer.wordpress.org/themes/features/block-stylesheets/).
## Hand-drawn and other unique styles
As you read earlier, size presets are applied on individual corners of a block or element. For example, here is the HTML output when applying the `3xl` size to a Group block:
```
...
```
This means that when you register size presets, what you’re actually defining are values that can be applied to:
* `border-top-left-radius`
* `border-top-right-radius`
* `border-bottom-left-radius`
* `border-bottom-right-radius`
For example, I *really* wanted to define a “hand drawn” size like this:
```
{
"name": "Hand Drawn",
"slug": "hand-drawn",
"size": "255px 15px 225px 15px/15px 225px 15px 255px"
}
```
This uses the [two-value syntax](https://developer.mozilla.org/en-US/docs/Web/CSS/border-top-left-radius#syntax) that defines both the horizontal and vertical semi-major axes of the ellipse for each corner, but this form is meant to be used with the `border-radius` shorthand.
I’d hoped to allow users to pick a border radius and quickly get this result:
Such a preset doesn’t work because that value is only valid for the `border-radius` shorthand. Remember that sizes must be valid for individual corners.
Instead, I had to define the two separate presets that could be used individually on the corners.
```
{
"$schema": "https://schemas.wp.org/trunk/theme.json",
"version": 3,
"settings": {
"border": {
"radius": true,
"radiusSizes": [
{
"name": "Drawn 1",
"slug": "drawn-1",
"size": "15px 255px"
},
{
"name": "Drawn 2",
"slug": "drawn-2",
"size": "255px 15px"
}
]
}
}
}
```
The two-value syntax (horizontal axis and vertical axis) is a valid value, so you can absolutely use it to accomplish some impressive designs.
The downside is that users must unlink the border radius control and define each of the corners to get that exact design for a block:
The upside is that they can try all sorts of neat combos of your registered radius size presets.
Of course, you can also make it easier for them by adding a “hand drawn” block style variation that automatically applies these radii.
Please share any unique border radius designs that you come up with.
## Notable limitations
As I was testing this feature for the article, I ran into two notable limitations. They are not blockers for most use cases, but it’s worth knowing about them as you define your own border radius sizes.
### Not all valid CSS values are allowed
I attempted to use `calc(infinity * 1px)` for the `full` size, but it didn’t work within the UI. Whenever selected, it applied the correct preset but the **Default** option was shown as selected in the UI.
Upon further testing, I attempted to use both `var()` and `clamp()` values, which also broke. As near as I can tell, this feature is limited to values without `(` or `)` characters, but it needs more investigation.
### No way to disable custom radius sizes
Border radius presets are a nice start, but there is not currently a method of disabling the UI for custom sizes. Generally, other design tools will let you disable custom values via a `customSettingName` property.
You can set `settings.border.radius` to `false` in `theme.json`. That will disable presets and custom sizes from appearing in the UI.
There is an [open ticket](https://github.com/WordPress/gutenberg/issues/49504) that discusses both this and the previous limitation in the Gutenberg repository.
*Props to [@welcher](https://profiles.wordpress.org/welcher/) and [@juanmaguitar](https://profiles.wordpress.org/juanmaguitar/) for feedback and review on this article.*
**Share the post:**
* [Share on Mastodon (Opens in new window)Mastodon](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/?share=mastodon&nb=1)
* [Share on LinkedIn (Opens in new window)LinkedIn](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/?share=linkedin&nb=1)
* [Share on X (Opens in new window)X](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/?share=x&nb=1)
* [Share on Bluesky (Opens in new window)Bluesky](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/?share=bluesky&nb=1)
* Share on Mail (Opens in new window)Mail
* [Share on Reddit (Opens in new window)Reddit](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/?share=reddit&nb=1)
* [Share on Tumblr (Opens in new window)Tumblr](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/?share=tumblr&nb=1)
* [Share on Telegram (Opens in new window)Telegram](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/?share=telegram&nb=1)
* [Share on WhatsApp (Opens in new window)WhatsApp](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/?share=whatsapp&nb=1)
**Categories:** [Themes](https://developer.wordpress.org/news/category/themes/)
**Tags:** [theme.json](https://developer.wordpress.org/news/tag/theme-json/)
## One response to “Border radius size presets in WordPress 6.9”
1. [Cory Birdsong](https://birdsong.dev)
[October 1, 2025](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/#comment-19445)
There should really also be border width presets, as well as possibly overall border style presets? I made an issue about widths here: <https://github.com/WordPress/gutenberg/issues/72015>
In general the support for presets and custom sizes is all over the place. Box shadow only supports presets, and border has thusfar only supported custom values. Some consistency here would be good.
[Reply](https://developer.wordpress.org/news/2025/09/border-radius-size-presets-in-wordpress-6-9/?replytocom=19445#respond)
### Leave a Reply [Cancel reply](/news/2025/09/border-radius-size-presets-in-wordpress-6-9/#respond)
Your email address will not be published. Required fields are marked \*
Comment \*
Name \*
Email \*
Website
Save my name, email, and website in this browser for the next time I comment.
Notify me of follow-up comments by email.
Notify me of new posts by email.
Δ
## Have an idea for a new post?
### [Learn how to contribute](https://developer.wordpress.org/news/how-to-contribute/)
Share your knowledge with fellow WordPress developers.
### [Review tips and guidelines](https://developer.wordpress.org/news/tips-and-guidelines-for-writers/)
Everything you need to know about writing for the Blog.
## Subscribe to the Blog
Email Address
Subscribe
Join 1,864 other subscribers
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
Close
PreviousNext

2.3s · markdown via readability
