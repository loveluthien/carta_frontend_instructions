# CARTA Frontend Development Guide

## Overview
CARTA is a radio-astronomy visualization tool. This React + TypeScript frontend communicates the backend through the WebSocket for image processing, uses WebAssembly modules for computation, and relies on Protocol Buffers for data exchange.

## Architecture

### State Management (MobX)
- **Singleton Stores**: Core state lives in singleton stores (e.g., `AppStore.Instance`, `WidgetsStore.Instance`)
- **MobX Decorators**: Use `@observable`, `@action`, `@computed` for reactive state
- **Components**: Mark observer components with `@observer` decorator from `mobx-react`
- **Example**: See `src/stores/AppStore/AppStore.ts` (3600+ lines, central orchestrator)

### Key Services
- **BackendService**: WebSocket connection to CARTA backend, Protocol Buffer message handling
- **TileService**: Image tile streaming and caching for efficient rendering
- **TileWebGLService**: WebGL2-based tile rendering with colormap support
- **ApiService**: HTTP API for configuration and runtime settings
- **ScriptingService**: Python scripting interface for automation

### UI Layout
- Using `flexlayout-react` for panel layout management. 
- Widgets (histogram, spectral profiles, etc.) are dynamically created/destroyed
- Each widget type has a corresponding store in `src/stores/Widgets/`

### WebAssembly Integration
Two build stages required:
1. **WASM Libraries** (`wasm_libs/`): AST (astronomical coordinate transforms), GSL (math), ZFP (compression), zstd (compression)
2. **WASM Wrappers** (`wasm_src/`): TypeScript-callable wrappers for the above libraries

Built using Emscripten (4.0.3 recommended), requires Docker/Singularity OR native emcc installation.

### Protocol Buffers
- Submodule at `protobuf/` (carta-protobuf repo)
- Messages imported as `import {CARTA} from "carta-protobuf"`
- Must run `protobuf/build_proto.sh` after protobuf changes
- Backend/frontend ICD version currently: 30 (see `BackendService.IcdVersion`)

### File Organization
- **Components**: `src/components/` - React UI components
- **Stores**: `src/stores/` - MobX state management (one folder per store)
- **Services**: `src/services/` - Backend communication, WebGL rendering
- **Models**: `src/models/` - Type definitions and data structures
- **Utilities**: `src/utilities/` - Helper functions (AST wrappers, parsing, sorting, etc.)
- **Enums**: `src/enums/` - Enumeration definitions

### TypeScript Configuration
- `baseUrl: "./src"` allows absolute imports from src root
- Experimental decorators enabled for MobX

### Component Patterns
```typescript
// Observer component pattern
@observer
export class MyComponent extends React.Component<MyProps> {
    render() {
        const appStore = AppStore.Instance; // Access singleton stores
        // Component renders automatically on observable changes
    }
}
```

### Store Patterns
```typescript
export class MyStore {
    private static staticInstance: MyStore;
    static get Instance() { /* singleton */ }
    
    @observable myState: string;
    @computed get derivedValue() { /* ... */ }
    @action updateState(newValue: string) { /* ... */ }
    
    constructor() {
        makeObservable(this); // Required for decorators
    }
}
```

## Development Workflows

- Code should follow the clean code principle.
- Use LLM following the skill [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills).

### Build Commands
See build skill in `skills/build-frontend/SKILL.md` for detailed build commands.

### Naming Conventions
Use the instruction file [style.instructions.md](./style.instructions.md) for detailed naming conventions.

### Grammar
- Use present tense verbs (is, open) instead of past tense (was, opened)
- Write factual statements and direct commands. Avoid hypotheticals like "could" or "would"
- Use active voice where the subject performs the action

### Run checks
After codebase changed, you should run the following checks to make sure the codebase is still in good condition:
- `npm run fix-eslint`
- `npm run reformat`
- `npm test`
- `npm run build-ts` or `npm run build` if `npm install` was performed

### MCP server
- Chrome devtools MCP
- Start CARTA backend with `build/carta_backend --omp_threads 8 --debug_no_auth --verbosity 5 --no_browser` in a user specific carta-backend path
- Default backend is at `http://localhost:3000`

### Write unit test
- Unit tests use Jest with canvas mocking. See `src/setupTests.js` for configuration.
- Tests are colocated with source files using `.test.ts/tsx` suffix. Follow the test structure guidelines in `skills/unit-test/SKILL.md`.

### Commit
- Use the skill [git-commit](https://skills.sh/github/awesome-copilot/git-commit) to create well-formatted commit messages that follow the Conventional Commits specification.
- Make multiple commits if the changes are large.

### Change log update
Use the skill [changelog](../skills/changelog/SKILL.md) to update the `CHANGELOG.md` file in the root directory.

### Write down TSdoc
When you modify any exported function or class, you should also write or update its TSDoc comments to keep the documentation up-to-date. The TSDoc comments should be written in the same style as the existing TSDoc comments in the codebase.

- Write clear and concise documentation
- Use consistent terminology and style
- Code comments use TSDoc format




## Key Integration Points

### WebAssembly Modules
- **ast_wrapper**: Coordinate system transforms (accessed via `import * as AST from "ast_wrapper"`)
- **carta_computation**: Image statistics and computations (accessed via `import * as CARTACompute from "carta_computation"`)
- **zfp_wrapper**: ZFP decompression for tiled image data
- **gsl_wrapper**: Statistical functions

Check readiness: `AppStore.Instance.astReady`, `AppStore.Instance.cartaComputeReady`

### Backend Communication
- WebSocket connection status: `BackendService.Instance.connectionStatus`
- Observable streams for tile data: `rasterTileStream`, `rasterSyncStream`
- All backend messages use Protocol Buffer format from `carta-protobuf`

### Image Rendering Pipeline
1. Backend sends compressed tiles via WebSocket
2. `TileService` decompresses and caches tiles
3. `TileWebGLService` renders tiles to canvas using WebGL2 shaders (see `src/services/GLSL/`)
4. Colormap textures loaded from `src/static/allmaps.png`

## Common Tasks

### Working with Regions
- Region stores in `src/stores/Frame/` (RegionStore, PointAnnotationStore, etc.)
- Cursor region has special ID: `CURSOR_REGION_ID = -1`
- Region rendering via Konva (react-konva) in ImageView

### Modifying Protocol Buffers
1. Create independent branch in `carta-protobuf` repo
2. Edit `.proto` files in `protobuf/` submodule
3. Commit changes to `carta-protobuf` repo
4. Update submodule reference in frontend

## Dependencies to Note
- **Blueprint.js**: UI component library (buttons, dialogs, etc.)
- **Konva/react-konva**: Canvas-based region rendering
- **Chart.js/react-chartjs-2**: Profile plot widgets
- **Flexlayout-react**: Layout manager
- **plotly.js**: Advanced plotting features