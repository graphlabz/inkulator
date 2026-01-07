# Inkulator - Tattoo Pricing Calculator

## Overview

Inkulator is a tattoo pricing calculator designed to help artists and clients understand tattoo cost based on size, style, and detail. The app features a free mode with basic styles and a Pro mode that unlocks advanced pricing customization, additional styles, and hourly rate calculations.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter (lightweight React router)
- **State Management**: TanStack React Query for server state, React hooks for local state
- **Styling**: Tailwind CSS with custom design system
- **UI Components**: shadcn/ui component library (Radix UI primitives)
- **Animations**: Framer Motion for smooth transitions
- **Build Tool**: Vite with React plugin

The frontend follows a component-based architecture with:
- Custom reusable components (`Button`, `Input`, `OptionGroup`, `PriceDisplay`, `Select`)
- shadcn/ui components for complex UI patterns
- Custom hooks for business logic (`use-calculator`, `use-estimates`)
- Path aliases configured: `@/` for client/src, `@shared/` for shared code

### Backend Architecture
- **Framework**: Express.js with TypeScript
- **Runtime**: Node.js with tsx for development
- **API Design**: RESTful endpoints with Zod validation
- **Database ORM**: Drizzle ORM with PostgreSQL dialect

The backend uses a layered architecture:
- `server/routes.ts`: API route handlers with input validation
- `server/storage.ts`: Database abstraction layer (IStorage interface)
- `server/db.ts`: Database connection configuration
- `shared/routes.ts`: Shared API contract definitions with Zod schemas

### Shared Code
The `shared/` directory contains code used by both frontend and backend:
- `schema.ts`: Drizzle database schema and Zod validation schemas
- `routes.ts`: API route definitions with type-safe contracts
- `pricing.ts`: Centralized pricing configuration, types, and calculation logic

## Pricing System (v8.7)

### Core Principles - SIZE-BASED MODE
1. **ALL modifiers are ADDITIVE** - No multipliers anywhere
   - Formula: `effective_rate = style_rate + visual_detail_add + mix_style_add + expertise_add + complexity_add + placement_add`
   - Then: `final_price = (width_cm + height_cm) * effective_rate`
2. **Style, Visual Detail, and Mix Style are INDEPENDENT** - Each adds to rate without affecting others
3. **Each style has its own rate constant** - Complete rate, not multiplier on base
4. **Perimeter-based sizing** - Uses (width + height) for strictly monotonic growth
5. **Only round FINAL price** - No intermediate rounding or clamping
6. **Round to whole number** - No decimals in final display

### TIME-BASED PRICING MODE (Pro Only)
**Formula:**
```
FinalTimePrice = (1.0 × VisualDetailMultiplier × DesignComplexityMultiplier × ColorCountMultiplier × BodyPlacementMultiplier) × ExpertiseHourlyRate × NormalizationFactor
```

Where:
- **NormalizationFactor** = EstimatedHours (derived from size or manual input)
- **EstimatedTime** = 15 minutes per inch of perimeter (auto-calculated)
- **VisualDetailMultiplier**: Minimal 1.0, Fine Line 1.15, Detailed 1.35, Semi-Realism 1.55, Full Realism 1.80
- **ComplexityMultiplier**: Simple 1.0, Moderate 1.30, Complex 1.65 (or slider 0-10: 1.0 to 1.80)
- **ColorMultiplier**: 1 color 1.0, 2 colors 1.10, 3 colors 1.20, up to 6+ colors 1.50
- **PlacementMultiplier**: None 1.0, Easy areas 1.0-1.08, Moderate 1.12-1.18, Hard 1.25-1.30, Very Hard 1.35-1.45, Extreme 1.55-1.70
- **ExpertiseHourlyRate**: Beginner ₱800/hr, Amateur ₱1200/hr, Advanced ₱1800/hr, Expert ₱2500/hr

**Key Difference from Size-Based:**
- Time-based uses MULTIPLIERS (not additions) for factors
- Time is editable with hours/minutes inputs (arrow up/down or typing)
- Effective rate = Base expertise rate × Combined multipliers

### Price Formula
- Formula: `effective_rate = style_rate + visual_detail_add + mix_style_add + expertise_add + complexity_add + placement_add`
- Then: `final_price = (width_cm + height_cm) * effective_rate`
- Visual Detail additions: Minimal +0, Fine Line +15, Detailed +35, Semi-Realism +55, Full Realism +80
- Mix Style addition: +20 per additional style beyond the first
- Expertise additions: Beginner +0, Amateur +19.685, Advanced +59.055, Expert +98.425
- Complexity additions: Simple +0, Moderate +29.53, Complex +68.90
- Placement additions: None/Arm/Leg +0, Chest/Back +9.84, Ribs/Foot +29.53, Hand/Neck +39.37, Face +49.21
- Example: Linework (118) + Fine Line (+15) + Amateur (+19.69) + Moderate (+29.53) + Chest (+9.84) = 192.06 effective rate
- At 2×2 inches (10.16 cm perimeter): 10.16 × 192.06 = ₱1951

### Style System (Two-Tier)

**Free Tier - 5 Visual Complexity Styles:**
- Minimalist: 98.43 (Simple shapes, clean lines, minimal shading)
- Fine Line: 125 (Precise, delicate linework with subtle details)
- Detailed: 148 (Medium complexity with textures and shading)
- Semi-Realism: 185 (Stylized realism with artistic interpretation)
- Full Realism: 220 (Photorealistic with maximum detail)

**Pro Tier - 50+ Styles in 8 Families:**
- Line & Minimal: Linework (118), Script (130), Single Needle (135)
- Traditional & Cultural: Traditional (140), Neo-Traditional (155), Japanese (235), Irezumi (245), Tribal (180), Maori (195), Polynesian (200), Celtic (175), Chicano (225)
- Blackwork & Geometry: Blackwork (160), Dotwork (230), Mandala (250), Geometric (165), Sacred Geometry (195), Ornamental (185), Patternwork (170)
- Realism: Black & Grey Realism (240), Color Realism (260), Hyper Realism (275), Micro Realism (280), Portrait (290), Wildlife (255), Botanical (210)
- Artistic & Experimental: Watercolor (225), Abstract (215), Surrealism (245), Sketch (155), Brushstroke (175), Impressionist (200)
- Pop Culture: Anime/Manga (185), New School (195), Cartoon (165), Comic Book (175), Gaming (180), Movie/TV (190)
- Dark & Ornamental: Gothic (210), Dark Art (230), Horror (225), Biomechanical (265), Cyber Sigilism (255), Engraving (240), Woodcut (220)
- Custom & Mixed: Trash Polka (235), Custom Concept (300), Mixed Styles (250)

**Pro Style Explorer Features:**
- Fuzzy search with keyword matching
- Family-based browser with expandable sections
- Mix styles mode for combining multiple styles
- No numeric rate values shown to users

### Pro Mode Clamp Ranges
- Style rate constant: 0.3x - 2.0x of base constant
- Color add-on: ₱0 - ₱1,000
- Hourly rate: ₱500 - ₱10,000

### Pricing Templates System
Pricing Templates allow Pro users to save and reuse custom pricing configurations across sessions and devices.

**Features:**
- Save current settings (global multiplier + 6 rate weights) as named templates
- Templates stored in PostgreSQL for cross-device persistence
- Rename, delete, and apply templates from dedicated `/templates` page
- Apply flow uses localStorage as temporary bridge between pages
- Unique template names enforced per user

**Template Data Structure:**
- `globalMultiplier`: 0.5 - 3.0 pricing tier multiplier
- `rateWeights`: Object with 6 weight values (sizeWeight, styleWeight, visualDetailWeight, expertiseWeight, placementWeight, mixStyleWeight)
- Each weight ranges from 0.5x to 3.0x (default 1.0x)

**Database Table:** `pricing_templates`
- Columns: id, userId, name, globalMultiplier, rateWeights (JSON), createdAt, updatedAt
- Unique constraint on (userId, name)

### Build System
- Development: Vite dev server with HMR, tsx for server
- Production: Vite builds frontend to `dist/public`, esbuild bundles server to `dist/index.cjs`
- Database migrations: Drizzle Kit with `db:push` command

## External Dependencies

### Database
- **PostgreSQL**: Primary database via `DATABASE_URL` environment variable
- **Drizzle ORM**: Type-safe database queries and schema management
- **Drizzle Kit**: Database migration tooling

### UI/Component Libraries
- **Radix UI**: Accessible primitive components (dialogs, dropdowns, tooltips, etc.)
- **shadcn/ui**: Pre-styled component collection built on Radix
- **Lucide React**: Icon library
- **Framer Motion**: Animation library

### Form & Validation
- **Zod**: Schema validation for API inputs and form data
- **React Hook Form**: Form state management (via @hookform/resolvers)
- **drizzle-zod**: Generate Zod schemas from Drizzle tables

### Styling
- **Tailwind CSS**: Utility-first CSS framework
- **class-variance-authority**: Component variant management
- **tailwind-merge**: Tailwind class merging utility
- **Google Fonts**: Cinzel (display), Inter (body), JetBrains Mono (monospace)

### State & Data Fetching
- **TanStack React Query**: Server state management and caching

### Session Management
- **connect-pg-simple**: PostgreSQL session store for Express
- **express-session**: Session middleware (infrastructure ready)