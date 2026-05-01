# Using a plugin to create a carousel slider

**Source:** https://learn.wordpress.org/lesson/using-a-plugin-to-create-a-carousel-slider/
**Course:** Beginner WordPress Designer
**Section:** Working with media blocks

---

## Transcript

### Introduction
Web carousels — elegant way to group related content into one space.

### Plugin options
- **MetaSlider** — traditional slider, many settings (mobile, advanced, accessibility, developer)
- **Smart Slider 3**
- **Carousel Slider Block for Gutenberg** — block-based, supports other blocks inside, WooCommerce compatible

### Carousel Slider Block for Gutenberg workflow
1. Install and activate plugin
2. Type `/slider` → select Carousel Slider
3. Add blocks within (e.g. 3 Image blocks)
4. Select parent block → alignment: wide width

**Settings:**
- Number of slides to show
- Slides to scroll at a time
- Slide animation speed
- Arrows and dots navigation (toggled on by default)
- **Responsive breakpoints** — how many slides at each screen size
- Set aspect ratio on images for consistent sizing

### Beyond images
Can add Cover blocks with Heading + Spacer + Buttons inside. Set slides to show: 1 for full-screen slider.

### Accessibility
Sliders can cause accessibility issues:
- Always add **alt text** to images
- Slider must be operable via **keyboard**
- Must work with **screen readers**
- **Avoid auto-scrolling/autoplay** — give audience control
- If auto-play: must have **pause control**
- RTL in WordPress = support for right-to-left languages

## Practical
Install and activate a slider plugin of choice and test it.
