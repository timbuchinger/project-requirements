# Frontend

This document contains the tools that frontend projects should use.

## Vue

- Vue.js 3
- TypeScript
- Vue Router for navigation
- Pinia for state management

## Build Tools

- Vite
- TypeScript configuration
- PostCSS for CSS processing

## Styling

- TailwindCSS v3 for utility-first styling
- Custom theme configuration
  - Custom colors, including primary and secondary, defined in Tailwind config
- Dark/Light mode support

## Notifications

- Centralized notification system

## Structure

- Components should be split at logical breakpoints; use ~250 lines as a guideline rather than a strict limit
- Use PascalCase for component names
- Prefix components with 'App' for global components

## Testing

- Vitest for unit and component testing
- Vue Test Utils for component testing
- Test coverage requirements: 80% minimum
- Snapshot testing for UI components
- Mock external dependencies and Pinia stores

## Error Handling

- Global error boundary setup
- Consistent error logging
- Custom error components for different scenarios

## State Management

- Use Pinia stores for global state
- Keep store actions atomic
- Modularize stores by feature
- Use composables for shared logic

## Component Guidelines

- Use `<script setup>` syntax
- Prefer composition API
- Props should be typed with PropType
- Emit typings with defineEmits
- Use defineExpose when needed
