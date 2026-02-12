# Codex 8-Point Review Checklist

> Generic checklist template. During project kickoff, this is customized with stack-specific sub-items and saved to `.agents/codex/review-checklist.md`.

---

## 1. Design Compliance
- [ ] Colors match design tokens (no hardcoded hex values outside constants)
- [ ] Typography uses correct font family, size, weight, line-height per text style
- [ ] Spacing uses spacing constants (no magic numbers)
- [ ] Border radius uses named tokens
- [ ] Shadows match shadow constants
- [ ] Icons are consistent in style, size, and stroke weight
<!-- Stack-specific: Add framework component styling checks here -->

## 2. Type Safety
- [ ] No `any` types — all props, state, and returns properly typed
- [ ] Shared types in central type files, not duplicated across modules
- [ ] Enums and unions match spec definitions
- [ ] Generics used appropriately (not over-engineered)
- [ ] API response types match actual backend responses
<!-- Stack-specific: Add language/framework type system checks here -->

## 3. Framework Practices
- [ ] Idiomatic patterns for the project's framework
- [ ] Component/module structure follows framework conventions
- [ ] Routing follows framework patterns
- [ ] No anti-patterns (check framework docs for common pitfalls)
<!-- Stack-specific: e.g., React hooks rules, Next.js data fetching, Django ORM patterns -->

## 4. State Management
- [ ] Stores/state containers are focused (not monolithic)
- [ ] Async actions handle loading and error states
- [ ] State persistence configured where needed
- [ ] No prop drilling when store/context access is cleaner
- [ ] Derived state computed efficiently (not redundantly stored)
<!-- Stack-specific: e.g., Zustand middleware, Redux selectors, Vuex modules -->

## 5. Security
- [ ] No secrets or API keys in source code (use environment variables)
- [ ] Authentication and authorization properly implemented
- [ ] User input validated and sanitized
- [ ] No SQL injection vectors
- [ ] No XSS vulnerabilities (output properly escaped)
- [ ] CSRF protection where applicable
- [ ] Dependencies free of known vulnerabilities
<!-- Stack-specific: e.g., RLS policies, CORS config, CSP headers -->

## 6. Performance
- [ ] Lists use virtualized/windowed rendering for large datasets
- [ ] Images optimized (proper formats, lazy loading, responsive sizes)
- [ ] No unnecessary re-renders or recomputations
- [ ] Heavy operations offloaded from main/UI thread
- [ ] Lazy loading for non-critical code paths
- [ ] Bundle size reasonable (no unnecessary dependencies)
<!-- Stack-specific: e.g., React.memo, useMemo, code splitting, CDN config -->

## 7. Accessibility
- [ ] All interactive elements have accessible labels
- [ ] Semantic roles set correctly (button, link, heading, etc.)
- [ ] Touch/click targets meet minimum size (44x44pt mobile, 24x24px desktop)
- [ ] Color contrast meets WCAG AA (4.5:1 for normal text, 3:1 for large text)
- [ ] Keyboard navigation works (focus management, tab order)
- [ ] Screen reader navigation is logical
- [ ] Dynamic content changes are announced
<!-- Stack-specific: e.g., ARIA attributes, React Native accessibility props -->

## 8. Code Structure
- [ ] Files under 300 lines — split if larger
- [ ] One component/class per file (for UI code)
- [ ] Consistent naming: PascalCase for components/classes, camelCase for functions/variables, UPPER_SNAKE for constants
- [ ] No dead code, unused imports, or commented-out blocks
- [ ] Error boundaries / error handling at appropriate levels
- [ ] Clear module boundaries and separation of concerns
<!-- Stack-specific: e.g., barrel exports, module organization, test co-location -->

---

## Review Verdict Scale

| Verdict | Meaning | Action |
|---------|---------|--------|
| **APPROVE** | Ship it. No issues or only nitpicks. | Merge / move forward |
| **APPROVE WITH COMMENTS** | Ship it, address comments in follow-up. | Merge, create follow-up tasks |
| **REQUEST CHANGES** | Must fix before merging. Blocking issues. | Return to Coding Agent |
| **REJECT** | Fundamental approach wrong. | Return with alternative direction |

---

## Customization

During `/agent-team:kickoff`, this checklist is customized for the project's stack. Common customizations:

| Stack | Section 3 | Section 6 | Other |
|-------|-----------|-----------|-------|
| React / Next.js | Hooks rules, RSC vs client | React.memo, Suspense, code splitting | — |
| React Native / Expo | StyleSheet.create, SafeAreaView | FlatList, expo-image, Reanimated | Platform-specific checks |
| Vue | Composition API, SFC conventions | Computed caching, lazy routes | — |
| Python / Django | Django ORM patterns, view conventions | QuerySet optimization, N+1 | — |
| Node.js / Express | Middleware patterns, error handling | Async patterns, connection pooling | — |
| Go | Interface patterns, error handling | Goroutine management, profiling | — |
