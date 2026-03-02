# Project Structure

## Current Organization

This workspace is newly initialized. Update this document as the project structure develops.

## Typical Structure Patterns

Organize your project based on the chosen tech stack:

### Frontend Projects
```
src/
  components/     # Reusable UI components
  pages/         # Page-level components
  hooks/         # Custom hooks
  utils/         # Helper functions
  styles/        # Global styles
  assets/        # Images, fonts, etc.
tests/           # Test files
public/          # Static assets
```

### Backend Projects
```
src/
  controllers/   # Request handlers
  models/        # Data models
  services/      # Business logic
  routes/        # API routes
  middleware/    # Custom middleware
  utils/         # Helper functions
  config/        # Configuration files
tests/           # Test files
```

### Full-stack Projects
```
client/          # Frontend code
server/          # Backend code
shared/          # Shared types/utilities
docs/            # Documentation
```

## Naming Conventions

Update with project-specific conventions:
- File naming: camelCase, kebab-case, PascalCase
- Directory naming
- Component/class naming
- Variable naming

## File Organization Rules

- Keep related files together
- Separate concerns appropriately
- Follow framework conventions
- Maintain consistent structure across similar modules
