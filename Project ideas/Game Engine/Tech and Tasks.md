## 🧩 0. Authentication & Workspace

### ✅ Tech Stack (all open source)

- **Backend:** [Supabase](https://supabase.com/) (open-source alternative to Firebase) or [PocketBase](https://pocketbase.io/) 
- **Frontend:** React, TypeScript, Zustand (for app state)
- **Auth:** Supabase Auth (email + OTP) or PocketBase Auth

### ✅ Tasks

-  Email + OTP login using open-source backend.
-  JWT session management.
-  User data storage (projects, settings).
-  Local backup (IndexedDB).
-  Self-hosted Supabase/PocketBase option.

---

## 🏠 1. Dashboard / Project Manager

### ✅ Tech Stack

- React + Tailwind for UI
- Zustand or Jotai for state management
- IndexedDB / Supabase for project data

### ✅ Tasks

-  Create new project (metadata + folder structure).
-  Project list view with search/sort.
-  Import/export projects (.zip or JSON).
-  Thumbnail generation (canvas snapshot).
-  LocalStorage or Supabase sync.

---

## 🎨 2. Editor UI (Main Canvas)

### ✅ Tech Stack

- **Rendering:** [Three.js](https://threejs.org/)
- **React Renderer:** [React Three Fiber](https://github.com/pmndrs/react-three-fiber)
- **Helpers:** [Drei](https://github.com/pmndrs/drei)
- **Gizmos & Controls:** drei/TransformControls, drei/OrbitControls
- **UI:** [Leva](https://github.com/pmndrs/leva) (properties), [React Flow](https://reactflow.dev/) for node editors

### ✅ Tasks

-  Core panels: Outline, Asset Manager, Properties, Viewport.
-  Entity tree & selection logic.
-  Add/remove meshes, cameras, lights.
-  Add preset human model (open GLTF like Mixamo sample).
-  Gizmos (translate, rotate, scale).
-  Scene save/load (JSON format).
-  Undo/redo command stack.

---

## 📦 3. Asset Manager

### ✅ Tech Stack

- [File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API)
- [Supabase Storage](https://supabase.com/storage) (optional)
- [GLTFLoader, TextureLoader] (Three.js)

### ✅ Tasks

-  Browse local or uploaded assets.
-  Upload GLTF, FBX, textures, audio.
-  Integrate external APIs via keys (e.g., [Sketchfab API](https://sketchfab.com/developers), [Polyhaven API](https://polyhaven.com/docs)).
-  Asset previews.
-  Drag/drop to scene.

---

## 🧍 4. Character & Armature Configurator

### ✅ Tech Stack

- [Three.js SkeletonHelper](https://threejs.org/docs/#api/en/helpers/SkeletonHelper)
- [GLTFLoader]
- Custom React UI for bone mapping

### ✅ Tasks

-  Upload GLTF/FBX character.
-  Visualize bones.
-  Manual bone-mapping (mixamo-like).
-  Animation playback & retargeting.
-  Save rig metadata as JSON.

---

## 🌆 5. Scene Setup Tools

### ✅ Tech Stack

- React Three Fiber + Drei (TransformControls)
- [three-mesh-bvh](https://github.com/gkjohnson/three-mesh-bvh) for optimized mesh operations

### ✅ Tasks

-  Scene hierarchy & nesting.
-  Transform gizmos (TRS).
-  LOD configurator.
-  Array tool for duplication.
-  Export/import scenes (.json).
-  Multiple scenes with switching logic.

---

## 🧱 6. UI Configurator (Game UI Builder)

### ✅ Tech Stack

- [GrapesJS](https://grapesjs.com/) (open-source HTML/CSS visual editor)
- React integration wrapper
- [Monaco Editor](https://github.com/microsoft/monaco-editor) for JS editing (open-source)

### ✅ Tasks

-  Drag-and-drop HTML/CSS builder.
-  Bind UI widgets to game events.
-  Preview in overlay.
-  Save UI schema as reusable widgets.
-  Import/export widget sets.

---

## 🧠 7. Flow Manager (Node-based Logic)

### ✅ Tech Stack

- [React Flow](https://reactflow.dev/) (open-source visual node editor)
- Zustand for node state
- [dagre](https://github.com/dagrejs/dagre) for auto layout

### ✅ Tasks

-  Node graph editor.
-  Nodes for triggers, actions, logic.
-  Connect game entities via events.
-  JSON serialization.
-  Runtime evaluator (executes node graphs).
-  Debug mode for live logic flow.

---

## ⚙️ 8. Collider / Portal / Scene Manager

### ✅ Tech Stack

- [Rapier.js](https://rapier.rs/) (open-source Rust-based physics engine for JS)
- [three-rapier](https://github.com/pmndrs/react-three-rapier)
- [Three.js layers/scenes API]

### ✅ Tasks

-  Add collider shapes (box, sphere, capsule).
-  Portal component (teleport player, load next scene).
-  Physics simulation toggle.
-  Scene streaming & manager.

---

## 🎬 9. Post-Processing & Transitions

### ✅ Tech Stack

- [postprocessing](https://github.com/pmndrs/postprocessing)
- Custom GLSL shaders via R3F
- [React Spring](https://react-spring.dev/) or [Framer Motion](https://www.framer.com/motion/) for UI transitions

### ✅ Tasks

-  Visual configurator for effects (bloom, DOF, SSAO, tone mapping).
-  Shader graph editor (optional – via React Flow).
-  Scene transition manager.
-  Save presets as postFX profiles.

---

## 💻 10. JS/TS Logic Configurator

### ✅ Tech Stack

- [Monaco Editor](https://microsoft.github.io/monaco-editor/)
- [esbuild-wasm](https://github.com/evanw/esbuild) for in-browser TypeScript compilation
- [vm2](https://github.com/patriksimek/vm2) (sandboxed execution, open-source)

### ✅ Tasks

-  In-editor code editor.
-  TypeScript transpilation in browser.
-  Hot reload scripts to entities.
-  Expose API for scene manipulation.
-  Sandbox execution environment.

---

## 🧰 11. Suggested Additions (All Open Source)

### 🧩 ECS Architecture

- [bitecs](https://github.com/NateTheGreatt/bitECS) — minimal, fast ECS for JS.
- Components: Transform, MeshRenderer, Collider, Script, Light.

### 🧰 Plugin System

-  Define plugin API (React hooks + runtime injection).
-  Allow registering custom nodes, components, panels.

### 💾 Persistence & Versioning

- [Gun.js](https://gun.eco/) for peer-to-peer data sync (open source, offline-first).
-  Auto-save scenes to local + sync with Supabase.

### 🧠 AI/Procedural Tools (optional)

-  Integrate open LLM API wrappers (Ollama/local models) — for procedural code/scene generation.
-  OpenNoise / FastNoiseLite for terrain generation.

### 🎮 Runtime Export

-  Scene compiler → single JS bundle.
-  Export runtime as embeddable web component.
-  Optional multiplayer via [Colyseus](https://www.colyseus.io/) (open source).

---

## 🗂️ Suggested Folder Structure

```
/src
 ├─ auth/              → Supabase / PocketBase integration
 ├─ dashboard/         → Project management UI
 ├─ editor/
 │   ├─ outline/
 │   ├─ asset-manager/
 │   ├─ properties/
 │   ├─ viewport/
 │   ├─ ui-configurator/
 │   ├─ flow-manager/
 │   ├─ collider-manager/
 │   ├─ postprocessing/
 │   ├─ logic-configurator/
 ├─ runtime/
 │   ├─ ecs/
 │   ├─ physics/
 │   ├─ renderer/
 │   ├─ scripting/
 ├─ api/
 ├─ utils/
```

---

## 🌱 Optional Roadmap by Stage

|Stage|Goal|Key Modules|
|---|---|---|
|**MVP 1**|Load + render scene|Viewport, Outline, Properties|
|**MVP 2**|Add logic & physics|Flow Manager, Collider|
|**MVP 3**|PostFX + UI Builder|Postprocessing, UI Configurator|
|**Beta**|Scripting + Runtime Export|Logic Configurator, Scene Export|
|**Release**|Plugin + Cloud Sync|Plugin API, Supabase Integration|
