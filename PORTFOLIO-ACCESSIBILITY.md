# Brainstorm: Accessibility & Compliance

## Design System Accessibility

All Vault components built with WCAG 2.1 AA compliance:

Colors: 4.5:1+ contrast ratio
- Primary blue on white: 4.54:1
- Dark gray on white: 9.17:1
- No color-only information (always paired with symbol/text)

Forms: Proper label associations
- <label htmlFor="email"> with <input id="email" />
- Screen readers announce input purpose

Interactive Elements: Semantic HTML
- Buttons are <button>, not <div onClick>
- Links are <a>, not <div onClick>
- Form groups: <fieldset> with <legend>

Keyboard Navigation: Tab order matches visual order
- All interactive elements reachable via keyboard
- Focus indicators visible

## Compliance Checklist

[x] All text 4.5:1+ contrast ratio
[x] All form inputs have labels
[x] All images have alt text
[x] All interactive elements keyboard accessible
[x] Color not sole conveyance of information
[x] Focus indicators visible
[x] Semantic HTML throughout
[x] No auto-playing video/audio
[x] ESLint a11y rules active

## Testing Strategy

Automated: ESLint jsx-a11y plugin, Lighthouse audit (90+)
Manual: Keyboard navigation (Tab/Enter/Escape), screen reader (NVDA/VoiceOver), zoom 200%, high contrast mode
User Testing: Deferred to Phase 2 (real users with disabilities)

## Tools Used

ESLint: jsx-a11y plugin with strict rules
Lighthouse: Automated accessibility audit
WebAIM: Contrast checker for color validation
NVDA: Screen reader testing (Windows)
VoiceOver: Screen reader testing (Mac/iOS)

## Future Improvements

Closed captions for deck-building videos
Text-to-speech for card descriptions
Voice search for card lookup
High contrast mode toggle

Conclusion: WCAG 2.1 AA compliance from day 1, zero accessibility compromises.
