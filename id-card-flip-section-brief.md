# 3D Hanging ID Card Flip Section — Website Implementation Brief

## Project Goal

Build a polished, interactive website section featuring a realistic ID card hanging from a short lanyard. The card rests naturally in the center of a minimal white page. When the user hovers over the card, it rotates in 3D to reveal its back, then smoothly returns to the front when the pointer leaves.

The experience should feel tactile and premium, with convincing depth, subtle inertia, soft shadows, and physically believable motion. It should look like a real plastic access card suspended from a lanyard rather than a flat UI tile.

## Visual Direction

- **Background:** clean white or near-white (`#FAFAFA`), with generous whitespace.
- **Composition:** one centered ID card with a short lanyard attached above it. Keep the section minimal and uncluttered.
- **Card:** realistic PVC/plastic access card, rounded corners, fine edge highlights, restrained reflections, and subtle thickness.
- **Lanyard:** short, fabric-like strap emerging from the top of the section, attached to a small metal clip or eyelet. The strap should move slightly as the card rotates.
- **Lighting:** soft studio lighting from the upper-left, with gentle contact shadows and a faint ambient shadow beneath/behind the card.
- **Depth:** visible card edges during rotation, natural perspective, layered shadows, and a subtle highlight that shifts with the card angle.
- **Typography:** clean modern sans-serif. Keep card information legible and realistically arranged.
- **Overall style:** minimalist product photography meets refined interactive web design. Avoid excessive gradients, neon colors, glassmorphism, decorative UI, or unnecessary visual effects.

## ID Card Content

Use believable placeholder details that can be easily replaced:

### Front
- Small brand/company logo
- Profile photo
- Full name
- Job title or role
- ID number
- Optional small QR code or barcode

### Back
- Company name or logo
- Short instruction such as “If found, please return this card.”
- Contact email or website
- QR code or barcode
- Optional small security details

Make the front and back feel like two sides of the same physical card. Keep both sides aligned and designed as one component.

## Interaction and Motion

### Default State
- The front of the card faces the viewer.
- The lanyard hangs naturally with a slight curve or gentle tension.
- The card has a subtle, soft shadow that visually separates it from the white background.
- Motion should be almost imperceptible at rest, not constantly distracting.

### Hover / Pointer Enter
- Rotate the card around its vertical axis to reveal the back (`rotateY(180deg)` or an equivalent 3D transform).
- Use a smooth, premium easing curve. The movement should feel physical rather than like a basic CSS toggle.
- Add a small amount of swing around the attachment point. The lanyard should react with slight delayed movement instead of appearing rigidly connected to the card.
- Let the shadow shift and soften in response to the card's orientation.
- Add a subtle perspective tilt based on pointer position, but keep the effect restrained and readable.
- Ensure the card does not move so much that the user loses sight of the flip interaction.

### Hover / Pointer Leave
- Return the card smoothly to the front.
- Allow a very small, damped settling motion after the flip, as if the card has weight.
- Avoid exaggerated wobbling, springiness, or repeated oscillation.

### Pointer Movement
- Optional: apply subtle `rotateX` and `rotateY` tilt based on pointer position.
- Clamp tilt angles to a small range so the card remains realistic.
- Use smoothing or interpolation to prevent jitter.
- Do not let pointer tracking fight with the primary front-to-back flip.

### Touch Devices
- Hover is unavailable on touchscreens, so provide a tap-to-flip interaction.
- A second tap should flip the card back.
- Keep the same 3D transition and visual quality on mobile.
- Prevent accidental page scrolling or trapping of touch gestures.

## Technical Approach

Use a layered implementation, selecting tools based on what genuinely improves the result rather than adding libraries unnecessarily.

### Core
- **HTML:** semantic section markup and separate front/back card faces.
- **CSS:** 3D transforms, `perspective`, `transform-style: preserve-3d`, `backface-visibility: hidden`, responsive layout, lighting details, and shadows.
- **JavaScript:** pointer/touch state, interaction handling, and accessible state updates.

### Animation
- **GSAP:** orchestrate the card flip, lanyard response, shadow changes, and settling motion. Use timelines so related movement feels coordinated.
- **GSAP ScrollTrigger:** only if the section needs scroll-based entrance animation. Do not add it just for the hover flip.
- **CSS transforms:** use for the underlying 3D structure; GSAP can animate transform values and timing.
- **Optional Three.js:** use only if true 3D geometry, physically based materials, realistic reflections, or a genuinely 3D clip/lanyard are essential. For a single ID card, CSS 3D plus GSAP is likely lighter, simpler, and more performant. Do not use Three.js merely to make the card rotate.

### Optional Enhancements
- **Lenis:** only if the overall site already uses smooth scrolling. It is not required for this component.
- **Custom shader/WebGL effects:** avoid unless a specific visual requirement cannot be achieved with CSS.
- **SVG:** useful for crisp logos, small icons, and barcode-like details.

## Suggested DOM Structure

Use a structure similar to this, adapting class names to the existing project:

```html
<section class="id-card-section" aria-label="Interactive ID card">
  <div class="id-card-stage">
    <div class="lanyard" aria-hidden="true">
      <div class="lanyard-strap"></div>
      <div class="lanyard-clip"></div>
    </div>

    <button
      class="id-card-interaction"
      type="button"
      aria-label="Flip ID card"
      aria-pressed="false"
    >
      <span class="id-card">
        <span class="id-card-face id-card-front">
          <!-- Front content -->
        </span>
        <span class="id-card-face id-card-back">
          <!-- Back content -->
        </span>
      </span>
    </button>
  </div>
</section>
```

This is a structural example, not a requirement to use these exact tags. Ensure the final implementation has valid semantics and that the button's accessible name and pressed state update correctly.

## 3D Construction Notes

- Place the card inside a 3D scene/container with a clear perspective value.
- Keep the front and back faces absolutely aligned.
- Front face: `rotateY(0deg)`.
- Back face: `rotateY(180deg)`.
- Hide the reverse side of each face using `backface-visibility: hidden`.
- Rotate the inner card wrapper, not the entire page or section.
- Use a subtle edge element or layered pseudo-elements to simulate card thickness.
- Keep the lanyard as a separate animated object connected visually to the card's top-center attachment point.
- Avoid applying conflicting CSS transitions and GSAP transforms to the same properties.
- Use a consistent transform origin near the card's top attachment point if the card should swing like a hanging object. If needed, separate the flip rotation from the swing rotation into nested wrappers so each animation has its own responsibility.

## Realistic Shadows and Materials

Create depth through multiple restrained layers instead of one dark shadow:

1. **Ambient shadow:** broad, very low-opacity shadow behind the card.
2. **Contact shadow:** tighter shadow close to the card and attachment point.
3. **Directional shadow:** shifts slightly during the flip to imply a consistent light source.
4. **Edge highlight:** thin, subtle border/highlight to suggest a plastic card surface.
5. **Surface detail:** optional fine texture or restrained reflection, kept subtle enough that the card remains clean.

Shadows should respond to movement without flickering. Avoid very dark shadows, excessive blur, or a floating-card effect that feels disconnected from the lanyard.

## GSAP Animation Guidance

Implement the motion as coordinated animation states rather than several unrelated event handlers.

- Create a reusable timeline or state-driven animation for the flip.
- Animate the lanyard and card with slightly different timing to suggest flexible fabric and a heavier card.
- Use a restrained ease such as a smooth power-based ease for the main rotation.
- Use a short, damped settling phase only after the main movement.
- Prevent rapid pointer enter/leave events from creating stacked or competing animations. Kill, overwrite, or reuse active animations appropriately.
- Keep the final state deterministic: front-facing when inactive and back-facing when activated.
- Respect `prefers-reduced-motion`. When reduced motion is enabled, use a shorter, simpler transition or an instant face change.
- Avoid continuous animation loops unless they are extremely subtle and demonstrably improve the experience.

## Responsive Behavior

### Desktop
- Center the card with generous whitespace.
- Support mouse hover and subtle pointer tracking.
- Keep the full lanyard visible within the section.

### Tablet
- Reduce the card and lanyard scale proportionally.
- Keep text readable and the flip interaction easy to trigger.
- Disable or reduce pointer tracking if it causes awkward movement.

### Mobile
- Use tap-to-flip.
- Ensure the card is large enough to read without filling the entire viewport.
- Keep the lanyard visible without pushing the card too far down.
- Avoid hover-only styling.
- Ensure the card faces do not overflow the viewport.

Use `clamp()` and responsive breakpoints instead of hard-coded dimensions wherever practical.

## Accessibility

- The card must be operable by keyboard.
- Use a real button or equivalent accessible control for the flip.
- Provide a visible focus state that fits the design.
- Update `aria-pressed` or another appropriate state attribute when the card flips.
- Keep decorative lanyard elements hidden from assistive technology.
- Do not communicate front/back state through color alone.
- Respect `prefers-reduced-motion`.
- Maintain sufficient contrast and readable type sizes.

## Performance and Code Quality

- Animate transforms and opacity where possible.
- Avoid expensive layout recalculations during pointer movement.
- Use `requestAnimationFrame` or GSAP's ticker only when needed for smooth pointer tracking.
- Clean up event listeners and GSAP timelines if the component is mounted/unmounted by a framework.
- Avoid unnecessary dependencies and large WebGL bundles.
- Test on current Chrome, Firefox, Safari, iOS Safari, and Android Chrome.
- Keep the card stable during rapid hover/tap changes and while resizing the browser.
- Make the section easy to customize through CSS variables for card dimensions, colors, radius, and shadow intensity.

## Implementation Deliverables

1. Complete responsive HTML structure.
2. CSS for the white section, lanyard, card faces, 3D depth, lighting, edge details, and shadows.
3. JavaScript using GSAP for coordinated flip and swing animation.
4. Touch interaction for mobile devices.
5. Keyboard support and reduced-motion handling.
6. Clearly marked placeholder content and assets that can be replaced.
7. Short setup instructions identifying required dependencies and where to customize the card.

## Acceptance Criteria

The finished section is successful when:

- The initial view is clean, minimal, and front-facing.
- The card visibly hangs from a short lanyard rather than floating independently.
- Hovering or tapping flips the card to its back in 3D.
- The lanyard, card, and shadow respond with subtle, coordinated, believable movement.
- The card has visible depth and realistic lighting without excessive effects.
- The animation feels smooth, controlled, and premium, with no jitter or sudden jumps.
- The card returns to a predictable state when the interaction ends.
- The layout works on desktop, tablet, and mobile.
- Keyboard users can flip the card, and reduced-motion preferences are respected.
- The implementation remains lightweight and maintainable.

## Final Direction for the Developer / AI Coding Agent

Build this as a production-quality website section, not a static mockup. First inspect the existing project structure and preserve its current framework, conventions, fonts, and styling where possible. Implement the effect in the actual website code, test the interaction, and refine the timing, perspective, shadows, and attachment point until the card feels physically suspended.

Prioritize realism and restraint: a short lanyard, a weighted card, soft studio shadows, a clean white background, and one satisfying 3D flip. Use GSAP for animation orchestration and CSS 3D for the card itself. Introduce Three.js only if the final visual requirements truly need real 3D geometry or materials.
