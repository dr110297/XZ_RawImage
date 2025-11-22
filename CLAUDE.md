# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**xzTimes (PixAI)** is a commercial-grade AI Image Generation Platform built with Vue 3, TypeScript, Element Plus, and Tailwind CSS. It features a modern light/SaaS theme UI fully localized in Chinese, with AI-powered text-to-image and image-to-image generation capabilities.

## Development Commands

### Setup
```bash
npm install
```

### Development Server
```bash
npm run dev
```
The app will be available at http://localhost:5173

### Build for Production
```bash
npm run build
```
This runs TypeScript compilation (`vue-tsc -b`) followed by Vite build.

### Preview Production Build
```bash
npm run preview
```

## Tech Stack

- **Framework**: Vue 3.4.21 + TypeScript 5.2.2
- **Build Tool**: Vite 5.1.4
- **UI Library**: Element Plus 2.6.1 with Element Plus Icons
- **Styling**: Tailwind CSS 3.4.1
- **Icons**: Lucide Vue Next 0.344.0
- **Routing**: Vue Router 4.3.0
- **Utilities**: @vueuse/core, clsx, tailwind-merge

## Architecture

### Layout System
The app uses a three-layout architecture managed by Vue Router:

1. **MainLayout** (`src/layouts/MainLayout.vue`)
   - Used for public pages (Home, Gallery)
   - Includes navbar and footer
   - Routes: `/`, `/gallery`

2. **AuthLayout** (`src/layouts/AuthLayout.vue`)
   - Used for authentication pages (Login, Register)
   - Centered content layout
   - Routes: `/login`, `/register`

3. **DashboardLayout** (`src/layouts/DashboardLayout.vue`)
   - Used for authenticated app pages (Generate, UserCenter)
   - Features a collapsible sidebar with navigation
   - Routes: `/generate`, `/profile`
   - Sidebar state controlled by `isCollapsed` ref
   - Sidebar width: 256px (expanded) / 80px (collapsed)

### State Management
The app uses Vue's reactive API for state management:

- **Favorites Store** (`src/store/favorites.ts`)
  - LocalStorage-persisted reactive store
  - Manages favorites and folders
  - Keys: `xztimes_favorites`, `xztimes_folders`
  - Uses Vue `watch` with `deep: true` for persistence
  - All operations are synchronous and update localStorage automatically

### Router Configuration
Router uses `createWebHistory()` for clean URLs. Lazy-loading is used for page components via dynamic imports (`() => import('@/pages/...')`).

### Utility Functions
- **cn()** (`src/lib/utils.ts`): Combines clsx and tailwind-merge for conditional Tailwind classes

## Key Pages

### Generate Page (`/generate`)
The main AI generation workspace with:
- **Unified Interface**: Merged text-to-image and image-to-image workflows
- **Session History**: Right sidebar displays conversation sessions
- **Chat Interface**: Main area renders full conversation history
- **Reference Media**: Upload area with image/video toggle
- **Amazon Suite Mode**: One-click setup for generating 6 product images
- **Creative Tools**: Art Style, Aspect Ratio, Count controls in left sidebar
- **Lightbox**: Full-screen image preview
- **Gallery Tab**: Integrated gallery sidebar using `GalleryCard`

### Gallery Page (`/gallery`)
- Masonry layout using `GalleryCard` component
- Detail modal via `GalleryModal` with Favorite, Copy Prompt, and "Use as Reference" actions

### User Center (`/profile`)
- Favorites management with folder filtering
- Basic profile editing

### Authentication
- **Login**: Supports Phone/Username authentication
- **Register**: Includes Phone/Username, Password, Confirm Password, and CAPTCHA

## Reusable Components

### GalleryCard (`src/components/ui/GalleryCard.vue`)
Unified gallery item card with hover actions. Used in both Gallery page and Generate page sidebar.

### GalleryModal (`src/components/ui/GalleryModal.vue`)
Unified detail modal for gallery items with actions (Favorite, Copy Prompt, Use as Reference).

### Dialog (`src/components/ui/Dialog.vue`)
Reusable dialog component.

## Styling

### Hybrid Styling Approach
The app uses a combination of Tailwind CSS, custom CSS variables, and Element Plus components:

**Tailwind Configuration** (`tailwind.config.js`):
- Custom colors: `primary`, `text.*`, `bg.*`, `border.*`, `danger`, `success`, `warning`
- Custom border radius: `base` (8px), `small` (4px), `round` (9999px)
- Custom shadows: `base`, `md`, `lg`, `glow`
- Inter font family
- Extends default theme rather than replacing it

**Custom CSS** (`src/styles/common.css`):
- CSS variables for all design tokens (colors, spacing, etc.)
- Utility classes that mirror Tailwind conventions
- Custom scrollbar styling (`.scrollbar-thin`)
- Element Plus button overrides
- Responsive breakpoints (sm, md, lg, xl)
- Special effects (backdrop-blur, gradients, animations)

**Element Plus Integration** (`src/index.css`):
- Custom button styles using Tailwind `@layer components`
- `.el-button-primary` and `.el-button-default` overrides for consistent theming
- All Element Plus icons globally registered in `main.ts`

### Path Alias
`@` is aliased to `./src` in Vite config for cleaner imports.

## Important Conventions

1. **All UI text is in Chinese** - Maintain Chinese localization for all user-facing strings
2. **Element Plus integration** - All Element Plus icons are globally registered in `main.ts`
3. **Component naming** - Use PascalCase for component files (e.g., `GalleryCard.vue`, not `galleryCard.vue`)
4. **State persistence** - The favorites store auto-persists to localStorage; no manual save needed
5. **Mock data** - History and generation features currently use mock data (backend integration pending)
6. **Layout consistency** - Use appropriate layout wrapper (MainLayout/AuthLayout/DashboardLayout) for new pages
7. **Import path alias** - Always use `@/` for imports from src (e.g., `import { cn } from '@/lib/utils'`)
8. **Lazy loading** - All pages are lazy-loaded via `() => import('@/pages/...')` in router config

## Component Development Guidelines

### When Creating New Components
- Place reusable UI components in `src/components/ui/`
- Use Vue 3 Composition API with `<script setup lang="ts">`
- Define props with `defineProps<T>()` for type safety
- Use `cn()` utility for conditional Tailwind classes
- Emit events with `defineEmits<T>()`

### When Modifying Pages
- Pages are located in `src/pages/` and wrapped by layouts
- Use computed properties for derived state
- Integrate with favorites store when needed via `import { store } from '@/store/favorites'`
- Maintain existing styling patterns (backdrop-blur, gradient effects, shadows)

## Data Flow Patterns

### Favorites Management
```typescript
import { store } from '@/store/favorites'

// Check if item is favorited
const isFavorite = computed(() => store.isFavorite(item.id))

// Toggle favorite
store.toggleFavorite(item)

// Remove favorite
store.removeFavorite(item.id)

// Create folder
store.createFolder(name)

// Move to folder
store.moveToFolder(itemId, folderId)
```

### Router Navigation with State
When navigating to Generate page with pre-filled data:
```typescript
router.push({
  path: '/generate',
  state: {
    prompt: item.prompt,
    referenceImage: item.src,
    autoGenerate: true  // or false to just pre-fill
  }
})
```

## Future Integration Notes

- Backend API integration planned (Youware Backend)
- Persistent state for history (currently mock)
- Video playback support in history
- Authentication guards for protected routes
- Credits system deduction logic
- Image upload backend storage integration

## TypeScript Types

Key interfaces used throughout the app:

```typescript
interface FavoriteItem {
  id: string | number
  src: string
  title?: string
  prompt?: string
  type: 'image' | 'video'
  style?: string
  folderId?: string
  createdAt: number
}

interface Folder {
  id: string
  name: string
  createdAt: number
}
```

## Known Limitations

1. **No authentication guards** - Protected routes (`/generate`, `/profile`) don't check auth state
2. **Mock data everywhere** - Gallery items, history sessions, and generation results use static mock data
3. **No real API calls** - All backend interactions are stubbed out with TODO comments
4. **Video playback incomplete** - Video indicator shown but playback not implemented
5. **README.md outdated** - Contains generic React template description instead of this Vue project
