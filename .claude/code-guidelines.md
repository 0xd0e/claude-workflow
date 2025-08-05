# IMPORTANT: CODE GUIDELINES

The code you produce will be production ready, no fallback and no unused code or migrate from old

## IMPORTANT: Code Style Guidelines

### 1. IMPORTANT: KISS (Keep It Simple, Stupid)

Write clean, straightforward code. Return early to reduce nesting. For general logic, prefer return early over try-catch. For TanStack Query functions and async operations, use throw/catch as required by the framework.

```js
const processUser = (user) => {
  if (!user) return
  if (!user.isActive) return
  return user.profile
}
```

### 2. IMPORTANT: DRY (Don't Repeat Yourself)

Extract reusable logic. Use SSOT for constants.

```ts
export const emailSchema = z.string().email()

export const userSchema = z.object({
  email: emailSchema,
})

export const newsletterSchema = z.object({
  email: emailSchema,
})
```

### 3. IMPORTANT: YAGNI (You Aren't Gonna Need It)

Only implement what's needed now. NO speculative features. NO additional improvements.

## IMPORTANT: TypeScript Code Style Guide

### IMPORTANT: 1. Parameter Passing

Always use object parameters (named parameters pattern).

```ts
function createUser({ email, name }: { email: string; name: string }) {}
```

### IMPORTANT: 2. Type Safety

IMPORTANT RULES:

- NEVER use `any`
- Use inline types for component props, place shared/reusable types into `types/` folder

### IMPORTANT: 3. Imports

- Use path aliases (`@/`) not relative paths
- NO index exports - Import directly
- NEVER use direct process.env access - Import from `@/config/env`

```ts
import { Button } from "@/components/ui/button"
import { userSchema } from "@/schemas/user"
```

### IMPORTANT: 4. Functional Programming

- NO classes - Use functional patterns

## IMPORTANT: Zod Schema Validation - Boundary Data Only

- Zod schemas are ONLY for data that crosses boundaries (API requests/responses, form inputs, external data)
- Zod schemas are the SINGLE SOURCE OF TRUTH for validation AND types for boundary data
- Use zod `safeParse` instead of `parse`
- ALWAYS infer types from Zod schemas for boundary data
- NEVER define separate interfaces for data that has Zod schemas
- Environment variables are EXCEPTED from Zod validation - centralize in `@/config/env` instead

```ts
//config/env.ts

//No zod validation env
export const env = {
  // Client-side (NEXT_PUBLIC_*)
  NEXT_PUBLIC_SUPABASE_URL: process.env.NEXT_PUBLIC_SUPABASE_URL!,
  NEXT_PUBLIC_SUPABASE_ANON_KEY: process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,

  // Server-side
  DATABASE_URL: process.env.DATABASE_URL!,
} as const
```

### IMPORTANT: TypeScript Type Organization

**DECISION MATRIX:**

1. **Boundary Data** → Use Zod schemas (API requests/responses, form inputs, external data)
2. **Reusable Types** → Extract to `types/` folder when used across multiple components
3. **Component Props** → Inline types for component-specific props

### IMPORTANT: General Guidelines

- **Extract types when reused** across multiple components (AI should decide based on usage)
- **Inline component-specific props** that aren't reused elsewhere
- **Use Zod schemas** for all boundary data validation
- **Use `import type`** for type-only imports to optimize bundle size

### IMPORTANT: Validation Rules - When to Use Zod vs TypeScript

EVERY data input that crosses boundaries (except env) MUST be validated with Zod. Use inline types for component props and TypeScript types for utilities.

### IMPORTANT: Required Validation Points

YOU MUST validate at these points:

- API request bodies
- Form inputs
- External API responses
- URL parameters
- File uploads
- User input of any kind except env

### IMPORTANT: Form Validation Pattern

```tsx
// Client-side form
import { zodResolver } from "@hookform/resolvers/zod"
import { useForm, type SubmitHandler } from "react-hook-form"
import { createUserSchema, type CreateUser } from "@/schemas/user"
import { createUserAction } from "@/actions/user"
import { toast } from "sonner"

export const UserForm = () => {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<CreateUser>({
    resolver: zodResolver(createUserSchema),
  })

  const onSubmit: SubmitHandler<CreateUser> = async (data) => {
    // data is pre-validated by Zod resolver
    const result = await createUserAction(data)

    if (result?.serverError) {
      toast("Error creating user", {
        description: result.serverError,
      })
    } else {
      toast("User created successfully!")
      // Handle success (redirect, reset form, etc.)
    }
  }

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        {...register("email")}
        type="email"
        placeholder="Email"
        disabled={isSubmitting}
      />
      {errors.email && <span>{errors.email.message}</span>}

      <input {...register("name")} placeholder="Name" disabled={isSubmitting} />
      {errors.name && <span>{errors.name.message}</span>}

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Creating..." : "Create User"}
      </button>
    </form>
  )
}
```

## IMPORTANT: React Development - Server Components First

- Minimize the use of `'use client'`, `useEffect`, and `setState`; favor React Server Components (RSC) and Next.js SSR features.
- Server Actions for internal mutations - Use for form submissions and UI-coupled operations
- Always build corresponding API routes - Create matching API routes for every Server Action to enable future external consumption and scaling
- Data Access Layer pattern - Extract shared business logic to maintain consistency between Server Actions and API routes
- API routes for all data operations - Ensure all CRUD operations have API endpoints for external apps, mobile clients, and third-party integrations
- Protect API routes for mutation endpoints

### IMPORTANT: React useEffect Guidelines

**NEVER use useEffect for:**

- **Data transformation** - Calculate derived values during rendering: `const fullName = firstName + ' ' + lastName`
- **Event handling** - Use event handlers directly, not Effects for user interactions
- **State calculations** - Derive state from props/state during rendering rather than storing in state
- **Resetting state** - Use `key` prop to reset component state or calculate values during rendering
- **Sharing logic between event handlers** - Extract to functions, don't use Effects
- **POST requests tied to specific interactions** - Handle in event handlers
- **Updating state based on other state changes** - Calculate during rendering instead
- **Inefficient computations** - Don't recalculate in Effects what can be computed directly

**Use useEffect ONLY for:**

- Synchronizing with external systems (APIs, DOM manipulation, third-party libraries)
- Side effects that should run because the component was displayed
- Fetching data on mount
- Subscribing to browser/network events
- Initializing app-wide configurations

**Performance optimizations:**

- **Calculate during rendering** for better performance than Effects
- **Use `useMemo`** for expensive computations that need caching: `useMemo(() => getFilteredTodos(todos, filter), [todos, filter])`
- **Use `key` prop** for component resets: `<Profile userId={userId} key={userId} />`
- **Minimize state updates** in Effects to avoid unnecessary re-renders

**Best practices:**

- Keep components pure - avoid side effects in render
- Lift state up when synchronization is needed between components
- Use custom hooks for reusable Effect logic
- Implement state management at the lowest necessary level
- Avoid chaining Effects that trigger each other
- Minimize dependency array complexity - only include variables actually used

### IMPORTANT: Page Structure

- Create new pages at: `src/app/PAGE_NAME/page.tsx`
- Components for the page put in the `src/components/PAGE_NAME/componentName.tsx`
- Pages are Server components so you can load data into them directly
- If you need reactivity e.g `onClick`, `onChange`, that component is a client component and file must start with `use client`

## IMPORTANT: Custom Hook Guidelines

IMPORTANT RULES:

- Purpose: Encapsulate reusable stateful logic, especially for data fetching
- Location: Place custom hooks in the `src/hooks/` directory
- Naming: Use the `use` prefix (e.g., `useAccounts.ts`)
- Data Fetching: Use fetch in Server Components for static or dynamic server-side data fetching to maximize performance and reduce client bundle size, and `@tanstack/react-query` in Client Components when interactivity or client-side revalidation is needed.
- Simplicity: Keep hooks focused on a single responsibility

## IMPORTANT: Data Fetching

### IMPORTANT: fetch (Default Choice)

Default to using fetch in Server Components for all static or dynamic data fetching.

```ts
async function Page() {
  const res = await fetch("https://api.example.com/users", {
    cache: "no-store", // or "force-cache" as needed
  })
  const data = await res.json()

  return <UserList users={data} />
}
```

### IMPORTANT: TanStack Query (Only When Interactivity Needed)

USE ONLY IF: The component requires interactivity, client-side revalidation, or real-time updates.

## IMPORTANT: State Management - Zustand for Client State

- Use Zustand for client application state management (UI state, user preferences, etc.). Use TanStack Query for server state.

### IMPORTANT: Usage Pattern

```tsx
// Component usage
const { users, addUser, loading } = useUserStore()

// Selective subscription
const selectedUser = useUserStore((state) => state.selectedUser)
```

## IMPORTANT: Utility Functions

- Use lodash utilities for common operations (arrays, objects, strings)
- Prefer using a well-tested library function over custom. Create custom utilities only when there is no library for that specific operation.
- Import specific lodash functions to minimize bundle size:
  ```typescript
  import groupBy from "lodash/groupBy"
  ```
- Create utility functions in `src/utils/` folder for reusable logic

```ts
import groupBy from "lodash/groupBy"
import debounce from "lodash/debounce"
import isEmpty from "lodash/isEmpty"

// utils/validation.ts
export function isValidEmail(email: string): boolean {
  return emailSchema.safeParse(email).success
}

// utils/formatters.ts
export function formatCurrency(amount: number): string {
  return new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD",
  }).format(amount)
}
```

## IMPORTANT: UI/UX Guidelines

IMPORTANT: ALWAYS refer to `globals.css` for color definitions and variables. Use shadcn design system.

### IMPORTANT: Responsive Design

- Mobile-first approach (base classes for mobile, prefixed classes for larger screens)
- Use responsive prefixes: `sm:`, `md:`, `lg:`, `xl:`, `2xl:`
- Use flex for layouts
- Use gap and space utilities instead of margins between flex children

### IMPORTANT: Tailwind CSS

- Mobile first Responsive Design pattern using Tailwind breakpoints and flex layout
- Avoid using `container` class in tailwindCSS
- Avoid Arbitrary Values: Favor Tailwind's predefined scale values
- Avoid custom CSS unless absolutely necessary
- Spacing pattern favor use `space` and `gap`

### IMPORTANT: UI Framework

- Use Shadcn UI and Tailwind for components and styling
- Use icons from `lucide-react` for consistent iconography
- Use `Image` from `next/image` for optimized images

### IMPORTANT: Accessibility (a11y)

- Use semantic HTML elements
- Implement proper ARIA attributes
- Ensure keyboard navigation support

### IMPORTANT: Loading Components

Use the `LoadingContent` or `skeleton` component to handle loading states

```tsx
<Card>
  <LoadingContent loading={isLoading} error={error}>
    {data && <MyComponent data={data} />}
  </LoadingContent>
</Card>
```

### Font Configuration

Current font configuration (DM Sans):

```typescript
import { DM_Sans } from "next/font/google"

const dmSans = DM_Sans({
  variable: "--font-dm-sans",
  subsets: ["latin"],
})

<body className={`${dmSans.variable} font-sans antialiased`}>
  {/* Content */}
</body>
```

To change to a different font:

```typescript
import { Inter } from "next/font/google"

const fontSans = Inter({
  variable: "--font-sans",
  subsets: ["latin"],
})

<body className={`${fontSans.variable} font-sans antialiased`}>
  {/* Content */}
</body>
```

### IMPORTANT: Shadcn UI Components

Always use Shadcn UI components. Install with:

```bash
npx shadcn@latest add [component-name]
```

Components: `accordion, alert, alert-dialog, aspect-ratio, avatar, badge, breadcrumb, button, calendar, card, carousel, chart, checkbox, collapsible, combobox, command, context-menu, data-table, date-picker, dialog, drawer, dropdown-menu, hover-card, input, input-otp, label, menubar, navigation-menu, pagination, popover, progress, radio-group, resizable, scroll-area, select, separator, sheet, skeleton, slider, sonner, switch, tabs, table, textarea, toggle, toggle-group, tooltip, typography`

### prompt-kit

`prompt-kit` extends shadcn with AI interface components:

```bash
npx shadcn@latest add "https://prompt-kit.com/c/[COMPONENT-NAME].json"
```

Components: `prompt-input, code-block, markdown, message, chat-container, scroll-button, loader, prompt-suggestion, response-stream, reasoning, file-upload, jsx-preview`

## IMPORTANT: NEVER Write Mock Data or Fallbacks

- NEVER create mock data, placeholder, stubs, or fallbacks into the source code. All test-related mocks and fixtures must reside exclusively within the test/ directory.
- Always implement real functionality or leave TODO comments.

```ts
// TODO: Implement user fetching from API endpoint /api/users
const users = []
```

## IMPORTANT: File Structure & Organization

### IMPORTANT: Naming Conventions

- Directories: `kebab-case` (e.g., `user-authentication/`)
- Variables & Functions: `camelCase` (e.g., `getUserData`)
- React Components: `PascalCase` (e.g., `UserProfile.tsx`)
- Routes: `kebab-case` (e.g., `api/hello-world/route.ts`)

### Directory Structure

```bash
src/
├── app/                    # Next.js App Router
│   ├── (auth)/            # Route group - auth pages
│   ├── (dashboard)/       # Route group - protected pages
│   ├── (public)/          # Route group - public pages
│   └── api/               # API routes
├── components/            # React components
│   ├── ui/               # Shadcn UI components
│   ├── auth/             # Auth-specific components
│   └── [feature]/        # Feature-specific components
├── schemas/              # Zod schemas (SSOT for validation & types)
├── config/               # Static constants and configuration
├── db/                   # Database schema and connection
├── server-actions/       # Next.js Server Actions
├── hooks/                # Custom React hooks
└── lib/                  # Third-party library configurations
```

## IMPORTANT: General Principles

- Concise and Modular Code under 500 lines
- Functional Programming patterns preferred over class-based structure
- Reusable Components & Helper Functions
- Descriptive Naming for functions and variables
- Organized File Structure: `components`, `config`, `types`, `stores`, `hooks`, `server-actions`, `libs`
- Shared components: Place in the `components` folder if used in many places

## Testing and Quality

### Testing Framework

- **Vitest**: Modern testing framework
- **Testing Library**: React component testing utilities
- **JSDOM**: Browser environment simulation

### Test Structure (Centralized)

```
test/
├── setup/                   # Test configuration and setup
├── fixtures/                # Test data and fixtures
├── helpers/                 # Test utilities and helpers
├── unit/                    # Unit tests (80% - fast, isolated)
├── integration/             # Integration tests (20% - component interactions)
└── e2e/                     # End-to-end tests (only when explicitly requested)
```

### Testing Rules

- **Default Testing**: Focus on unit and integration tests only (80/20 split: 80% unit tests, 20% integration tests)
- **E2E Testing**: Only implement e2e tests when explicitly requested by the user
- **Centralized Structure**: All tests in `test/` folder, mirroring `src/` structure
- **File Naming**: `*.test.ts` or `*.test.tsx` for all test files
- **Fixtures**: Use type-safe fixtures with `test.extend()` for reusable test data
- **Isolation**: Each test should be independent and not rely on other tests
- **Coverage Target**: Aim for 80%+ overall coverage, with unit tests covering business logic and integration tests covering critical user flows

### What to Test (Examples)

Unit Tests (80%) - Single component/function isolation

- Component rendering and props handling
- Utility functions and business logic
- Form validation and state management
- Schema validation (Zod schemas)
- Custom hooks behavior
- State management (Zustand stores, React state)
- Event handlers and user interactions

Integration Tests (20%) - Multi-component interactions

- API endpoints (POST, GET, PUT, DELETE) with mocked external services
- Database operations with test database
- Form submission with validation and server actions
- Component composition (parent-child component interactions)
- Authentication middleware with protected routes

E2E Tests (Only when explicitly requested) - Complete user journeys

- Full authentication flow (sign up → verify → sign in → dashboard)
- Blog management workflow (create → edit → publish → view)
- Core business processes (user onboarding, payment flows)
- Cross-browser compatibility for critical features

### ❌ What You SHOULD NOT Test

1. Implementation Details

- Avoid asserting internal state or function calls
- Focus on what the user sees or gets, not how it's done

❌ Bad:
expect(setState).toHaveBeenCalledWith("loading")

✅ Good:
expect(screen.getByText("Loading...")).toBeInTheDocument()

2. 3rd-Party Library Internals

- Do not test what you don't own
- ❌ Examples: Zod's internal validation logic, Axios response formatting, Tailwind behavior
- ✅ Only test how our code uses them

3. Static Markup and Styles

- Avoid snapshot tests of static components
- Avoid testing CSS class names unless they affect functionality

4. Generated or Boilerplate Code

- No need to test Next.js config, Tailwind config, ESLint, or auto-generated types

5. Over Testing

- Test common use cases and 2-3 important edge cases only
- Avoid testing all permutations of optional props or config combinations
- Focus on what users are most likely to do, not exhaustive internal branches
- Don't test things just to increase coverage numbers — value > volume

### Configuration

**Installation:**

```bash
npm install -D vitest @vitejs/plugin-react jsdom @testing-library/react @testing-library/dom vite-tsconfig-paths @vitest/coverage-v8
```

**vitest.config.ts:**

```typescript
/// <reference types="@vitest/browser/context" />
import { defineConfig } from "vitest/config"
import react from "@vitejs/plugin-react"
import tsconfigPaths from "vite-tsconfig-paths"

export default defineConfig({
  plugins: [tsconfigPaths(), react()],
  test: {
    environment: "jsdom",
    coverage: {
      reporter: ["text", "json", "html"],
    },
    // Browser testing (optional)
    // browser: {
    //   enabled: true,
    //   provider: 'playwright',
    //   instances: [{ browser: 'chromium' }]
    // }
  },
})
```

tsconfig.json

```typescript
{
  "compilerOptions": {
    "types": ["vitest/globals", "vitest/jsdom"],

}
```

### Best Practices

- **Mock External Services**: Isolate dependencies for faster, predictable tests
- **Type-Safe Fixtures**: Use TypeScript interfaces with `test.extend()` for reusable test data
- **Object Destructuring**: Always destructure context in fixture functions and tests
- **Cleanup Pattern**: Fixtures support setup → use → teardown lifecycle
- **Module Mocking**: Use `vi.mock()` for external dependencies like Supabase client
- **Browser Testing**: Use Vitest built-in Playwright provider for real browser testing when needed
- **Custom Matchers**: Extend `expect` with domain-specific assertions using `expect.extend()`
- **Test Lifecycle**: Use `beforeAll/afterAll` and `beforeEach/afterEach` for setup/teardown
- **Conditional Tests**: Use `test.skipIf()` and `test.runIf()` for environment-specific tests
- **Module Import Errors**: Install `vite-tsconfig-paths` for path alias support
- **Mock Behavior**: Remember Vitest `mockReset()` resets to original implementation (not empty function)

## IMPORTANT: Content Guidelines

### Headline Design

- Make it BIG: `text-4xl md:text-6xl` (h1), `text-3xl md:text-5xl` (h2)
- Make it BOLD
- One headline per section
- Can highlight numbers and key words with primary color

### Spacing System

- 4-point increments. Example: `space-x-4`, `space-y-4`, `space-x-8`, `space-y-8`, `space-x-12`, `space-y-12`
- Semantic spacing: paragraph to headline, section to section
- When in doubt, add space

### Headlines & Titles

- Start with clear benefit statements
- Use contrast to emphasize key words
- Keep under 10 words when possible
- Example: "Learn to [action] in [short timeframe], not [long timeframe]"

### Social Proof Elements

- Include detailed testimonials with specific results/metrics
- Show counts prominently
- Highlight financial results
- Use video testimonials when possible

### Value Proposition

- Lead with time savings
- Contrast traditional vs accelerated
- Highlight AI integration
- Show concrete path

### Visual Hierarchy

- Size indicates importance
- Color contrast guides attention
- Spacing creates relationships
- Consistent styling throughout

## Copywriting Guidelines

### Headlines & Titles

- Start with clear benefit statements
- Use contrast to emphasize key words (e.g. "weeks not months")
- Keep under 10 words when possible
- Example formats:
  - "Learn to [action] in [short timeframe], not [long timeframe]"
  - "From [starting point] to [result] in [timeframe]"
  - "[Number] students learned to [action], fast"

### Paragraph Structure

- Single sentence paragraphs preferred
- Maximum 2-3 sentences per paragraph
- Use emoji sparingly for emphasis (1-2 per section)
- Bullet points for lists of features/benefits

### Social Proof Elements

- Include detailed testimonials with:
  - Specific results/metrics e.g. $ amounts, user counts
  - Timeframe e.g built in 3 weeks
- Show counts prominently e.g 3,086 users
- Highlight financial results e.g $400 MRR, $162.5 in sales
- Use video testimonials when possible

### Value Proposition (Education Focus)

- Lead with time savings e.g 12 hours of video vs 64 hours elsewhere
- Contrast traditional vs accelerated learning
- Highlight AI integration e.g AI writes and fixes code for you
- Include:
  1. Problem with traditional approach
  2. Your accelerated approach
  3. AI advantage
  4. success stories
  5. Detailed breakdown

### Pricing Section

- Simple tiered pricing
- Highlight savings/discounts
- Include:
  - Price
  - Billing frequency
  - Key features
  - CTA button
- Add comparison to show value

### Visual Style

- Consistent color scheme
- High contrast text
- Ample white space
- Mobile-first responsive design
- Illustrations/icons to break up text

## Additional Tools

- Use Supabase MCP for any task involving Supabase and database
- Use browseruse MCP for frontend testing, browser logs, screenshots, visiting URLs
- Use context7 to get the latest documentation of a library
- Use Bash `ast-grep` command for a search that requires syntax-aware or structural matching, default to `ast-grep --lang typescript -p 'searchpattern'` (or set `--lang` appropriately) and avoid falling back to text-only tools unless user explicitly request a plain-text search.

## IMPORTANT: Git Convention

### IMPORTANT: Branch Naming Convention

The branch specification by describing with feat/, fix/ and chore/ and it should be structured as follows:
`<type>/<description>`

- main: The main development branch
- feat/: For new features (e.g., feat/add-login-page)
- fix/: For bug fix (e.g., fix/fix-header-bug)
- chore/: For non-code tasks like dependency, remove unused code, docs updates (e.g., chore/update-dependencies)

### IMPORTANT: Commit Convention

The commit message should be structured as follows:

<type>[scope]: <description>

[body]

[optional footer]
