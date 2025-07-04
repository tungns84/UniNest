# Style Guide - UniNest

## Code Style Standards

### TypeScript/JavaScript
- **Formatting:** Prettier with 2-space indentation  
- **Linting:** ESLint with TypeScript extensions
- **Naming:** camelCase for variables/functions, PascalCase for classes/interfaces
- **File Naming:** kebab-case for files, PascalCase for React components

### Backend (NestJS)
- **Module Structure:** Feature-based modules with clear separation
- **Database:** TypeORM entities with decorators, repository pattern
- **API Design:** RESTful endpoints with OpenAPI documentation
- **Error Handling:** Global exception filters with proper HTTP status codes

### Frontend (React)
- **Components:** Functional components with hooks
- **State Management:** Redux Toolkit with RTK Query for API calls
- **Styling:** TailwindCSS with component-level organization
- **File Structure:** Feature-based folders with index exports

### Mobile (Kotlin)
- **Architecture:** MVVM with Jetpack Compose
- **Naming:** Kotlin conventions with camelCase
- **UI:** Material Design 3 components
- **State:** Compose state management with local ViewModels

## Documentation Standards
- **API:** OpenAPI/Swagger documentation for all endpoints
- **Components:** JSDoc comments for complex functions
- **Database:** ER diagrams and migration documentation
- **Architecture:** Mermaid diagrams for system flows

## Git Workflow
- **Branching:** Feature branches from main with descriptive names
- **Commits:** Conventional commits with clear, descriptive messages
- **PRs:** Template-based with testing checklist and documentation updates

## Testing Standards
- **Backend:** Jest unit tests with >80% coverage
- **Frontend:** React Testing Library with user interaction focus
- **E2E:** Playwright for critical user journeys
- **API:** Postman/Insomnia collections for manual testing

## Performance Guidelines
- **Database:** Proper indexing, query optimization, connection pooling
- **Frontend:** Code splitting, lazy loading, image optimization
- **API:** Response caching, rate limiting, pagination
- **Mobile:** Image compression, network optimization, offline support

*Established during VAN initialization for consistent development standards*
