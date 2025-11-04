# CodeClip - Advanced Code Snippet Manager

A modern, feature-rich code snippet manager built with React, TypeScript, and Tailwind CSS. This project showcases advanced React patterns and best practices for a portfolio-ready application.

## Features

### Core Functionality
- **CRUD Operations**: Create, read, update, and delete code snippets
- **Persistent Storage**: All snippets saved to localStorage
- **Syntax Highlighting**: Beautiful code highlighting with react-syntax-highlighter
- **Search & Filter**: Search by title/tags and filter by programming language
- **Copy to Clipboard**: One-click copying with visual feedback
- **Tags System**: Organize snippets with multiple tags

### Advanced Features
- **Dark/Light Mode**: Toggle with theme persistence
- **Favorites**: Star important snippets for quick access
- **Export/Import**: Backup and restore snippets as JSON
- **Recently Used**: Quick access to recent snippets
- **Responsive Design**: Mobile-friendly interface

## Tech Stack

- **React 18** with TypeScript
- **Vite** for fast development and building
- **Tailwind CSS** for styling
- **react-syntax-highlighter** for code display

## Advanced React Patterns

This project demonstrates:

1. **useReducer for State Management**: Complex state logic with actions for add, update, delete, filter, and search
2. **Custom Hooks**:
   - `useLocalStorage` - Syncs state with localStorage
   - `useTheme` - Manages dark mode with persistence
   - `useCopyToClipboard` - Handles clipboard operations
3. **Component Composition**: Well-organized, single-responsibility components
4. **Performance Optimization**: useCallback and useMemo to prevent unnecessary re-renders
5. **TypeScript**: Full type safety throughout the application

## Getting Started

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Project Structure

```
src/
├── components/           # React components
│   ├── Header.tsx       # App header with theme toggle
│   ├── SearchBar.tsx    # Search and filter controls
│   ├── SnippetForm.tsx  # Create/edit snippet form
│   ├── SnippetList.tsx  # List of snippets
│   ├── SnippetCard.tsx  # Individual snippet display
│   └── ...
├── hooks/               # Custom React hooks
│   ├── useLocalStorage.ts
│   ├── useTheme.ts
│   └── useCopyToClipboard.ts
├── reducers/            # State management
│   └── snippetReducer.ts
├── types/               # TypeScript types
│   └── index.ts
├── utils/               # Utility functions
│   └── ...
├── styles/              # CSS files
│   └── index.css
├── App.tsx              # Main app component
└── main.tsx            # Entry point
```

## Component Architecture

### State Management
The app uses `useReducer` for managing snippet state with the following actions:
- `ADD_SNIPPET` - Add a new snippet
- `UPDATE_SNIPPET` - Edit existing snippet
- `DELETE_SNIPPET` - Remove a snippet
- `TOGGLE_FAVORITE` - Star/unstar snippet
- `SET_SNIPPETS` - Load snippets from localStorage
- `IMPORT_SNIPPETS` - Import from JSON file

### Custom Hooks

**useLocalStorage**
```typescript
const [snippets, setSnippets] = useLocalStorage<Snippet[]>('snippets', []);
```
Automatically syncs state with localStorage.

**useTheme**
```typescript
const { theme, toggleTheme } = useTheme();
```
Manages dark mode with persistence and system preference detection.

**useCopyToClipboard**
```typescript
const { copied, copyToClipboard } = useCopyToClipboard();
```
Handles clipboard operations with feedback state.

## Key Features Implementation

### Search & Filter
- Real-time search across titles and tags
- Filter by programming language
- Combined search and filter logic

### Export/Import
- Export all snippets to JSON file
- Import snippets from JSON with validation
- Merge or replace existing snippets

### Dark Mode
- Tailwind CSS dark mode classes
- Persisted preference in localStorage
- Smooth transitions between themes

## Browser Support

Modern browsers with ES2020 support:
- Chrome/Edge 90+
- Firefox 88+
- Safari 14+

## License

MIT

## Portfolio Highlights

This project demonstrates:
- Advanced React patterns (useReducer, custom hooks)
- TypeScript proficiency
- Modern CSS with Tailwind
- Clean code architecture
- Performance optimization
- User experience design
- Responsive web design
