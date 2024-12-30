
#### **Basic Shader Effects:**

- **Surface Effects:**
    - Phong shading (customizable lighting models)
    - Simple toon shading
	- Metallic/reflection effects
- **Procedural Effects:**
    - Gradient textures
    - Noise-based distortion (e.g., ripples, wavy surfaces)
    - Water or liquid effects
    - Fire

#### 2. **Simple API:**

- Expose shaders as easy-to-use modules or functions:
```javascript
import { GlowShader } from 'simpleshaderlib';  

const glowMaterial = new THREE.ShaderMaterial({
	uniforms: GlowShader.uniforms,
	vertexShader: GlowShader.vertexShader,
	fragmentShader: GlowShader.fragmentShader, 
});
```

#### 3. **Customization:**

- Uniforms with clear defaults:
    - Time (`uTime`) for animations
    - User parameters like color, intensity, distortion scale
- Focus on reusability and clear parameterization.

#### 4. **Examples and Documentation**

- **Shader Playground:** A basic webpage using Three.js to showcase your shaders with tweakable parameters.
- Ready-to-use Three.js materials (e.g., `GlowMaterial`, `ToonMaterial`) wrapping your shaders.

---

### Example Shader Material Component

```javascript
import * as THREE from 'three';

class GradientShaderMaterial extends THREE.ShaderMaterial {
    constructor(colors) {
        const vertexShader = `
            varying vec2 vUv;

            void main() {
                vUv = uv;
                gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
            }
        `;

        const fragmentShader = `
            uniform vec3 uColors[${colors.length}];
            uniform float uColorCount;
            varying vec2 vUv;

            vec3 getGradientColor(float t) {
                float scaledT = t * (uColorCount - 1.0); // Map t to the color index range
                int idx1 = int(floor(scaledT));
                int idx2 = int(ceil(scaledT));
                float mixFactor = fract(scaledT);

                vec3 color1 = uColors[idx1];
                vec3 color2 = uColors[idx2];

                return mix(color1, color2, mixFactor);
            }

            void main() {
                float t = vUv.y; // Gradient based on vertical position (vUv.y)
                vec3 color = getGradientColor(t);
                gl_FragColor = vec4(color, 1.0);
            }
        `;

        // Convert the colors to a uniform-compatible format
        const colorArray = colors.map(color => new THREE.Color(color));

        // Prepare uniforms
        const uniforms = {
            uColors: { value: colorArray },
            uColorCount: { value: colors.length },
        };

        // Call ShaderMaterial's constructor
        super({
            vertexShader,
            fragmentShader,
            uniforms,
        });
    }
}

export default GradientShaderMaterial;
```

---

### How to Use

1. **Import and Instantiate the Shader Material**:
```javascript
import GradientShaderMaterial from './GradientShaderMaterial';

// Define your gradient colors
const colors = ['#ff0000', '#00ff00', '#0000ff']; // Red -> Green -> Blue gradient
const gradientMaterial = new GradientShaderMaterial(colors);
```

2. **Apply it to an Object**:
```javascript
const geometry = new THREE.BoxGeometry(1, 1, 1); // Example geometry
const mesh = new THREE.Mesh(geometry, gradientMaterial);   
scene.add(mesh);
```

3. **Customization**:
- Update the `colors` array to define your own gradient.
- Adjust the `uv.y` mapping in the fragment shader for horizontal gradients or other effects.

---

### Notes

- The shader assumes the object has UV coordinates (`uv`).
- The gradient interpolation is linear across the `vUv.y` axis by default but can be modified to suit your needs.
- For more advanced gradients, consider adding noise or time-based animation to the `getGradientColor` logic.

**To Create it as a R3F useable** 

``` jsx
extend({ GradientShaderMaterial }); 

// R3F Component 
const GradientMaterial = ({ colors }) => {
	const uniforms = useMemo(() => { 
		const colorArray = colors.map(color => new THREE.Color(color)); 
		return {
			uColors: { value: colorArray }, 
			uColorCount: { value: colors.length }, 
		}; 
	}, [colors]); 
	return ( 
		<gradientShaderMaterial attach="material" uniforms={uniforms} /> 
	); 
}; 

export default GradientMaterial;
```

**Usage:**

``` jsx
import React from 'react';
import { Canvas } from '@react-three/fiber';
import GradientMaterial from './GradientMaterial'; // Import your custom material

const App = () => {
    return (
        <Canvas>
            <mesh>
                <boxGeometry args={[1, 1, 1]} />
                <GradientMaterial colors={['#ff0000', '#00ff00', '#0000ff']} />
            </mesh>
        </Canvas>
    );
};

export default App;
```
