
#### Vertex shader

```
const vertexShader = `
        varying vec3 vNormal;
        varying vec3 vPosition;
        varying vec2 vUv;

        uniform bool uDrawOutline;
        uniform float uOutlineWidth;

        void main() {
            vNormal = normalize(normalMatrix * normal);
            vPosition = (modelViewMatrix * vec4(position, 1.0)).xyz;
            vUv = uv;

            if (uDrawOutline) {
                vec4 viewPosition = modelViewMatrix * vec4(position, 1.0);
                vec4 offsetPosition = viewPosition + normalize(vPosition) * uOutlineWidth;
                gl_Position = projectionMatrix * offsetPosition;
            } else {
                gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
            }
        }
    `
```

#### Fragment shader

```
const fradmentShader = `
        uniform vec3 uToonColor;
        uniform vec3 uOutlineColor;
        uniform bool uDrawOutline;
        uniform vec3 uLightPosition;
        uniform vec3 uCameraPosition;

        varying vec3 vNormal;
        varying vec3 vPosition;
        varying vec2 vUv;

        vec3 phongShading(vec3 normal, vec3 lightDir, vec3 viewDir) {
            float diffuse = max(dot(normal, lightDir), 0.0);
            float specular = pow(max(dot(reflect(-lightDir, normal), viewDir), 0.0), 32.0);
            return uToonColor * diffuse + specular;
        }

        void main() {
            vec3 lightDir = normalize(uLightPosition - vPosition);
            vec3 viewDir = normalize(uCameraPosition - vPosition);
            vec3 normal = normalize(vNormal);
            vec3 color = phongShading(normal, lightDir, viewDir);

            if (uDrawOutline) {
                gl_FragColor = vec4(uOutlineColor, 1.0);
            } else {
                gl_FragColor = vec4(color, 1.0);
            }
        }
    `,
```

#### Create the shader material for both toon shading and outline

```js
const toonOutlineMaterial = new THREE.ShaderMaterial({
    vertexShader,
    fragmentShader,
    uniforms: {
        uToonColor: { value: new THREE.Color(1.0, 0.5, 0.0) },  // Color 
        uOutlineColor: { value: new THREE.Color(0.0, 0.0, 0.0) },  // Default outline color (black)
        uOutlineWidth: { value: 0.1 },  // Outline width
        uDrawOutline: { value: true },   // Toggle outline
        uLightPosition: { value: new THREE.Vector3(10, 10, 10) }, // Light position
        uCameraPosition: { value: new THREE.Vector3(0, 0, 10) }, // Camera position
    },
    side: THREE.FrontSide,  // Use front side for toon shading
    transparent: true
});
```
