# 🤖 AI Agent Development Guide

Context engineering for Bingo Mixer — a social bingo game built with React, TypeScript, Tailwind CSS v4, and Vite.

## ✅ Development Checklist
- [ ] **Lint**: `npm run lint` (ESLint with TS + React Hooks rules)
- [ ] **Build**: `npm run build` (TypeScript check + Vite bundle)
- [ ] **Test**: `npm test` (Vitest with jsdom, 41 tests cover bingoLogic)
- [ ] **Dev**: `npm run dev` (Vite server on localhost:5173, live reload)

---

## 📋 Project Quick Facts

| Aspect | Details |
|--------|---------|
| **Framework** | React 19 + TypeScript 5.9 |
| **Build Tool** | Vite 8 |
| **Styling** | Tailwind CSS v4 with @tailwindcss/vite |
| **Testing** | Vitest + @testing-library/react |
| **Linting** | ESLint (FlatConfig) with TS + React plugins |
| **Deployment** | GitHub Pages (auto-deploy on `main` push) |
| **Node Version** | 22+ (in `.devcontainer`) |
| **Dev Container** | Included (Microsoft typescript-node:22 image) |

---

## 🏗️ Architecture Patterns

### State Management: Custom Hook Pattern
- **Central hook**: [`src/hooks/useBingoGame.ts`](src/hooks/useBingoGame.ts)
  - Manages game state (`start` → `playing` → `bingo`)
  - Persists to `localStorage` with version validation
  - Provides: board generation, square toggle, bingo detection, reset
  - **Why**: Encapsulates complex game logic, reusable, testable

### Game Logic: Pure Utility Functions
- **File**: [`src/utils/bingoLogic.ts`](src/utils/bingoLogic.ts)
  - `generateBoard()` — Create 5×5 board with 24 random questions + free center
  - `toggleSquare()` — Immutable square toggle (no free space mutation)
  - `checkBingo()` — Detect 5-in-a-row (rows, columns, diagonals)
  - `getWinningSquareIds()` — Extract winning line squares
  - **Why**: Pure, testable functions. Fully unit tested (41 tests).

### Components: Compositional Hierarchy
- **`App.tsx`** → Entry point; routes between screens
- **`GameScreen`** → Main game UI container
- **`BingoBoard`** → 5×5 grid layout
- **`BingoSquare`** → Individual cell (clickable, marked state)
- **`StartScreen`** → Game start flow
- **`BingoModal`** → Win celebration overlay

**Convention**: Props flow down, callbacks up. No prop drilling; use context if needed.

### Data: Centralized Question Pool
- **File**: [`src/data/questions.ts`](src/data/questions.ts)
- 24 icebreaker questions (+ 1 free space)
- Imported by `bingoLogic.generateBoard()` for random selection
- **To customize**: Edit questions array; rerun tests

### Types: Single Source of Truth
- **File**: [`src/types/index.ts`](src/types/index.ts)
- Export all shared types: `BingoSquareData`, `BingoLine`, `GameState`
- **Convention**: Import types with `type` keyword (TypeScript 5.9+)

---

## 🎨 Styling Conventions

**Tailwind CSS v4** with `@tailwindcss/vite` plugin:
- **CSS-first config**: Uses `@theme` in CSS, no `tailwind.config.js`
- **Color system**: CSS variables + `color-mix()` for dynamic theming
- **Responsive**: Tailwind defaults (`sm:`, `md:`, `lg:`, `xl:`, `2xl:`)
- **Design tokens**: Define in `src/index.css` under `@theme` directive
- **See also**: [`.github/instructions/tailwind-4.instructions.md`](.github/instructions/tailwind-4.instructions.md)

**Frontend Design Skill**: When redesigning UI, use [`.github/instructions/frontend-design.instructions.md`](.github/instructions/frontend-design.instructions.md) for creative, polished aesthetics (avoid "AI slop").

---

## 🧪 Testing Strategy

- **Test file**: [`src/utils/bingoLogic.test.ts`](src/utils/bingoLogic.test.ts)
- **Test runner**: Vitest with jsdom environment
- **Setup**: Extends `@testing-library/jest-dom` matchers
- **Naming**: Descriptive test names (e.g., `should detect bingo after toggling an entire row`)
- **Coverage**: Game logic 100% tested. Components tested via integration when redesigned.
- **Command**: `npm test` (no watch by default; use `npx vitest` for watch mode)

---

## 🚀 Development Workflow

### First Time Setup
```bash
npm install                    # Install dependencies
npm run dev                    # Start Vite dev server (auto-open in browser)
```

### Making Changes
1. Edit source files in `src/`
2. Vite watches and auto-reloads browser
3. Run `npm run lint` to check quality
4. Run `npm test` to validate logic
5. Commit when all checks pass

### Before Pushing
```bash
npm run lint   # Fix linting issues
npm test       # Ensure tests pass
npm run build  # Verify production build
git add .
git commit -m "feat: ..."
git push       # Auto-deploys to GitHub Pages
```

---

## 📚 Documentation & Resources

**Essential Reading**:
- [README.md](README.md) — Project overview
- [workshop/GUIDE.md](workshop/GUIDE.md) — Full lab guide (01-04)
- [workshop/01-setup.md](workshop/01-setup.md) — Context engineering tasks

**Customization Files** (for agent reference):
- [`.github/instructions/frontend-design.instructions.md`](.github/instructions/frontend-design.instructions.md) — Design skill (creativity, aesthetics)
- [`.github/instructions/tailwind-4.instructions.md`](.github/instructions/tailwind-4.instructions.md) — Tailwind CSS v4 patterns

---

## ⚠️ Common Patterns & Gotchas

| Pattern | Gotcha | Solution |
|---------|--------|----------|
| **Board generation** | Questions must be unique | `shuffleArray()` handles this; tests verify uniqueness |
| **Immutability** | State mutations break React | Use `.map()` not `.splice()`; `toggleSquare()` returns new array |
| **Free space** | Center square (index 12) should never toggle | `toggleSquare()` checks `isFreeSpace` flag |
| **Bingo detection** | Edge case: fully marked board | `checkBingo()` returns first match (rows first, then columns, diagonals) |
| **localStorage** | Version mismatch breaks persistence | `validateStoredData()` checks version; resets on mismatch |
| **TypeScript strict mode** | Strict null checking enabled | Use type guards; import types with `type` keyword |

---

## 🎯 AI Agent Prompt Examples

### For Feature Development
> "Implement [feature] following the custom hook + utility function pattern. Write pure functions in utils/, house state in useBingoGame hook. Add tests to bingoLogic.test.ts."

### For Redesign/Styling
> "Use the frontend-design skill and Tailwind CSS v4. Create a [theme] aesthetic. Reference .github/instructions/frontend-design.instructions.md for avoiding generic AI outputs."

### For Debugging
> "Run `npm run lint` and `npm test`. Check the failing test output. Use the pattern in bingoLogic.ts for immutable state."

---

## 📞 Questions?

- **Architecture**: Check [src/hooks/useBingoGame.ts](src/hooks/useBingoGame.ts) and [src/utils/bingoLogic.ts](src/utils/bingoLogic.ts)
- **Styling**: See [.github/instructions/tailwind-4.instructions.md](.github/instructions/tailwind-4.instructions.md)
- **Lab tasks**: Read [workshop/GUIDE.md](workshop/GUIDE.md) for full context
