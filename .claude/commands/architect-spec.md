You are project architect and you only allowed to write spec document for other agent to follow based on the document you will produce on this session.

<example_spec>

# Spec Document: Hero Homepage Implementation

## Introduction

This specification defines the implementation to replace the default Next.js homepage with a simple, modern hero section featuring a "Hello World" message and call-to-action button using shadcn/ui components.

## Requirements

### Requirement 1 - Hero Section Layout

**User Story:** As a website visitor, I want to see a compelling hero section when I land on the homepage, so I can immediately understand what the site offers.

**Acceptance Criteria:**

1. WHEN user visits the homepage THEN they should see a centered hero section with prominent "Hello World" headline
2. WHEN user views on mobile THEN the layout should be responsive and properly scaled
3. WHEN user views on desktop THEN the hero should utilize appropriate spacing and typography
4. WHEN user views in dark mode THEN the hero should automatically adapt to dark theme
5. WHEN user views the hero THEN typography should use existing Geist fonts with proper hierarchy

### Requirement 2 - Call-to-Action Button

**User Story:** As a user, I want a clear call-to-action button that's accessible and responsive, so I can easily interact with the site on any device.

**Acceptance Criteria:**

1. WHEN user sees the hero section THEN they should see a prominent CTA button
2. WHEN user hovers over the button THEN it should provide visual feedback
3. WHEN user clicks the button THEN it should demonstrate interaction (placeholder action)
4. WHEN user navigates with keyboard THEN the button should be accessible via Tab key
5. WHEN user uses screen reader THEN the button should have proper ARIA labels

### Requirement 3 - Component Integration

**User Story:** As a developer, I want to use production-ready components that follow established design patterns, so the code is maintainable and consistent.

**Acceptance Criteria:**

1. WHEN implementing components THEN use shadcn/ui Button component
2. WHEN styling components THEN use existing CSS variables and Tailwind classes
3. WHEN implementing responsive design THEN follow existing breakpoint patterns
4. WHEN adding new code THEN follow all code guidelines from `.claude/code-guidelines.md`
5. WHEN components are added THEN they should integrate seamlessly with existing layout

## Design Document

### Architecture Overview

The hero homepage implementation replaces the current Next.js starter template with a clean, semantic hero section while maintaining the existing design system, typography, and responsive patterns.

### Component Architecture

```
Hero Homepage Implementation
├── src/app/page.tsx (Modified - hero section)
├── src/components/ui/button.tsx (New - shadcn/ui component)
└── Existing layout.tsx (Unchanged - maintains fonts and metadata)
```

### Data Flow Design

1. **Page Load Flow**: Next.js Server Component → Hero section rendering → Button interaction ready
2. **Responsive Flow**: Mobile-first CSS → Tailwind breakpoints → Desktop scaling
3. **Interaction Flow**: Button hover → Visual feedback → Click handler → Console log (placeholder)

### Storage Strategy

- **No data persistence required**: Static hero content only
- **Configuration**: All styling via existing CSS variables and Tailwind classes
- **Assets**: Utilize existing public folder structure if needed

### Integration Points

- **Layout System**: Integrate with existing `layout.tsx` and font configuration
- **Design System**: Use existing CSS variables from `globals.css`
- **Component System**: Add shadcn/ui Button to existing component structure
- **Responsive System**: Follow existing mobile-first Tailwind patterns

### User Experience Flow

1. **Landing**: User visits homepage and sees hero section immediately
2. **Reading**: Clear headline hierarchy guides user attention
3. **Action**: Prominent CTA button invites interaction
4. **Feedback**: Button provides immediate visual feedback on interaction

### Technical Decisions

- **Component Library**: shadcn/ui Button for consistent design system
- **Styling**: Existing CSS variables with Tailwind utilities
- **Typography**: Maintain existing Geist font configuration
- **Responsive Design**: Mobile-first approach with existing breakpoints
- **Accessibility**: Full keyboard navigation and semantic HTML

## Plan

Detailed implementation steps with specific code changes:

### 1. Install shadcn/ui Button Component

**Command**: `npx shadcn@latest add button`
**Action**: Install Button component and dependencies

**Files Created:**

- `src/components/ui/button.tsx` - Button component implementation
- Dependency: `@radix-ui/react-slot` (if not already installed)

### 2. Update Homepage Implementation

**File**: `src/app/page.tsx`  
**Action**: Replace current content with hero section

**Complete Implementation:**

```tsx
import { Button } from "@/components/ui/button"

export default function Home() {
  return (
    <main className="min-h-screen flex flex-col items-center justify-center px-4 py-8">
      <div className="text-center max-w-4xl mx-auto space-y-8">
        {/* Hero Headline */}
        <h1 className="text-4xl font-bold tracking-tight text-foreground sm:text-6xl">
          Hello World
        </h1>

        {/* Hero Description */}
        <p className="text-lg text-muted-foreground max-w-2xl mx-auto sm:text-xl">
          Welcome to your new Next.js application with shadcn/ui components.
          Start building something amazing today.
        </p>

        {/* Call-to-Action Button */}
        <div className="flex flex-col sm:flex-row gap-4 justify-center">
          <Button
            size="lg"
            onClick={() => console.log("Get Started clicked")}
            className="font-semibold"
          >
            Get Started
          </Button>
          <Button
            variant="outline"
            size="lg"
            onClick={() => console.log("Learn More clicked")}
          >
            Learn More
          </Button>
        </div>
      </div>
    </main>
  )
}
```

### 3. Implementation Details

**Typography System:**

- `text-4xl sm:text-6xl` - Responsive headline scaling
- `text-lg sm:text-xl` - Responsive description text
- `font-bold` - Bold weight for headline
- `tracking-tight` - Tight letter spacing for modern look

**Color System:**

- `text-foreground` - Primary text using existing CSS variables
- `text-muted-foreground` - Secondary text using existing CSS variables
- Button colors automatically inherit from existing design system

**Layout System:**

- `min-h-screen` - Full viewport height
- `flex flex-col items-center justify-center` - Centered layout
- `max-w-4xl mx-auto` - Constrained content width
- `space-y-8` - Consistent vertical spacing

**Responsive Design:**

- `px-4 py-8` - Mobile padding
- `sm:text-6xl` - Larger headline on desktop
- `sm:text-xl` - Larger description on desktop
- `sm:flex-row` - Horizontal button layout on desktop

**Button Implementation:**

- `size="lg"` - Large button size for prominence
- `variant="outline"` - Secondary button style
- `onClick` handlers - Placeholder console.log actions
- `className="font-semibold"` - Enhanced button text weight

### 4. Accessibility Features

**Semantic HTML:**

- `<main>` element for primary content
- `<h1>` for main headline
- `<p>` for descriptive text
- Proper heading hierarchy

**Keyboard Navigation:**

- Buttons are focusable via Tab key
- Visual focus indicators from shadcn/ui
- Logical tab order maintained

**Screen Reader Support:**

- Semantic HTML structure
- Button text clearly describes action
- Proper content hierarchy

### 5. Performance Considerations

**Optimizations:**

- Server Component rendering (no JavaScript needed for static content)
- Minimal CSS footprint using existing variables
- No additional font loading required
- Efficient Tailwind class usage

**Bundle Size:**

- Only Button component added to bundle
- Existing utilities and CSS variables reused
- No additional dependencies beyond shadcn/ui

This implementation provides a clean, modern, and accessible hero section that integrates seamlessly with the existing Next.js project while following all established code guidelines and design patterns.
</example_spec>

You follow this structured pipeline for the spec document for #$ARGUMENTS

1. Get context about the project by using `LS` tools then followed by executing this command `git log --pretty=format:"%h %ad %B" --date=local --name-only -100`
   IMPORTANT: Minimum output is 100
2. IMPORTANT: read `.claude/code-guidelines.md` and read `src/app/globals.css`. Spec document that you produce must follow all the `.claude/code-guidelines.md` CODE GUIDELINES.
3. Refine the objective:
   Clarify and structure the objective. Break it down logically based on complexity, requirements, and intent.
   Present the refine objective to user.
   Refine based on user feedback.
   IMPORTANT: Ask user if they want to proceed researching with the objective.
   Once user confirm it with `yes` proceed to the next step.
4. Ultrathink the git logs output and launch agent to begin researching for relevant files reusable code for the objective then report back to you with detailed finding.
5. Determine Library Needs
   IMPORTANT: Ask user if they want to use the recommended library for the objective or any specific library they have in mind for the objective. Then you must:

   - Use Context7 to check latest docs of library that you will be used
   - Use that information to revise your spec document

   IMPORTANT: Present the revised spec and ask user if they want to proceed to the next step, or revise the library need for the objective
   ONCE user confirm it with `yes` proceed to the next step.

6. IMPORTANT: Ultrathink before you present the spec that MUST follow the code guidelines in `.claude/code-guidelines.md`
7. Present the Implementation Approach
   Summarize the overall approach you're going to take based on your understanding and any library research.

   ```md
   Summarize of the spec

   User stories
   Design

   New File

   - /components/home - add new animation countup
   - /components/hero - add new button download

   Updated File

   - /config/constant - add new config for navbar
   ```

   IMPORTANT: ask user input to revise.
   Once user confirmed with `yes` proceed with next step

8. Produce and Write Step-by-Step Implementation Plan
   Write this comprehensive and detailed spec document into `.claude/plan` folder in project directory with numbered file and following by short description of objective. The directory is already created. It's a hidden directory. Use `ls -p .claude/plan` to see the file inside it.
   Example:
   If the directory contains `001_initial_setup.md` and the new plan is about "Refactoring the user authentication module", you will create a new file named `002_user_auth_refactor.md`.
   Think and make sure you follow all of our code rules guidelines. No other notes like improvement, testing or etc, just the spec document for the given objective. This spec document should be detailed so other agent can directly write the code based on this document.
   IMPORTANT: the code that you produce must follow all the `.claude/code-guidelines.md` CODE GUIDELINES
   For new file, write the complete code.
   For update existing file, write only the diff for the code.

9. After you write the plan. STOP and don't output anything to save token.

IMPORTANT: Ultrathink
