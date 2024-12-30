

#### Vertex Shader

``` glsl
const vertexShader = `
    varying vec2 vUv;

    void main() {
        vUv = uv;
        gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
    }
`;
```

#### Fragment Shader

```glsl
const fragmentShader = `
    uniform float uTime;               // Time for animation
    uniform vec2 uRippleCenter;        // Center of the ripple
    uniform float uRippleStrength;     // Strength of the ripple effect
    uniform vec3 uWaterColor;          // Base water color
    uniform vec3 uHighlightColor;      // Highlight water color
    varying vec2 vUv;

    void main() {
        // Calculate distance from the ripple center
        float distance = length(vUv - uRippleCenter);

        // Ripple effect: sine wave decays with distance
        float ripple = sin(distance * 20.0 - uTime * 5.0) * exp(-distance * uRippleStrength);

        // Base water animation: moving waves
        float wave = sin(vUv.x * 10.0 + uTime * 2.0) * 0.1 +
                     cos(vUv.y * 10.0 - uTime * 2.5) * 0.1;

        // Combine wave and ripple
        float combined = wave + ripple;

        // Color blending between base and highlight
        vec3 color = mix(uWaterColor, uHighlightColor, 0.5 + combined);

        gl_FragColor = vec4(color, 1.0);
    }
`;
```

#### Shader Material

```js
const waterMaterial = new THREE.ShaderMaterial({
    vertexShader,
    fragmentShader,
    uniforms: {
        uTime: { value: 0.0 },                 // Animation time
        uRippleCenter: { value: new THREE.Vector2(0.5, 0.5) }, // Initial ripple center
        uRippleStrength: { value: 5.0 },      // Strength of the ripple effect
        uWaterColor: { value: new THREE.Color(0.2, 0.4, 0.8) }, // Water color
        uHighlightColor: { value: new THREE.Color(0.8, 0.9, 1.0) }, // Highlight color
    },
    transparent: true,
});
```

### Interaction: Ripple animation on click eg.,

``` js
function ripples(event) {
    // Convert click position to normalized device coordinates (NDC)
    const mouse = new THREE.Vector2(
        (event.clientX / window.innerWidth) * 2 - 1,
        -(event.clientY / window.innerHeight) * 2 + 1
    );

    // Raycast to find the point of intersection
    const raycaster = new THREE.Raycaster();
    raycaster.setFromCamera(mouse, camera);
    const intersects = raycaster.intersectObject(waterMesh);

    if (intersects.length > 0) {
        const point = intersects[0].point;

        // Convert world coordinates to UV coordinates
        const uv = intersects[0].uv;

        // Update ripple center in shader
        waterMaterial.uniforms.uRippleCenter.value.set(uv.x, uv.y);
    }
}

window.addEventListener('click', ripples);
```
