# Requirements Document

## Introduction

This feature involves simplifying the PancakeSwap network selection by removing multiselect network functionality and focusing exclusively on BSC (Binance Smart Chain). The goal is to streamline the user experience by eliminating complex network switching options and defaulting to BSC as the primary and only supported network for core functionality.

## Requirements

### Requirement 1

**User Story:** As a PancakeSwap user, I want the application to default to BSC network only, so that I don't get confused by multiple network options and can focus on BSC-based trading.

#### Acceptance Criteria

1. WHEN a user visits the PancakeSwap application THEN the system SHALL default to BSC network without showing other network options
2. WHEN a user attempts to access network selection components THEN the system SHALL only display BSC as an available option
3. WHEN a user's wallet is connected to a non-BSC network THEN the system SHALL prompt them to switch to BSC network
4. IF a user is on the wrong network THEN the system SHALL show a clear message to switch to BSC

### Requirement 2

**User Story:** As a developer, I want to remove multiselect network components from the gamification app, so that the codebase is simplified and focused on BSC functionality.

#### Acceptance Criteria

1. WHEN the NetworkMultiSelector component is referenced THEN the system SHALL remove or replace it with BSC-only logic
2. WHEN network selection arrays are used THEN the system SHALL default to BSC ChainId only
3. WHEN multiselect network state is managed THEN the system SHALL replace it with single BSC network state
4. IF any component depends on multiple network selection THEN the system SHALL refactor it to work with BSC only

### Requirement 3

**User Story:** As a user of the main web application, I want network switching to be simplified to BSC-focused options, so that I can easily stay on the primary supported network.

#### Acceptance Criteria

1. WHEN the NetworkSwitcherModal is opened THEN the system SHALL prioritize BSC and minimize other network options
2. WHEN the NetworkSelector component is used THEN the system SHALL default to BSC and limit other chain selections
3. WHEN network filtering logic is applied THEN the system SHALL ensure BSC is always available and preferred
4. IF a user needs to switch networks THEN the system SHALL guide them primarily toward BSC

### Requirement 4

**User Story:** As a system administrator, I want to ensure that BSC-only configuration doesn't break existing functionality, so that users can continue using all PancakeSwap features seamlessly.

#### Acceptance Criteria

1. WHEN network-dependent features are accessed THEN the system SHALL continue to function properly with BSC as the primary network
2. WHEN chain-specific configurations are loaded THEN the system SHALL properly handle BSC-specific settings
3. WHEN users interact with liquidity, farming, or trading features THEN the system SHALL work correctly with BSC network
4. IF any feature requires multi-chain support THEN the system SHALL maintain minimal necessary chain support while prioritizing BSC

# Design Document

## Overview

This design outlines the approach to simplify PancakeSwap's network selection by removing multiselect network functionality and focusing exclusively on BSC (Binance Smart Chain). The solution involves modifying existing network selection components, updating configuration logic, and ensuring BSC remains the primary supported network while maintaining backward compatibility for essential features.

## Architecture

### Component Hierarchy
```
Network Selection System
├── NetworkSwitcherModal (Modified)
│   ├── BSC-focused network list
│   └── Simplified chain switching
├── NetworkSelector (Updated)
│   ├── BSC-default selection
│   └── Limited chain options
└── NetworkMultiSelector (Removed/Replaced)
    └── Single BSC selection logic
```

### Data Flow
1. **Application Initialization**: Default to BSC ChainId (56)
2. **Network Detection**: Check user's wallet network
3. **Network Validation**: Ensure BSC compatibility
4. **Network Switching**: Guide users to BSC when needed

## Components and Interfaces

### Modified NetworkSwitcherModal
- **Purpose**: Simplified network switching with BSC priority
- **Changes**: 
  - Reduce chain options in `getSortedChains()` function
  - Prioritize BSC in chain ordering
  - Simplify non-EVM chain handling
- **Interface**: Maintains existing props but with BSC-focused logic

### Updated NetworkSelector
- **Purpose**: BSC-default network selection for liquidity features
- **Changes**:
  - Default `chainId` to BSC (ChainId.BSC)
  - Filter chain list to prioritize BSC
  - Maintain compatibility with version-specific filtering
- **Interface**: Same props, BSC-focused implementation

### Replaced NetworkMultiSelector
- **Purpose**: Convert multiselect to single BSC selection
- **Changes**:
  - Replace `pickMultiSelect` array with single BSC value
  - Simplify `setPickMultiSelect` to BSC-only setter
  - Remove MultiSelect component usage
- **Interface**: Simplified to single network selection

## Data Models

### Network Configuration
```typescript
// Before: Multiple chain support
interface NetworkConfig {
  supportedChains: ChainId[]
  defaultChain: ChainId
  allowMultiSelect: boolean
}

// After: BSC-focused configuration
interface BSCNetworkConfig {
  primaryChain: ChainId.BSC
  fallbackChains?: ChainId[] // Minimal fallback options
  enforceBSC: boolean
}
```

### Chain Selection State
```typescript
// Before: Multi-chain selection
interface MultiChainState {
  selectedChains: ChainId[]
  activeChain: ChainId
}

// After: BSC-focused state
interface BSCChainState {
  activeChain: ChainId.BSC
  isCorrectNetwork: boolean
}
```

## Error Handling

### Network Mismatch Scenarios
1. **Wrong Network Detection**
   - Display clear BSC switching prompt
   - Provide one-click BSC network switch
   - Show network mismatch warning

2. **Unsupported Network**
   - Guide users to BSC network
   - Disable functionality until BSC connection
   - Provide helpful switching instructions

3. **Connection Issues**
   - Fallback to BSC default
   - Show connection status
   - Retry mechanism for network switching

### Error Recovery
- **Graceful Degradation**: Maintain core functionality on BSC
- **User Guidance**: Clear instructions for BSC network setup
- **Fallback Handling**: Default to BSC when network detection fails

## Testing Strategy

### Unit Testing
- **Component Tests**: Verify BSC-only logic in network selectors
- **State Management**: Test BSC-focused state updates
- **Configuration Tests**: Validate BSC default configurations

### Integration Testing
- **Network Switching**: Test BSC switching functionality
- **Component Integration**: Verify modified components work together
- **User Flow Testing**: Test complete BSC-focused user journeys

### User Acceptance Testing
- **Network Selection**: Verify simplified network selection UX
- **BSC Functionality**: Ensure all features work properly on BSC
- **Error Scenarios**: Test wrong network handling and recovery

## Implementation Considerations

### Backward Compatibility
- Maintain existing component interfaces where possible
- Preserve essential multi-chain functionality for critical features
- Ensure gradual migration path for dependent components

### Performance Impact
- Reduced complexity in network selection logic
- Faster component rendering with fewer network options
- Simplified state management with single network focus

### Configuration Management
- Update default chain configurations to BSC
- Modify supported chain lists to prioritize BSC
- Ensure environment-specific BSC settings are properly configured

# Implementation Plan

- [ ] 1. Update NetworkMultiSelector component to BSC-only selection
  - Replace MultiSelect component with single BSC selection display
  - Modify pickMultiSelect array to default to BSC ChainId only
  - Update setPickMultiSelect function to handle single BSC value
  - Create unit tests for BSC-only selection logic
  - _Requirements: 2.1, 2.2, 2.3_

- [ ] 2. Modify NetworkSwitcherModal for BSC-focused network switching
  - Update getSortedChains function to prioritize BSC at the top
  - Reduce available network options while keeping BSC prominent
  - Modify chain ordering logic to ensure BSC is always first
  - Test network switching modal with BSC priority
  - _Requirements: 3.1, 3.3, 1.2_

- [ ] 3. Update NetworkSelector component with BSC defaults
  - Set default chainId to ChainId.BSC instead of INFINITY_SUPPORTED_CHAINS[0]
  - Modify chainList filtering to ensure BSC is always available
  - Update component to prioritize BSC in chain selection options
  - Add unit tests for BSC-default behavior
  - _Requirements: 3.2, 3.3, 1.1_

- [ ] 4. Replace NetworkMultiSelector usage in gamification app
  - Update DashboardCampaigns component to use single BSC selection
  - Modify defaultValueChains to return only BSC ChainId
  - Replace multiselect state management with single BSC state
  - Update related components that depend on multiselect functionality
  - _Requirements: 2.1, 2.2, 2.3_

- [ ] 5. Update network configuration constants and defaults
  - Modify chain filtering logic to prioritize BSC across components
  - Update default network configurations to use BSC as primary
  - Ensure BSC ChainId is properly set in network-dependent features
  - Create configuration tests to verify BSC defaults
  - _Requirements: 4.1, 4.2, 1.1_

- [ ] 6. Implement BSC-focused wrong network handling
  - Update WrongNetworkSelect component to guide users specifically to BSC
  - Modify network mismatch detection to prioritize BSC switching
  - Enhance user messaging to clearly indicate BSC as the preferred network
  - Test wrong network scenarios with BSC-focused guidance
  - _Requirements: 1.3, 1.4, 3.4_

- [ ] 7. Update network selection in liquidity and trading features
  - Modify AddLiquiditySelector to default to BSC network selection
  - Update NetworkSelector usage in liquidity-related components
  - Ensure BSC compatibility in farming and pool features
  - Test liquidity features with BSC-only network focus
  - _Requirements: 4.3, 3.2, 1.1_

- [ ] 8. Create comprehensive tests for BSC-only functionality
  - Write integration tests for BSC-focused network selection flow
  - Test component interactions with BSC-only configuration
  - Verify error handling and network switching to BSC
  - Create end-to-end tests for BSC-focused user journeys
  - _Requirements: 4.1, 4.2, 4.3, 4.4_
