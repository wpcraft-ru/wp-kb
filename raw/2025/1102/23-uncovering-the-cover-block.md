# Uncovering the Cover block

**Source:** https://learn.wordpress.org/lesson/uncovering-the-cover-block-copy/
**Course:** Beginner WordPress Designer
**Section:** Working with media blocks

---

## Transcript

### Introduction
The Cover block — displays text/content on top of an image or video. Excellent for headers, banners, CTAs.

### Key features and settings
The Cover block is a **container block** that holds other blocks within it.

**Adding:**
- Upload image/video from computer or Media Library
- "Use Featured Image" option — sets cover as page/post featured image
- WordPress auto-adds complementary overlay color

**Settings:**
- **Alignment:** full width
- **Content position:** top left, middle left, center, etc.
- **Full height** toggle
- **Duotone filter**
- **Fixed Background** — parallax effect
- **Repeat Background** — when image is smaller than space
- **Focal point picker** — important for mobile viewing
- **Alt text** — required

**Styles:**
- Overlay color + opacity (lighter/darker)
- Padding around content
- **Aspect ratio** — prevents image cropping on different screen sizes (e.g. Classic 3×2)

### Banner example
1. Add Cover block → image → full width → content position: top middle
2. Text "Explore the mountains" → custom size (4 rem) → appearance: medium
3. Wrap text in Group block → deselect "Inner blocks use content width" → alignment: full width
4. Overlay opacity: 50% → padding → check on mobile

### Parallax example
1. Cover block → image → full width → full height → Fixed Background
2. Duplicate block → add new image
3. Select both → Group → block spacing: 0

### More examples
- CTA for portfolio
- Header with logo + site title + navigation (from Patterns Directory)

## Practical
1. Add Cover block with mountain image from Unsplash
2. Text: "Exploring the Mountains"
3. Design banner to match video example
4. Test on mobile → optionally adjust aspect ratio
