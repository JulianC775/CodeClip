# CodeClip - Development Guide for Claude

## Project Overview

CodeClip is an advanced code snippet manager web application built with React, TypeScript, Vite, and Tailwind CSS. This project showcases intermediate-to-advanced React patterns and serves as a portfolio piece demonstrating modern frontend development skills.

## Architecture & Design Patterns

### State Management Philosophy
- **useReducer** for complex snippet state management (not useState)
- Centralized action types for predictable state updates
- Immutable state updates using spread operators
- Integration with localStorage for persistence

### Custom Hooks Strategy
All reusable logic is abstracted into custom hooks:
1. **useLocalStorage** - Bidirectional sync between React state and localStorage
2. **useTheme** - Dark mode management with system preference detection
3. **useCopyToClipboard** - Clipboard operations with timeout-based feedback

### Component Architecture
Components follow single responsibility principle:
- **Presentational Components**: SnippetCard, SearchBar (receive props, render UI)
- **Container Components**: App, SnippetList (manage state, handle logic)
- **Form Components**: SnippetForm (controlled inputs, validation)
- **Layout Components**: Header (navigation, theme toggle)

## Tech Stack Details

### Core Dependencies
```json
{
  "react": "^18.3.1",
  "react-dom": "^18.3.1",
  "react-syntax-highlighter": "^15.5.0"
}
```

### Development Dependencies
- TypeScript 5.7+ for type safety
- Vite 6.0+ for fast development
- Tailwind CSS 3.4+ for styling
- ESLint for code quality

### Build Tool: Vite
- Fast HMR (Hot Module Replacement)
- Optimized production builds
- Native ES modules support
- TypeScript support out of the box

## Data Model

### Snippet Interface
```typescript
interface Snippet {
  id: string;              // UUID v4
  title: string;           // Snippet name
  language: string;        // Programming language
  code: string;            // Code content
  tags: string[];          // Array of tag strings
  isFavorite: boolean;     // Star status
  createdAt: number;       // Unix timestamp
  updatedAt: number;       // Unix timestamp
}
```

### State Shape
```typescript
interface SnippetState {
  snippets: Snippet[];
  searchQuery: string;
  selectedLanguage: string;
  editingSnippet: Snippet | null;
}
```

### Reducer Actions
```typescript
type SnippetAction =
  | { type: 'ADD_SNIPPET'; payload: Snippet }
  | { type: 'UPDATE_SNIPPET'; payload: Snippet }
  | { type: 'DELETE_SNIPPET'; payload: string }
  | { type: 'TOGGLE_FAVORITE'; payload: string }
  | { type: 'SET_SNIPPETS'; payload: Snippet[] }
  | { type: 'SET_SEARCH_QUERY'; payload: string }
  | { type: 'SET_LANGUAGE_FILTER'; payload: string }
  | { type: 'SET_EDITING_SNIPPET'; payload: Snippet | null }
  | { type: 'IMPORT_SNIPPETS'; payload: Snippet[] };
```

## Component Specifications

### App.tsx
**Responsibility**: Main application container, state management
**Hooks Used**:
- `useReducer` for snippet state
- `useLocalStorage` for persistence
- `useTheme` for dark mode
- `useEffect` for initialization

**Key Logic**:
- Initialize snippets from localStorage on mount
- Sync state changes to localStorage
- Compute filtered snippets based on search/language
- Handle all CRUD operations through reducer dispatch

### Header.tsx
**Props**:
```typescript
interface HeaderProps {
  onExport: () => void;
  onImport: (file: File) => void;
  snippetCount: number;
}
```

**Features**:
- App branding and title
- Theme toggle button
- Export/import buttons
- Snippet count display

### SearchBar.tsx
**Props**:
```typescript
interface SearchBarProps {
  searchQuery: string;
  onSearchChange: (query: string) => void;
  selectedLanguage: string;
  onLanguageChange: (language: string) => void;
  languages: string[];
}
```

**Features**:
- Search input with debouncing (optional optimization)
- Language filter dropdown
- Clear filters button
- Responsive layout

### SnippetForm.tsx
**Props**:
```typescript
interface SnippetFormProps {
  snippet?: Snippet;
  onSubmit: (snippet: Omit<Snippet, 'id' | 'createdAt' | 'updatedAt'>) => void;
  onCancel: () => void;
}
```

**Features**:
- Controlled form inputs (title, language, code, tags)
- Validation (required fields)
- Tag input with add/remove
- Edit mode vs create mode
- Code textarea with monospace font
- Form submission with Enter (Cmd/Ctrl + Enter)

### SnippetList.tsx
**Props**:
```typescript
interface SnippetListProps {
  snippets: Snippet[];
  onEdit: (snippet: Snippet) => void;
  onDelete: (id: string) => void;
  onToggleFavorite: (id: string) => void;
  onCopy: (code: string) => void;
}
```

**Features**:
- Render list of SnippetCard components
- Empty state message
- Filter favorites section (optional)
- Grid/list layout

### SnippetCard.tsx
**Props**:
```typescript
interface SnippetCardProps {
  snippet: Snippet;
  onEdit: () => void;
  onDelete: () => void;
  onToggleFavorite: () => void;
  onCopy: () => void;
}
```

**Features**:
- Syntax highlighting with react-syntax-highlighter
- Copy button with feedback
- Edit/delete buttons
- Favorite star toggle
- Tag badges
- Timestamp display (relative time)
- Expandable code view
- Language badge

## Custom Hooks Implementation

### useLocalStorage
```typescript
function useLocalStorage<T>(key: string, initialValue: T): [T, (value: T) => void] {
  // Initialize state from localStorage or use initialValue
  // Sync state to localStorage on changes
  // Handle JSON parsing errors
  // Handle storage events for cross-tab sync (optional)
}
```

**Key Points**:
- Generic type for type safety
- Try-catch for JSON parse errors
- useEffect to write to localStorage
- Optional: window storage event listener for sync

### useTheme
```typescript
function useTheme() {
  // Check localStorage for saved theme
  // Check system preference (prefers-color-scheme)
  // Apply theme class to document element
  // Persist theme changes to localStorage
  return { theme, toggleTheme, setTheme };
}
```

**Key Points**:
- Add/remove 'dark' class on <html> element
- Use matchMedia for system preference
- Persist to localStorage as 'theme'
- Initialize on mount with useEffect

### useCopyToClipboard
```typescript
function useCopyToClipboard(timeout = 2000) {
  // Track copied state
  // Copy text to clipboard using navigator.clipboard
  // Show success feedback
  // Reset after timeout
  return { copied, copyToClipboard };
}
```

**Key Points**:
- Use navigator.clipboard.writeText()
- Handle async clipboard API
- Timeout to reset copied state
- Error handling for clipboard permissions

## Styling Guidelines

### Tailwind Configuration
- **Dark Mode**: Class-based (`darkMode: 'class'`)
- **Custom Colors**: Primary palette (blue)
- **Custom Animations**: fade-in, slide-up
- **Responsive Breakpoints**: sm, md, lg, xl

### Component Styling Patterns
```css
/* Buttons */
.btn-primary - Primary action buttons
.btn-secondary - Secondary action buttons

/* Inputs */
.input-field - Text inputs, textareas, selects

/* Cards */
.card - Container cards with shadow and border
```

### Dark Mode Classes
All components should have dark mode variants:
```tsx
<div className="bg-white dark:bg-gray-800 text-gray-900 dark:text-gray-100">
```

### Responsive Design
- Mobile-first approach
- Stack vertically on small screens
- Grid layout on larger screens
- Touch-friendly button sizes (min 44px)

## Feature Implementation Details

### Search & Filter Logic
```typescript
const filteredSnippets = useMemo(() => {
  return snippets.filter(snippet => {
    const matchesSearch =
      snippet.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
      snippet.tags.some(tag => tag.toLowerCase().includes(searchQuery.toLowerCase()));

    const matchesLanguage =
      selectedLanguage === '' || snippet.language === selectedLanguage;

    return matchesSearch && matchesLanguage;
  });
}, [snippets, searchQuery, selectedLanguage]);
```

### Export Functionality
```typescript
function exportSnippets(snippets: Snippet[]) {
  const dataStr = JSON.stringify(snippets, null, 2);
  const dataBlob = new Blob([dataStr], { type: 'application/json' });
  const url = URL.createObjectURL(dataBlob);
  const link = document.createElement('a');
  link.href = url;
  link.download = `codeclip-snippets-${Date.now()}.json`;
  link.click();
  URL.revokeObjectURL(url);
}
```

### Import Functionality
```typescript
function importSnippets(file: File, onSuccess: (snippets: Snippet[]) => void) {
  const reader = new FileReader();
  reader.onload = (e) => {
    try {
      const snippets = JSON.parse(e.target?.result as string);
      // Validate snippet structure
      onSuccess(snippets);
    } catch (error) {
      console.error('Invalid JSON file');
    }
  };
  reader.readAsText(file);
}
```

### Syntax Highlighting Setup
```typescript
import { Prism as SyntaxHighlighter } from 'react-syntax-highlighter';
import { vscDarkPlus, vs } from 'react-syntax-highlighter/dist/esm/styles/prism';

<SyntaxHighlighter
  language={snippet.language}
  style={theme === 'dark' ? vscDarkPlus : vs}
  customStyle={{ borderRadius: '0.5rem' }}
>
  {snippet.code}
</SyntaxHighlighter>
```

## Performance Optimization

### useCallback Usage
Wrap event handlers passed as props to child components:
```typescript
const handleDelete = useCallback((id: string) => {
  dispatch({ type: 'DELETE_SNIPPET', payload: id });
}, []);
```

### useMemo Usage
Memoize expensive computations:
```typescript
const filteredSnippets = useMemo(() => {
  // Filtering logic
}, [snippets, searchQuery, selectedLanguage]);

const languages = useMemo(() => {
  return Array.from(new Set(snippets.map(s => s.language))).sort();
}, [snippets]);
```

### Code Splitting (Optional)
```typescript
const SyntaxHighlighter = lazy(() => import('react-syntax-highlighter'));
```

## Error Handling

### localStorage Errors
```typescript
try {
  localStorage.setItem(key, JSON.stringify(value));
} catch (error) {
  if (error.name === 'QuotaExceededError') {
    console.error('localStorage quota exceeded');
  }
}
```

### Clipboard Errors
```typescript
try {
  await navigator.clipboard.writeText(text);
  setCopied(true);
} catch (error) {
  console.error('Failed to copy to clipboard', error);
}
```

### Import Validation
```typescript
function validateSnippet(obj: any): obj is Snippet {
  return (
    typeof obj.id === 'string' &&
    typeof obj.title === 'string' &&
    typeof obj.language === 'string' &&
    typeof obj.code === 'string' &&
    Array.isArray(obj.tags)
  );
}
```

## Supported Languages

Provide a predefined list of popular languages:
```typescript
const LANGUAGES = [
  'javascript',
  'typescript',
  'python',
  'java',
  'cpp',
  'csharp',
  'go',
  'rust',
  'ruby',
  'php',
  'swift',
  'kotlin',
  'sql',
  'html',
  'css',
  'bash',
  'json',
  'yaml',
  'markdown',
];
```

## Testing Considerations

### Unit Test Targets
- Custom hooks (useLocalStorage, useTheme)
- Reducer logic (all actions)
- Utility functions (validation, filtering)

### Integration Test Targets
- Create snippet flow
- Edit snippet flow
- Delete snippet with confirmation
- Search and filter combination
- Export/import round-trip

### E2E Test Scenarios
- Complete CRUD cycle
- Dark mode persistence
- localStorage persistence
- Copy to clipboard feedback

## Development Workflow

### File Creation Order
1. Types (`src/types/index.ts`)
2. Reducer (`src/reducers/snippetReducer.ts`)
3. Custom hooks (`src/hooks/*.ts`)
4. Utility functions (`src/utils/*.ts`)
5. Presentational components (SnippetCard, SearchBar)
6. Container components (SnippetList, SnippetForm)
7. Main App component
8. Entry point (main.tsx)

### Development Commands
```bash
npm run dev       # Start dev server on port 3000
npm run build     # Build for production
npm run preview   # Preview production build
npm run lint      # Run ESLint
```

## Security Considerations

### XSS Prevention
- Sanitize user input (title, tags)
- Use react-syntax-highlighter for code display (safe)
- Avoid dangerouslySetInnerHTML

### localStorage Security
- Data is not encrypted in localStorage
- Sensitive code should not be stored
- Consider adding a warning for sensitive snippets

### Content Security Policy
- No external script loading
- All assets served from same origin

## Accessibility

### Keyboard Navigation
- Tab order should be logical
- Enter to submit forms
- Escape to cancel/close modals
- Space to toggle checkboxes/favorites

### ARIA Labels
```tsx
<button aria-label="Copy code to clipboard">
<button aria-label="Toggle dark mode">
<input aria-label="Search snippets">
```

### Focus Management
- Focus trap in modals/forms
- Visible focus indicators
- Focus on first input when opening form

### Color Contrast
- WCAG AA compliant
- Test with accessibility tools
- Don't rely solely on color for information

## Browser Compatibility

### Target Browsers
- Chrome/Edge 90+
- Firefox 88+
- Safari 14+

### Required APIs
- localStorage (IE8+)
- Clipboard API (Chrome 63+, Firefox 53+, Safari 13.1+)
- CSS Custom Properties
- ES2020 features

### Fallbacks
- Clipboard: Use execCommand('copy') fallback
- Dark mode: Graceful degradation without system preference

## Future Enhancements

### Phase 2 Features
- Folders/categories for organization
- Syntax highlighting themes selection
- Keyboard shortcuts overlay
- Snippet templates
- Duplicate snippet function
- Bulk operations (delete, export selected)

### Phase 3 Features
- Cloud sync (Firebase/Supabase)
- Sharing snippets via URL
- Collaboration features
- Version history
- Code execution (sandbox)
- AI-powered snippet suggestions

### Technical Debt
- Add comprehensive tests
- Implement proper error boundaries
- Add loading states for async operations
- Optimize re-renders with React.memo
- Add internationalization (i18n)

## Deployment

### Build Configuration
```bash
npm run build
# Output: dist/ directory
```

### Hosting Options
- **Vercel**: Zero-config deployment
- **Netlify**: Drag-and-drop or Git integration
- **GitHub Pages**: Static site hosting
- **Cloudflare Pages**: Fast global CDN

### Environment Variables
No environment variables needed for basic version.
For cloud sync, add:
- `VITE_API_URL`
- `VITE_API_KEY`

## Portfolio Presentation

### Key Highlights
1. **Advanced React Patterns**: useReducer, custom hooks, composition
2. **TypeScript Proficiency**: Full type safety, interfaces, generics
3. **Modern Tooling**: Vite, Tailwind CSS, ESLint
4. **Code Quality**: Clean architecture, separation of concerns
5. **UX Polish**: Dark mode, responsive, accessible
6. **Performance**: Optimized renders, efficient filtering

### Demo Scenarios
1. Create a few sample snippets
2. Demonstrate search and filter
3. Show dark mode toggle
4. Export and import snippets
5. Copy to clipboard functionality
6. Responsive design on mobile

### Code Walkthrough Points
- Explain useReducer vs useState decision
- Show custom hook implementations
- Discuss component composition
- Highlight TypeScript types
- Explain performance optimizations

## Common Issues & Solutions

### localStorage Full
```typescript
// Implement cleanup for old snippets
// Add warning when approaching quota
// Provide export before cleanup
```

### Slow Rendering with Many Snippets
```typescript
// Implement virtualization (react-window)
// Paginate snippets
// Lazy load syntax highlighter
```

### Tags Input UX
```typescript
// Use comma/Enter to add tags
// Show tag suggestions
// Validate tag format
```

## Contributing Guidelines

When extending this project:
1. Follow existing code style
2. Maintain type safety
3. Write meaningful commit messages
4. Update this documentation
5. Add tests for new features
6. Ensure accessibility compliance

## Resources

### Documentation
- [React Docs](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [Vite Guide](https://vitejs.dev/guide/)

### Libraries
- [react-syntax-highlighter](https://github.com/react-syntax-highlighter/react-syntax-highlighter)
- [Prism Languages](https://prismjs.com/#supported-languages)

### Design Inspiration
- GitHub Gists
- CodePen
- Carbon (code screenshots)
- Ray.so

---

## Quick Start for Claude

When implementing this project:

1. **Start with types** - Define all interfaces first
2. **Build the reducer** - Implement state management logic
3. **Create custom hooks** - Extract reusable logic
4. **Build components bottom-up** - Start with SnippetCard, then SnippetList, etc.
5. **Wire up App.tsx** - Connect all pieces together
6. **Polish UI/UX** - Add animations, responsive design, dark mode
7. **Test thoroughly** - Manual testing of all features

Remember to use useCallback for event handlers and useMemo for computed values to optimize performance!
