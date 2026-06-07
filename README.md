# JUNG Admin Panel

[![Next.js](https://img.shields.io/badge/Next.js-15-000000?logo=nextdotjs&logoColor=white)](https://nextjs.org)
[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Supabase](https://img.shields.io/badge/Supabase-backend-3ECF8E?logo=supabase&logoColor=white)](https://supabase.com)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-3-06B6D4?logo=tailwindcss&logoColor=white)](https://tailwindcss.com)

> The web-based admin panel for the JUNG ecosystem — manage products, recommendation categories, and users from a single dashboard with role-based access control.

## Features

- **Products** — create, view, edit, and delete products; search by ID or name; server-side pagination; one-click PDF report export
- **Categories & Q&A** — manage recommendation categories, questions, and multi-answer entries; link each answer to one or more products to drive the mobile app's recommendation engine
- **User management** — list all registered users, filter by role, delete accounts; role assignment via the Supabase service role API
- **Role-based access** — three roles (`admin`, `consultant`, `authenticated`) with UI elements and write operations conditionally rendered per role
- **Session management** — persistent Supabase auth session with a React context; automatic redirect for unauthenticated users

## Related

This panel is the backend management interface for the JUNG mobile app. Both projects share the same Supabase database — content created here (products, categories, questions) is consumed directly by the mobile client:

**[JUNG Mobile](https://github.com/eco-akram/JUNG-Mobile)** — cross-platform iOS/Android/Web app built with Expo and React Native.

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 15 (App Router, Turbopack) |
| Language | TypeScript |
| Styling | Tailwind CSS + shadcn/ui (Radix UI) |
| Backend / Auth | Supabase |
| PDF Export | jsPDF + jspdf-autotable |
| Icons | Lucide React |

## Prerequisites

- [Node.js](https://nodejs.org) 18+
- A [Supabase](https://supabase.com) project with the schema from `backup.sql`

## Getting Started

**1. Clone the repository**

```bash
git clone https://github.com/eco-akram/JUNG-Admin-Pannel.git
cd JUNG-Admin-Pannel
```

**2. Install dependencies**

```bash
npm install
```

**3. Configure environment variables**

Create a `.env.local` file at the project root:

```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key
```

> [!WARNING]
> `SUPABASE_SERVICE_ROLE_KEY` grants full database access and is used server-side only (API routes). Never expose it to the client.

**4. Set up the database**

Import the schema using the Supabase CLI or paste `backup.sql` into the SQL editor in the Supabase dashboard.

**5. Start the development server**

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Project Structure

```
src/
├── app/
│   ├── (auth)/login/          # Login page
│   ├── api/auth/              # Server-side API routes (users, delete-user)
│   ├── dashboard/
│   │   ├── page.tsx           # Overview / all products
│   │   ├── products/          # Products management
│   │   ├── categories/        # Categories & Q&A management
│   │   └── users/             # User management
│   └── products/
│       ├── add/               # Add product form
│       ├── edit/[id]/         # Edit product form
│       └── view/[id]/         # Product detail view
├── components/
│   ├── AddQuestionModal.tsx   # Multi-step question + answer + product assignment
│   ├── EditQuestionModal.tsx
│   ├── AddCategoryModal.tsx
│   ├── Sidebar.tsx
│   └── ui/                    # Shared UI components (shadcn/ui + custom)
├── context/
│   └── SessionContext.tsx     # Auth session + role provider
└── utils/
    ├── authHelpers.ts
    └── dataTransformers.ts    # DB ↔ UI model mapping
```

## Roles

| Role | Permissions |
|---|---|
| `admin` | Full CRUD on products, categories, questions, and users |
| `consultant` | Read access; no create, edit, or delete |
| `authenticated` | Read access only |

Roles are stored in Supabase `user_metadata` and read from the session at runtime.

## Available Scripts

```bash
npm run dev      # Start development server with Turbopack
npm run build    # Build for production
npm run start    # Start production server
npm run lint     # Run ESLint
```
