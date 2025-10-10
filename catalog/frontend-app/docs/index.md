# Frontend Application

## Overview

The Frontend App is our customer-facing web application built with React, TypeScript, and modern web technologies. It provides a fast, responsive, and accessible user interface for our platform.

## Key Features

- **Modern React**: Built with React 18+ and functional components
- **Type Safety**: Full TypeScript coverage for reliability
- **State Management**: Redux Toolkit for predictable state
- **Routing**: React Router v6 for navigation
- **Styling**: Tailwind CSS for utility-first styling
- **Component Library**: Custom design system components
- **Internationalization**: Multi-language support (i18n)
- **Accessibility**: WCAG 2.1 AA compliant

## Tech Stack

| Layer | Technology | Version |
|-------|------------|---------|
| Framework | React | 18.2+ |
| Language | TypeScript | 5.0+ |
| Build Tool | Vite | 5.0+ |
| State | Redux Toolkit | 2.0+ |
| Routing | React Router | 6.20+ |
| Styling | Tailwind CSS | 3.4+ |
| Forms | React Hook Form | 7.49+ |
| HTTP Client | Axios | 1.6+ |
| Testing | Vitest + RTL | Latest |

## Project Structure

```
frontend-app/
├── public/              # Static assets
├── src/
│   ├── assets/         # Images, fonts, icons
│   ├── components/     # Reusable UI components
│   │   ├── common/     # Generic components
│   │   ├── forms/      # Form components
│   │   └── layout/     # Layout components
│   ├── features/       # Feature-based modules
│   │   ├── auth/       # Authentication
│   │   ├── dashboard/  # Dashboard views
│   │   └── profile/    # User profile
│   ├── hooks/          # Custom React hooks
│   ├── lib/            # Utility libraries
│   ├── pages/          # Page components
│   ├── services/       # API services
│   ├── store/          # Redux store
│   ├── styles/         # Global styles
│   ├── types/          # TypeScript types
│   ├── App.tsx         # Root component
│   └── main.tsx        # Entry point
├── tests/              # Test utilities
├── .env.example        # Environment template
├── package.json
├── tsconfig.json
└── vite.config.ts
```

## Quick Start

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Run tests
npm test
```

## Environment Variables

Create a `.env.local` file:

```bash
VITE_API_BASE_URL=http://localhost:7007
VITE_APP_NAME="My Platform"
VITE_ENABLE_ANALYTICS=false
VITE_SENTRY_DSN=your-sentry-dsn
```

## Features

### Authentication
- Login/logout flows
- Social authentication (Google, GitHub)
- Password reset
- Email verification

### Dashboard
- Overview metrics
- Recent activity feed
- Quick actions
- Customizable widgets

### User Profile
- Profile editing
- Avatar upload
- Preferences management
- Security settings

## Performance

- **Bundle Size**: < 200KB (gzipped)
- **First Contentful Paint**: < 1.5s
- **Time to Interactive**: < 3.5s
- **Lighthouse Score**: 90+

## Browser Support

- Chrome (last 2 versions)
- Firefox (last 2 versions)
- Safari (last 2 versions)
- Edge (last 2 versions)
