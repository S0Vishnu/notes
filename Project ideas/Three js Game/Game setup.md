
https://github.com/pmndrs/ecctrl?tab=readme-ov-file

### Prerequisites

1. Install dependencies:
    
    ```bash
    npm install @react-three/fiber @react-three/rapier @react-three/drei @react-three/rapier react-three-gltfloader three
    ```
    
2. Ensure your character model is in **GLTF/GLB** format with animations.
    

---

### Code Example

```tsx
// App.tsx
import React, { useRef, useState, useEffect } from "react";
import { Canvas, useFrame } from "@react-three/fiber";
import { OrbitControls, useGLTF } from "@react-three/drei";
import { Physics, RigidBody, useRapier } from "@react-three/rapier";
import * as THREE from "three";

interface KeyMap {
  forward: boolean;
  backward: boolean;
  left: boolean;
  right: boolean;
  jump: boolean;
  shift: boolean;
}

const Character = () => {
  const characterRef = useRef<THREE.Group>(null);
  const { scene, animations } = useGLTF("/path/to/your/model.glb") as any;
  const [mixer] = useState(() => new THREE.AnimationMixer(scene));
  const actions = useRef<{ [key: string]: THREE.AnimationAction }>({});
  const keys = useRef<KeyMap>({
    forward: false,
    backward: false,
    left: false,
    right: false,
    jump: false,
    shift: false,
  });

  const velocity = useRef(new THREE.Vector3(0, 0, 0));
  const { rapier, world } = useRapier();
  const [canJump, setCanJump] = useState(true);

  useEffect(() => {
    // Load animations
    animations.forEach((clip: THREE.AnimationClip) => {
      actions.current[clip.name] = mixer.clipAction(clip);
    });

    // Play idle by default
    actions.current["Idle"]?.play();

    // Handle key events
    const handleKeyDown = (event: KeyboardEvent) => {
      if (event.code === "KeyW") keys.current.forward = true;
      if (event.code === "KeyS") keys.current.backward = true;
      if (event.code === "KeyA") keys.current.left = true;
      if (event.code === "KeyD") keys.current.right = true;
      if (event.code === "Space") keys.current.jump = true;
      if (event.code === "ShiftLeft") keys.current.shift = true;
    };

    const handleKeyUp = (event: KeyboardEvent) => {
      if (event.code === "KeyW") keys.current.forward = false;
      if (event.code === "KeyS") keys.current.backward = false;
      if (event.code === "KeyA") keys.current.left = false;
      if (event.code === "KeyD") keys.current.right = false;
      if (event.code === "Space") keys.current.jump = false;
      if (event.code === "ShiftLeft") keys.current.shift = false;
    };

    window.addEventListener("keydown", handleKeyDown);
    window.addEventListener("keyup", handleKeyUp);

    return () => {
      window.removeEventListener("keydown", handleKeyDown);
      window.removeEventListener("keyup", handleKeyUp);
    };
  }, [animations, mixer]);

  useFrame((_, delta) => {
    mixer.update(delta);

    const body = rapier?.getRigidBodyHandle(characterRef.current?.uuid ?? "");
    if (!body) return;

    const moveSpeed = keys.current.shift ? 8 : 4; // Run or walk
    velocity.current.set(0, 0, 0);

    if (keys.current.forward) velocity.current.z -= moveSpeed;
    if (keys.current.backward) velocity.current.z += moveSpeed;
    if (keys.current.left) velocity.current.x -= moveSpeed;
    if (keys.current.right) velocity.current.x += moveSpeed;

    rapier.setBodyVelocity(
      body,
      new rapier.Vector3(velocity.current.x, velocity.current.y, velocity.current.z)
    );

    // Play animations
    if (keys.current.forward || keys.current.backward || keys.current.left || keys.current.right) {
      if (keys.current.shift && actions.current["Run"]) {
        actions.current["Run"].play();
      } else if (actions.current["Walk"]) {
        actions.current["Walk"].play();
      }
    } else {
      actions.current["Idle"]?.play();
    }

    if (keys.current.jump && canJump) {
      rapier.setBodyVelocity(body, new rapier.Vector3(0, 5, 0));
      setCanJump(false);
      actions.current["Jump"]?.play();
    }
  });

  return (
    <RigidBody
      ref={characterRef}
      type="dynamic"
      position={[0, 1, 0]}
      colliders="capsule"
      onCollisionEnter={(event) => {
        if (event.colliderName === "ground") {
          setCanJump(true);
        }
      }}
    >
      <primitive object={scene} scale={0.5} />
    </RigidBody>
  );
};

const App = () => {
  return (
    <Canvas shadows camera={{ position: [5, 5, 5], fov: 50 }}>
      <ambientLight intensity={0.5} />
      <directionalLight position={[10, 10, 10]} intensity={1} castShadow />
      <Physics>
        {/* Ground */}
        <RigidBody type="fixed" colliders="cuboid" name="ground">
          <mesh receiveShadow>
            <boxGeometry args={[100, 1, 100]} />
            <meshStandardMaterial color="gray" />
          </mesh>
        </RigidBody>

        {/* Character */}
        <Character />
      </Physics>
      <OrbitControls />
    </Canvas>
  );
};

export default App;
```

---

### Key Features

1. **Physics Integration**:
    
    - `react-three-rapier` is used for handling physics with `RigidBody`.
2. **Animation Management**:
    
    - Animations are controlled using `THREE.AnimationMixer`.
    - Animations such as "Idle," "Walk," "Run," and "Jump" are conditionally played based on input.
3. **Keyboard Controls**:
    
    - Keyboard events manage movement and actions (`WASD`, `Shift`, `Space`).
4. **Collision Handling**:
    
    - Collision detection is implemented for jump logic to check when the character is grounded.
5. **Dynamic Character Movement**:
    
    - Movement speed and animations adapt based on user input (walking vs. running).
6. **Scene Setup**:
    
    - Includes lighting and ground plane.

---
