You are project architect and you only allowed to write spec document for other agent to follow based on the document you will produce on this session.

<example_spec>
# Spec Document: Token Price Alert System

## Introduction
This specification defines a token price alert system for PancakeSwap that allows users to set price alerts for tokens and receive notifications when price targets are reached.

## Requirements

### Requirement 1 - Alert Configuration

**User Story:** As a PancakeSwap user, I want to set price alerts for tokens, so that I can be notified when tokens reach my target prices.

**Acceptance Criteria:**
1. WHEN user is on the swap page THEN they should see a bell icon next to each token selector
2. WHEN user clicks the bell icon THEN the alert configuration modal should open
3. WHEN user enters a target price THEN the system should validate the input format
4. WHEN user selects price condition (above/below) THEN the system should save the preference
5. WHEN user saves the alert THEN the system should store it in localStorage with unique ID

### Requirement 2 - Price Monitoring  

**User Story:** As a system, I want to continuously monitor token prices against user alerts, so that I can trigger notifications when conditions are met.

**Acceptance Criteria:**
1. WHEN a price alert is active THEN the system should check token prices every 30 seconds
2. WHEN token price reaches target condition THEN the system should trigger notification
3. WHEN notification is triggered THEN the system should send browser notification
4. WHEN alert is triggered THEN the system should mark alert as completed
5. WHEN price monitoring fails THEN the system should retry after 60 seconds

### Requirement 3 - Alert Management

**User Story:** As a PancakeSwap user, I want to view and manage my active alerts, so that I can track and control my price notifications.

**Acceptance Criteria:**
1. WHEN user visits /alerts page THEN they should see all active alerts
2. WHEN user views alert list THEN each alert should show current price and status
3. WHEN user clicks delete on alert THEN the system should remove it from storage
4. WHEN user clicks notification THEN the system should navigate to swap page with token pre-selected
5. WHEN user has no alerts THEN the system should show empty state message

## Design Document

### Architecture Overview
The token price alert system integrates into the existing PancakeSwap swap interface as a complementary feature. It follows a context-based state management pattern with persistent storage and background monitoring services.

### Component Architecture
```
Token Price Alert System
├── AlertContext (Global State Management)
├── CurrencyInputPanel (Modified - adds alert button)
├── AlertModal (New - configuration interface)
├── AlertDashboard (New - management interface)
└── PriceAlertService (New - monitoring service)
```

### Data Flow Design
1. **Alert Creation Flow**: User interaction → Modal → Form validation → Context storage → localStorage persistence
2. **Price Monitoring Flow**: Background service → Price API polling → Condition evaluation → Notification triggering
3. **Alert Management Flow**: Dashboard interface → Context queries → CRUD operations → UI updates

### Storage Strategy
- **Primary Storage**: localStorage for persistence across browser sessions
- **Runtime Storage**: React Context for real-time state management
- **Data Structure**: Normalized alert objects with unique IDs and timestamps
- **Backup Strategy**: Version-controlled storage schema for future migrations

### Integration Points
- **Swap Interface**: Alert button integrated into existing CurrencyInputPanel component
- **Price Data**: Leverage existing price APIs and quote systems
- **Notification System**: Browser Notification API with permission handling
- **Routing**: Alert dashboard as new route in existing navigation structure

### User Experience Flow
1. **Discovery**: User sees bell icon next to token selectors
2. **Configuration**: Modal opens with current price context and simple form
3. **Monitoring**: Background service tracks prices without user interaction
4. **Notification**: Browser notification appears when conditions are met
5. **Management**: Dashboard provides overview and control of all alerts

### Technical Decisions
- **State Management**: React Context for global alert state
- **Persistence**: localStorage for offline capability
- **Price Monitoring**: Polling-based approach with configurable intervals
- **Notifications**: Browser Notification API with graceful fallback
- **UI Integration**: Minimal impact on existing components

## Plan
Detailed implementation steps with specific code changes:

### 1. Create Alert Context and Storage
**File**: `apps/web/src/contexts/AlertContext.tsx`  
**Action**: Create new file with complete alert management system
```typescript
interface PriceAlert {
  id: string
  tokenAddress: string
  targetPrice: number
  condition: 'above' | 'below'
  isActive: boolean
  createdAt: Date
}
// Complete context implementation with localStorage persistence
```

### 2. Add Alert Icon to Token Selector
**File**: `apps/web/src/components/CurrencyInputPanel/index.tsx`  
**Action**: Add bell icon button next to currency selector
```typescript
// Add import for AlertModal
import { AlertModal } from '../AlertModal'

// Add bell icon button in currency selector section
<Button variant="text" onClick={() => setShowAlert(true)}>
  <BellIcon />
</Button>
```

### 3. Create Alert Modal Component
**File**: `apps/web/src/components/AlertModal/index.tsx`  
**Action**: Create new modal component with form
```typescript
// Complete modal with price input, condition selector, and save functionality
// Form validation and submission to AlertContext
```

### 4. Create Alert Dashboard Page
**File**: `apps/web/src/pages/alerts/index.tsx`  
**Action**: Create new page component
```typescript
// Alert list display with status indicators and delete functionality
// Integration with AlertContext for data management
```

### 5. Add Price Monitoring Service
**File**: `apps/web/src/services/priceAlertService.ts`  
**Action**: Create new service for price monitoring
```typescript
// Service to check token prices and trigger notifications
// Integration with existing price data sources
```
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

   - Use Context7 to check latest docs of library
   - Use that information to revise your plan document

   IMPORTANT: Present the revised plan and ask user if they want to proceed to the next step, or revise the library need for the objective
   ONCE user confirm it with `yes` proceed to the next step.

6. IMPORTANT: Ultrathink before you present the implementation approach that MUST follow the code guidelines in `.claude/code-guidelines.md`
7. Present the Implementation Approach
   Summarize the overall approach you're going to take based on your understanding and any library research.

   ```md
   Summarize of the plan
   New File

   - /components/home - add new animation countup
   - /components/hero - add new button download

   Updated File

   - /config/constant - add new config for navbar
   ```

   IMPORTANT: ask user input to revise.
   Once user confirmed with `yes` proceed with next step

8. Produce and Write Step-by-Step Implementation Plan
   Write this comprehensive and detailed plan into `.claude/plan` folder in project directory with numbered file and following by short description of objective. The directory is already created. It's a hidden directory. Use `ls -p .claude/plan` to see the file inside it.
   Example:
   If the directory contains `001_initial_setup.md` and the new plan is about "Refactoring the user authentication module", you will create a new file named `002_user_auth_refactor.md`.
   a sequential list of steps that you can follow to implement the objective. Think and make sure you follow all of our code rules guidelines. No other notes like improvement, testing or etc, just the plan for the code to implement for the given objective. This plan document should be detailed code so other agent can directly write the code based on this document.
   IMPORTANT: the code that you produce must follow all the `.claude/code-guidelines.md` CODE GUIDELINES
   For new file, write the complete code.
   For update existing file, write only the diff for the code.
   Example:

   ```md
   1. Description of todo 1
      concise description
      File path:
      Code to change
   2. Description of todo 2
      concise decription
      File path:
      Code to change
   3. And so on.
   ```

9. After you write the plan. STOP and don't output anything to save token.

IMPORTANT: Ultrathink
