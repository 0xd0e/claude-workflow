# Requirements Document

## Introduction

This feature involves migrating the main PancakeSwap web application from its current UI system (styled-components + custom uikit) to shadcn/ui, a modern component library built on Radix UI primitives and Tailwind CSS. This migration aims to improve developer experience, maintainability, and provide a more consistent design system while preserving the existing PancakeSwap brand identity and user experience.

## Requirements

### Requirement 1

**User Story:** As a developer, I want to use shadcn/ui components instead of the current uikit, so that I can leverage modern, accessible, and well-maintained UI primitives.

#### Acceptance Criteria

1. WHEN setting up the project THEN the system SHALL install and configure shadcn/ui with Tailwind CSS
2. WHEN configuring shadcn/ui THEN the system SHALL maintain PancakeSwap's existing color scheme and branding
3. WHEN installing components THEN the system SHALL use shadcn/ui CLI to add components to the project
4. IF existing styled-components conflict with Tailwind THEN the system SHALL provide a migration strategy

### Requirement 2

**User Story:** As a user, I want the application to maintain the same visual appearance and functionality, so that my experience remains consistent during the UI migration.

#### Acceptance Criteria

1. WHEN viewing any page THEN the visual design SHALL remain identical to the current implementation
2. WHEN interacting with components THEN all functionality SHALL work exactly as before
3. WHEN using responsive features THEN mobile and desktop layouts SHALL remain unchanged
4. IF animations exist THEN they SHALL be preserved or improved with shadcn/ui equivalents

### Requirement 3

**User Story:** As a developer, I want core components to be migrated systematically, so that the transition is manageable and doesn't break existing functionality.

#### Acceptance Criteria

1. WHEN migrating components THEN the system SHALL start with foundational components (Button, Input, Card)
2. WHEN replacing components THEN the system SHALL maintain the same props interface where possible
3. WHEN components are migrated THEN existing imports SHALL continue to work through re-exports
4. IF breaking changes are necessary THEN the system SHALL provide clear migration guides

### Requirement 4

**User Story:** As a developer, I want the design system to be consistent across the application, so that maintenance and future development is easier.

#### Acceptance Criteria

1. WHEN using shadcn/ui components THEN they SHALL follow PancakeSwap's design tokens
2. WHEN styling components THEN the system SHALL use Tailwind CSS classes consistently
3. WHEN creating new components THEN they SHALL be built on shadcn/ui primitives
4. IF custom styling is needed THEN it SHALL be applied through Tailwind CSS utilities or CSS variables

### Requirement 5

**User Story:** As a developer, I want the migration to be incremental, so that the application remains stable throughout the transition.

#### Acceptance Criteria

1. WHEN migrating components THEN the system SHALL allow both old and new components to coexist
2. WHEN deploying changes THEN the application SHALL remain fully functional at each step
3. WHEN testing components THEN both visual and functional tests SHALL pass
4. IF issues arise THEN the system SHALL allow easy rollback to previous component versions

### Requirement 6

**User Story:** As a developer, I want proper TypeScript support, so that I can catch errors at compile time and have good IDE experience.

#### Acceptance Criteria

1. WHEN using shadcn/ui components THEN they SHALL have full TypeScript definitions
2. WHEN importing components THEN TypeScript SHALL provide accurate autocompletion
3. WHEN passing props THEN TypeScript SHALL validate prop types correctly
4. IF custom variants are created THEN they SHALL be properly typed

# Design Document

## Overview

This design outlines the migration strategy from PancakeSwap's current UI system (styled-components + custom uikit) to shadcn/ui. The migration will be incremental, maintaining visual consistency while modernizing the component architecture. The approach focuses on establishing shadcn/ui as the foundation while preserving PancakeSwap's brand identity and existing functionality.

## Architecture

### Current State Analysis
- **UI Framework**: Custom `@pancakeswap/uikit` package with 60+ components
- **Styling**: Styled Components 6.0.7 + Vanilla Extract CSS
- **Design System**: Custom theme with light/dark modes
- **Component Structure**: Monolithic uikit package with components, themes, and utilities

### Target State
- **UI Framework**: shadcn/ui components with Tailwind CSS
- **Styling**: Tailwind CSS utilities + CSS variables for theming
- **Design System**: shadcn/ui design tokens adapted to PancakeSwap branding
- **Component Structure**: Hybrid approach with shadcn/ui primitives and PancakeSwap customizations

### Migration Strategy
1. **Parallel Implementation**: Run both systems simultaneously during transition
2. **Component-by-Component**: Migrate individual components incrementally
3. **Backward Compatibility**: Maintain existing component APIs through adapters
4. **Progressive Enhancement**: Enhance components with shadcn/ui features

## Components and Interfaces

### Phase 1: Foundation Setup
```typescript
// Install shadcn/ui with Tailwind CSS
// Configure design tokens to match PancakeSwap theme
// Set up component directory structure

// apps/web/src/components/ui/ (shadcn/ui components)
// apps/web/src/lib/utils.ts (utility functions)
// tailwind.config.js (Tailwind configuration)
// components.json (shadcn/ui configuration)
```

### Phase 2: Core Component Migration

#### Button Component Migration
```typescript
// Current: @pancakeswap/uikit Button
import { Button } from '@pancakeswap/uikit'

// Target: shadcn/ui Button with PancakeSwap styling
import { Button } from '@/components/ui/button'

// Migration adapter to maintain compatibility
export const PancakeButton = forwardRef<
  HTMLButtonElement,
  ButtonProps & { variant?: 'primary' | 'secondary' | 'tertiary' }
>(({ variant = 'primary', ...props }, ref) => {
  const shadcnVariant = mapPancakeVariantToShadcn(variant)
  return <Button ref={ref} variant={shadcnVariant} {...props} />
})
```

#### Input Component Migration
```typescript
// Current: @pancakeswap/uikit Input
// Target: shadcn/ui Input with form integration
import { Input } from '@/components/ui/input'
import { Label } from '@/components/ui/label'

// Enhanced with form validation
export const PancakeInput = ({ label, error, ...props }) => (
  <div className="space-y-2">
    {label && <Label>{label}</Label>}
    <Input {...props} className={cn(props.className, error && 'border-red-500')} />
    {error && <p className="text-sm text-red-500">{error}</p>}
  </div>
)
```

#### Card Component Migration
```typescript
// Current: @pancakeswap/uikit Card
// Target: shadcn/ui Card with PancakeSwap styling
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'

export const PancakeCard = ({ title, children, ...props }) => (
  <Card className="bg-background border-border" {...props}>
    {title && (
      <CardHeader>
        <CardTitle>{title}</CardTitle>
      </CardHeader>
    )}
    <CardContent>{children}</CardContent>
  </Card>
)
```

### Phase 3: Complex Component Migration

#### Modal System
```typescript
// Current: @pancakeswap/uikit Modal
// Target: shadcn/ui Dialog with enhanced features
import { Dialog, DialogContent, DialogHeader, DialogTitle } from '@/components/ui/dialog'

export const PancakeModal = ({ isOpen, onDismiss, title, children }) => (
  <Dialog open={isOpen} onOpenChange={onDismiss}>
    <DialogContent className="max-w-md">
      <DialogHeader>
        <DialogTitle>{title}</DialogTitle>
      </DialogHeader>
      {children}
    </DialogContent>
  </Dialog>
)
```

#### Form Components
```typescript
// Enhanced form handling with react-hook-form integration
import { useForm } from 'react-hook-form'
import { Form, FormControl, FormField, FormItem, FormLabel, FormMessage } from '@/components/ui/form'

export const PancakeForm = ({ schema, onSubmit, children }) => {
  const form = useForm({ resolver: zodResolver(schema) })
  
  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)}>
        {children}
      </form>
    </Form>
  )
}
```

## Data Models

### Theme Configuration
```typescript
// tailwind.config.js - PancakeSwap theme adaptation
module.exports = {
  theme: {
    extend: {
      colors: {
        // PancakeSwap brand colors
        primary: {
          50: '#fef7ed',
          500: '#f97316', // PancakeSwap orange
          900: '#9a3412',
        },
        secondary: {
          500: '#1d4ed8', // PancakeSwap blue
        },
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        // ... other PancakeSwap colors
      },
      borderRadius: {
        lg: '12px', // PancakeSwap border radius
        md: '8px',
        sm: '4px',
      },
      fontFamily: {
        sans: ['Kanit', 'sans-serif'], // PancakeSwap font
      },
    },
  },
}
```

### Component Mapping
```typescript
// Component migration mapping
export const COMPONENT_MIGRATION_MAP = {
  // Core components
  'Button': '@/components/ui/button',
  'Input': '@/components/ui/input',
  'Card': '@/components/ui/card',
  'Modal': '@/components/ui/dialog',
  
  // Form components
  'Checkbox': '@/components/ui/checkbox',
  'Radio': '@/components/ui/radio-group',
  'Select': '@/components/ui/select',
  'Slider': '@/components/ui/slider',
  
  // Layout components
  'Box': 'div', // Replace with Tailwind utilities
  'Flex': 'div', // Replace with Tailwind flex utilities
  'Grid': 'div', // Replace with Tailwind grid utilities
  
  // Navigation
  'Menu': '@/components/ui/navigation-menu',
  'Dropdown': '@/components/ui/dropdown-menu',
  'Tabs': '@/components/ui/tabs',
} as const
```

### CSS Variable System
```css
/* CSS variables for theme consistency */
:root {
  /* PancakeSwap colors */
  --primary: 24 100% 50%; /* HSL values */
  --primary-foreground: 0 0% 100%;
  --secondary: 217 91% 60%;
  --secondary-foreground: 0 0% 100%;
  
  /* Background colors */
  --background: 0 0% 100%;
  --foreground: 222.2 84% 4.9%;
  
  /* Border and input */
  --border: 214.3 31.8% 91.4%;
  --input: 214.3 31.8% 91.4%;
  --ring: 24 100% 50%;
  
  /* Radius */
  --radius: 0.75rem;
}

[data-theme="dark"] {
  --background: 222.2 84% 4.9%;
  --foreground: 210 40% 98%;
  /* ... dark theme values */
}
```

## Error Handling

### Migration Error Boundaries
```typescript
// Component migration error boundary
export class MigrationErrorBoundary extends Component {
  constructor(props) {
    super(props)
    this.state = { hasError: false, fallbackToOld: false }
  }

  static getDerivedStateFromError(error) {
    // Log migration errors and fallback to old component
    console.error('Component migration error:', error)
    return { hasError: true, fallbackToOld: true }
  }

  render() {
    if (this.state.fallbackToOld) {
      // Render old component as fallback
      return this.props.fallbackComponent
    }

    return this.props.children
  }
}
```

### Component Compatibility Layer
```typescript
// Compatibility wrapper for gradual migration
export const withMigrationSupport = (NewComponent, OldComponent) => {
  return forwardRef((props, ref) => {
    const [useLegacy, setUseLegacy] = useState(false)
    
    useEffect(() => {
      // Feature detection or error handling
      try {
        // Test new component compatibility
        return <NewComponent ref={ref} {...props} />
      } catch (error) {
        console.warn('Falling back to legacy component:', error)
        setUseLegacy(true)
      }
    }, [])
    
    if (useLegacy) {
      return <OldComponent ref={ref} {...props} />
    }
    
    return <NewComponent ref={ref} {...props} />
  })
}
```

## Testing Strategy

### Visual Regression Testing
```typescript
// Storybook stories for component comparison
export default {
  title: 'Migration/Button',
  component: Button,
} as ComponentMeta<typeof Button>

export const OldVsNew: ComponentStory<typeof Button> = () => (
  <div className="flex gap-4">
    <div>
      <h3>Old Button</h3>
      <OldButton variant="primary">Click me</OldButton>
    </div>
    <div>
      <h3>New Button</h3>
      <NewButton variant="default">Click me</NewButton>
    </div>
  </div>
)
```

### Unit Testing Strategy
```typescript
// Test component compatibility
describe('Button Migration', () => {
  it('should maintain same props interface', () => {
    const oldProps = { variant: 'primary', size: 'md', disabled: false }
    const newProps = mapOldPropsToNew(oldProps)
    
    expect(newProps).toEqual({
      variant: 'default',
      size: 'default',
      disabled: false,
    })
  })
  
  it('should render with same visual appearance', async () => {
    const oldButton = render(<OldButton variant="primary">Test</OldButton>)
    const newButton = render(<NewButton variant="default">Test</NewButton>)
    
    // Visual comparison testing
    expect(await compareScreenshots(oldButton, newButton)).toBeLessThan(0.1)
  })
})
```

### Integration Testing
```typescript
// Test full page migration
describe('Swap Page Migration', () => {
  it('should maintain functionality after component migration', async () => {
    render(<SwapPage />)
    
    // Test user interactions
    await userEvent.click(screen.getByRole('button', { name: /connect wallet/i }))
    expect(screen.getByRole('dialog')).toBeInTheDocument()
    
    // Test form functionality
    await userEvent.type(screen.getByLabelText(/amount/i), '100')
    expect(screen.getByDisplayValue('100')).toBeInTheDocument()
  })
})
```

## Implementation Phases

### Phase 1: Foundation (Week 1-2)
- Install and configure shadcn/ui with Tailwind CSS
- Set up PancakeSwap theme configuration
- Create component directory structure
- Implement utility functions and helpers

### Phase 2: Core Components (Week 3-4)
- Migrate Button, Input, Card components
- Implement compatibility wrappers
- Set up visual regression testing
- Update Storybook stories

### Phase 3: Form Components (Week 5-6)
- Migrate form-related components (Checkbox, Radio, Select)
- Integrate with react-hook-form
- Implement form validation patterns
- Test form functionality

### Phase 4: Layout Components (Week 7-8)
- Replace Box, Flex, Grid with Tailwind utilities
- Migrate navigation components (Menu, Dropdown, Tabs)
- Update layout patterns across the application
- Performance optimization

### Phase 5: Complex Components (Week 9-10)
- Migrate Modal, Toast, Tooltip components
- Implement advanced features (animations, accessibility)
- Update component documentation
- Final testing and optimization

### Phase 6: Cleanup (Week 11-12)
- Remove unused styled-components dependencies
- Clean up legacy component imports
- Update build configuration
- Performance auditing and optimization

# Implementation Plan

- [ ] 1. Set up shadcn/ui foundation and configuration
  - Install shadcn/ui, Tailwind CSS, and required dependencies in the web app
  - Configure Tailwind CSS with PancakeSwap theme colors and design tokens
  - Set up shadcn/ui configuration file with custom component paths
  - Create utility functions for className merging and component variants
  - _Requirements: 1.1, 1.3_

- [ ] 2. Create PancakeSwap theme configuration
  - Define CSS variables for PancakeSwap brand colors in both light and dark themes
  - Configure Tailwind theme extension with PancakeSwap-specific design tokens
  - Set up font family configuration for Kanit font
  - Create theme provider component that integrates with existing theme system
  - _Requirements: 1.2, 4.1_

- [ ] 3. Install and configure core shadcn/ui components
  - Use shadcn/ui CLI to install Button, Input, Card, Label, and Dialog components
  - Customize component styles to match PancakeSwap design system
  - Create TypeScript type definitions for component variants
  - Set up component re-export structure in components/ui directory
  - _Requirements: 1.3, 6.1_

- [ ] 4. Implement Button component migration
- [ ] 4.1 Create shadcn/ui Button wrapper with PancakeSwap variants
  - Build PancakeButton component that maps existing variants to shadcn/ui variants
  - Implement size mappings (xs, sm, md, lg) to match current button sizes
  - Add PancakeSwap-specific button styles (primary, secondary, tertiary variants)
  - Create TypeScript interface that maintains backward compatibility
  - _Requirements: 3.1, 3.2, 6.2_

- [ ] 4.2 Implement Button compatibility layer and testing
  - Create migration wrapper that allows both old and new buttons to coexist
  - Write unit tests comparing old vs new button behavior and props
  - Add Storybook stories showing visual comparison between old and new buttons
  - Test button functionality in key pages (swap, farms, pools)
  - _Requirements: 3.3, 5.1, 5.2_

- [ ] 5. Implement Input component migration
- [ ] 5.1 Create shadcn/ui Input wrapper with form integration
  - Build PancakeInput component with label, error state, and validation support
  - Integrate with react-hook-form for enhanced form handling
  - Implement input variants (default, error, disabled states)
  - Add support for input icons and helper text
  - _Requirements: 3.1, 3.2, 6.2_

- [ ] 5.2 Create specialized input components
  - Build BalanceInput component using shadcn/ui Input as base
  - Create SearchInput component with integrated search functionality
  - Implement CurrencyInput with number formatting and validation
  - Add comprehensive TypeScript types for all input variants
  - _Requirements: 3.1, 3.2, 6.2_

- [ ] 6. Implement Card component migration
- [ ] 6.1 Create shadcn/ui Card wrapper with PancakeSwap styling
  - Build PancakeCard component with header, content, and footer sections
  - Implement card variants (default, outlined, elevated)
  - Add support for card actions and interactive states
  - Create responsive card layouts using Tailwind utilities
  - _Requirements: 3.1, 3.2, 4.2_

- [ ] 6.2 Migrate complex card components
  - Update FarmCard component to use new PancakeCard base
  - Migrate PoolCard component with enhanced styling
  - Convert InfoCard components to use shadcn/ui primitives
  - Test card components across different screen sizes
  - _Requirements: 2.3, 3.1, 5.2_

- [ ] 7. Implement form component migrations
- [ ] 7.1 Create form control components
  - Install and configure Checkbox, Radio, Select, and Slider components
  - Build PancakeCheckbox with custom styling and states
  - Create PancakeRadio component with group functionality
  - Implement PancakeSelect with search and multi-select capabilities
  - _Requirements: 3.1, 4.1, 6.2_

- [ ] 7.2 Integrate form components with validation
  - Set up react-hook-form integration with shadcn/ui Form components
  - Create form validation schemas using Zod
  - Build reusable form field components with error handling
  - Test form components in swap and liquidity forms
  - _Requirements: 3.1, 4.3, 6.2_

- [ ] 8. Implement Modal and Dialog migration
- [ ] 8.1 Create shadcn/ui Dialog wrapper for modals
  - Build PancakeModal component using shadcn/ui Dialog primitives
  - Implement modal variants (small, medium, large, fullscreen)
  - Add modal animations and transitions
  - Create modal header, body, and footer components
  - _Requirements: 2.2, 3.1, 3.2_

- [ ] 8.2 Migrate critical modal components
  - Update WalletModal to use new PancakeModal base
  - Migrate TransactionModal with enhanced UX
  - Convert SettingsModal to use shadcn/ui components
  - Test modal functionality and accessibility features
  - _Requirements: 2.2, 5.1, 5.2_

- [ ] 9. Replace layout components with Tailwind utilities
- [ ] 9.1 Create layout utility components
  - Build Flex component wrapper using Tailwind flex utilities
  - Create Grid component with responsive grid layouts
  - Implement Container component with max-width constraints
  - Add spacing and alignment utility components
  - _Requirements: 4.2, 4.3_

- [ ] 9.2 Update page layouts to use new utilities
  - Migrate swap page layout to use Tailwind utilities
  - Update farms page layout with new grid system
  - Convert pools page to use new layout components
  - Test responsive behavior across all screen sizes
  - _Requirements: 2.3, 4.2, 5.2_

- [ ] 10. Implement navigation component migrations
- [ ] 10.1 Create navigation components
  - Install and configure NavigationMenu, DropdownMenu, and Tabs components
  - Build PancakeMenu component for main navigation
  - Create PancakeDropdown with custom styling
  - Implement PancakeTabs with enhanced functionality
  - _Requirements: 3.1, 4.1, 6.2_

- [ ] 10.2 Update navigation across the application
  - Migrate main menu to use new PancakeMenu component
  - Update user dropdown menu with enhanced features
  - Convert tab navigation in farms and pools pages
  - Test navigation accessibility and keyboard support
  - _Requirements: 2.2, 5.1, 5.2_

- [ ] 11. Implement advanced components migration
- [ ] 11.1 Create Toast and notification components
  - Install and configure Toast component with custom styling
  - Build notification system using shadcn/ui primitives
  - Implement toast variants (success, error, warning, info)
  - Add toast positioning and animation controls
  - _Requirements: 2.2, 3.1, 4.1_

- [ ] 11.2 Create Tooltip and Popover components
  - Install and configure Tooltip and Popover components
  - Build PancakeTooltip with custom styling and positioning
  - Create PancakePopover for complex interactive content
  - Test tooltip and popover functionality across the application
  - _Requirements: 2.2, 3.1, 6.2_

- [ ] 12. Implement compatibility and migration utilities
- [ ] 12.1 Create component migration helpers
  - Build withMigrationSupport HOC for gradual component migration
  - Create prop mapping utilities for component compatibility
  - Implement error boundaries for migration fallbacks
  - Add development-time warnings for deprecated component usage
  - _Requirements: 5.1, 5.3_

- [ ] 12.2 Set up migration testing infrastructure
  - Create visual regression testing setup with Storybook
  - Build component comparison utilities for testing
  - Implement automated screenshot testing for component migration
  - Add performance benchmarking for new vs old components
  - _Requirements: 5.2, 5.3_

- [ ] 13. Update build configuration and dependencies
- [ ] 13.1 Optimize build configuration for new dependencies
  - Update webpack configuration to handle Tailwind CSS properly
  - Configure PostCSS for optimal CSS processing
  - Set up CSS purging for production builds
  - Update TypeScript configuration for new component types
  - _Requirements: 1.1, 1.3_

- [ ] 13.2 Clean up legacy dependencies and imports
  - Remove unused styled-components dependencies where possible
  - Update import statements to use new component paths
  - Clean up legacy CSS and theme files
  - Update package.json dependencies and scripts
  - _Requirements: 5.1, 5.3_

- [ ] 14. Performance optimization and final testing
- [ ] 14.1 Optimize component performance
  - Implement lazy loading for large component libraries
  - Optimize bundle size by tree-shaking unused components
  - Add performance monitoring for component render times
  - Test application performance with new component system
  - _Requirements: 2.3, 5.2_

- [ ] 14.2 Comprehensive testing and documentation
  - Run full test suite to ensure no regressions
  - Update component documentation with new usage examples
  - Create migration guide for developers
  - Perform accessibility audit on migrated components
  - _Requirements: 2.2, 5.1, 5.2, 6.1_
