# Inkulator Design Guidelines

## Design Approach
**Dark Luxury Calculator Interface** - Drawing inspiration from premium financial apps (Robinhood, Cash App) with tattoo industry sophistication. This utility-focused tool demands clarity, trust, and professional polish while maintaining artistic edge.

## Typography System
- **Primary Font**: Inter (Google Fonts) - exceptional readability for numbers/forms
- **Accent Font**: Bebas Neue or Oswald - bold headers for impact
- **Hierarchy**:
  - Hero/Headers: 3xl-5xl, bold, tight tracking
  - Calculator labels: base-lg, medium weight
  - Input fields: lg, regular weight
  - Numerical outputs: 2xl-4xl, semibold (pricing prominence)
  - Body/helper text: sm-base, regular

## Layout System
**Tailwind Spacing Units**: Consistent use of 4, 6, 8, 12, 16 units
- Mobile padding: p-4 to p-6
- Desktop padding: p-8 to p-12
- Section spacing: gap-8 to gap-12
- Component spacing: gap-4 to gap-6

## Component Architecture

### Hero Section
**Full-width dark gradient backdrop with atmospheric tattoo imagery** (blurred background showing tattoo equipment, ink bottles, or artist at work - professional studio setting). Centered content with:
- Bold headline emphasizing precision pricing
- Subheadline on value proposition
- Primary CTA button (blurred background, gold accent border)
- Trust indicators below (e.g., "Used by 5,000+ artists")

### Calculator Interface (Core Component)
**Card-based design** with elevated appearance:
- Dark background with subtle border glow (gold/amber)
- Segmented form sections with clear labels
- Input fields: Dark with gold underline/border on focus
- Range sliders with gold track fill
- Real-time pricing display in prominent gold-accented card
- Breakdown table showing itemized calculations
- Toggle switches for additional options (color, custom design, complexity)

### Features Grid
**2-column on desktop, single on mobile**:
- Icon + title + description cards
- Icons using Heroicons (via CDN)
- Features: Time estimation, complexity factors, material costs, hourly rate calculator, deposit calculator, client quote generation

### Pricing Tiers (if applicable)
**3-column comparison table**:
- Individual, Shop, Enterprise tiers
- Gold accent on recommended plan
- Feature checkmarks with gold highlights

### Social Proof Section
**Single column testimonials** with:
- Artist photos (circular frames with gold border)
- Quote text
- Name, shop name, location
- Star ratings in gold

### Footer
Comprehensive footer with:
- Quick links navigation
- Social media icons (gold on hover)
- Newsletter signup with inline form
- Contact information
- Trust badges (secure payment, privacy certified)

## Images Section
1. **Hero Background Image**: Professional tattoo studio environment - artist working or close-up of tattoo equipment/ink setup. Dark, moody lighting with warm undertones. Image should be slightly blurred (backdrop-blur) to maintain text readability.

2. **Feature Icons**: Use Heroicons solid variants for calculator, clock, chart-bar, currency-dollar, document-text icons.

3. **Testimonial Photos**: Circular professional headshots of tattoo artists (diverse representation). Gold circular border treatment.

## Interaction Patterns
- Smooth input transitions (200ms)
- Calculator updates in real-time as users type
- Accordion sections for advanced options
- Sticky calculator summary on scroll (mobile)
- Haptic-feel number inputs with increment/decrement buttons
- Toast notifications for quote generation/saving

## Mobile-First Considerations
- Thumb-zone optimized button placement
- Bottom-sheet style calculator panel
- Swipeable testimonial cards
- Collapsible advanced options
- Large touch targets (minimum 44px)

## Trust-Building Elements
- Transparent pricing breakdown
- Professional certifications/partnerships logos
- Security badges
- Calculation methodology explanation tooltip
- Industry-standard rate references

**Key Design Principle**: Every element serves calculation clarity or trust-building—no decorative bloat. The dark aesthetic with gold accents creates professional luxury while maintaining functional excellence for daily pricing work.