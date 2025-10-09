> [!NOTE]
> Each card includes:
> 
> - **Title**
> - **Short description**
> - **Labels:** (`core`, `ui`, `logic`, `backend`, `optional`)
> - **Priority:** 🟥 High / 🟧 Medium / 🟩 Low
> - **Dependencies:** what needs to be done before it
> 

---

## 🏁 **MVP 1 — Core Editor Foundation**

Goal: get a minimal working editor — create a project, load scene, add lights/objects, save.

---

### 🟥 [Core] Authentication System

- Email + OTP login using **Supabase** or **PocketBase**
    
- Store user sessions locally  
    **Labels:** `backend`, `core`  
    **Priority:** 🟥  
    **Depends on:** none

---

### 🟥 [Core] Dashboard & Project Manager

- Create/delete/import/export projects
    
- Save project metadata (title, scene list)  
    **Labels:** `core`, `ui`  
    **Priority:** 🟥  
    **Depends on:** Authentication

---

### 🟥 [Core] Editor UI Layout

- Implement layout with **Outline**, **Viewport**, **Properties**, **Asset Manager** panels
    
- Use React, Tailwind, Zustand for state  
    **Labels:** `core`, `ui`  
    **Priority:** 🟥  
    **Depends on:** Dashboard

---

### 🟥 [Core] Scene View (Viewport)

- Integrate **Three.js + React Three Fiber + Drei**
    
- Render grid, add/remove meshes, cameras, lights  
    **Labels:** `core`, `rendering`  
    **Priority:** 🟥  
    **Depends on:** Editor UI


---

### 🟧 [Core] Transform Gizmos

- Use **drei/TransformControls**
    
- Manipulate selected object (move/rotate/scale)  
    **Labels:** `ui`, `interaction`  
    **Priority:** 🟧  
    **Depends on:** Scene View

---

### 🟩 [Core] Scene Save/Load

- Save current scene graph as JSON
    
- Reload on project open  
    **Labels:** `core`, `data`  
    **Priority:** 🟩  
    **Depends on:** Scene View

---

## 🚀 **MVP 2 — Logic & Physics Layer**

Goal: basic interactivity + physics + flow control.

---

### 🟥 [Logic] Flow Manager (Node Graph)

- Visual scripting using **React Flow**
    
- Add nodes: event → action → condition → output  
    **Labels:** `logic`, `ui`  
    **Priority:** 🟥  
    **Depends on:** Scene Save/Load

---

### 🟥 [Physics] Collider & RigidBody System

- Use **Rapier.js / react-three-rapier**
    
- Add box/sphere colliders, enable physics simulation toggle  
    **Labels:** `physics`, `core`  
    **Priority:** 🟥  
    **Depends on:** Scene View

---

### 🟧 [Logic] Entity Component System (ECS)

- Integrate **bitecs**
    
- Components: Transform, MeshRenderer, Collider, Script  
    **Labels:** `core`, `logic`  
    **Priority:** 🟧  
    **Depends on:** Collider System

---

### 🟩 [Logic] Portal & Multi-Scene Manager

- Scene switching and loading system  
    **Labels:** `logic`, `rendering`  
    **Priority:** 🟩  
    **Depends on:** Scene Save/Load

---

## 💎 **MVP 3 — Visual Enhancements & UI System**

Goal: make it look/feel like a true game editor.

---

### 🟥 [Render] Post-Processing Configurator

- Integrate **pmndrs/postprocessing**
    
- Add UI to toggle Bloom, DOF, SSAO, etc.  
    **Labels:** `rendering`, `ui`  
    **Priority:** 🟥  
    **Depends on:** Scene View

---

### 🟥 [UI] UI Builder (HTML/CSS/JS)

- Integrate **GrapesJS** + **Monaco Editor**
    
- Create reusable widget components  
    **Labels:** `ui`, `logic`  
    **Priority:** 🟥  
    **Depends on:** Flow Manager

---

### 🟧 [Render] Shader Graph Editor

- Build node-based GLSL material builder using **React Flow**  
    **Labels:** `rendering`, `shaders`, `optional`  
    **Priority:** 🟧  
    **Depends on:** Flow Manager

---

### 🟩 [UI] Theme & Layout Manager

- Save panel layout & color themes per user  
    **Labels:** `ui`, `optional`  
    **Priority:** 🟩  
    **Depends on:** Editor UI

---

## 🧠 **BETA — Scripting & Runtime Export**

Goal: add scripting, logic compilation, and game runtime.

---

### 🟥 [Logic] JS/TS Script Configurator

- In-browser **Monaco Editor** + **esbuild-wasm**
    
- Sandbox code execution via **vm2**  
    **Labels:** `logic`, `scripting`  
    **Priority:** 🟥  
    **Depends on:** ECS + Flow Manager

---

### 🟥 [Runtime] Scene Exporter

- Export project as standalone **R3F WebGL runtime bundle**
    
- Includes assets, logic, shaders  
    **Labels:** `core`, `build`  
    **Priority:** 🟥  
    **Depends on:** Scripting Configurator

---

### 🟧 [Backend] Version Control

- Implement **Gun.js** or **Supabase** snapshots
    
- Track scene changes and restore points  
    **Labels:** `backend`, `optional`  
    **Priority:** 🟧  
    **Depends on:** Scene Save/Load

---

## 🧰 **RELEASE — Extensibility & Collaboration**

Goal: make it extensible and community-ready.

---

### 🟥 [Core] Plugin System

- Register new panels, components, nodes via plugin API  
    **Labels:** `core`, `extensibility`  
    **Priority:** 🟥  
    **Depends on:** All previous MVPs

---

### 🟧 [Core] Asset API Integration

- Integrate open APIs (Sketchfab, Polyhaven) via API keys  
    **Labels:** `assets`, `optional`  
    **Priority:** 🟧  
    **Depends on:** Asset Manager

---

### 🟩 [Core] Multiplayer Scene Sync

- Real-time collaboration via **Gun.js** or **Colyseus**  
    **Labels:** `backend`, `optional`  
    **Priority:** 🟩  
    **Depends on:** Version Control

---

### 🟩 [Core] AI Assistant (Optional)

- Integrate local AI models (Ollama, Transformers.js) for scene/code suggestions  
    **Labels:** `ai`, `optional`  
    **Priority:** 🟩  
    **Depends on:** Scripting Configurator

---

## 🗂 Suggested GitHub Project Columns

|Column|Description|
|---|---|
|**Backlog**|Future ideas and optional tasks|
|**To Do**|Ready to start — MVP priority features|
|**In Progress**|Currently being developed|
|**Testing**|QA and internal testing|
|**Done**|Completed & merged into main branch|

---

### 📌 Example Structure in GitHub Projects

**Project name:** `WebGameEngine`  
**Views:**

- **Board** (Kanban)
- **Table (Tasks by Priority)**
- **Roadmap (Milestones by MVP)**

**Labels:**

```
core, ui, logic, rendering, physics, backend,
scripting, optional, ai, extensibility
```

**Milestones:**

```
MVP 1 – Core Editor
MVP 2 – Logic & Physics
MVP 3 – Visual & UI
Beta – Scripting & Runtime
Release – Plugin & Cloud
```

