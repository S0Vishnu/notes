
#### Vertex Shader

``` glsl
const vertexShader = `
    uniform float uTime;
    uniform float uWaveSpeed;
    uniform float uWaveAmplitude;
    uniform float uWaveFrequency;
    uniform vec3 uObjectPositions[10]; // Array of up to 10 interacting objects
    uniform int uObjectCount;         // Number of interacting objects
    uniform float uRippleStrength;

    varying vec2 vUv;
    varying vec3 vPosition;

    void main() {
        vUv = uv;
        vPosition = position;

        // Base wave displacement
        float wave = sin(position.x * uWaveFrequency + uTime * uWaveSpeed) * uWaveAmplitude +
                     cos(position.z * uWaveFrequency + uTime * uWaveSpeed) * uWaveAmplitude;

        // Ripple effect from all objects
        float ripple = 0.0;
        for (int i = 0; i < 10; i++) {
            if (i >= uObjectCount) break; // Use only active objects
            float distanceToObject = distance(position.xz, uObjectPositions[i].xz);
            ripple += exp(-distanceToObject * uRippleStrength) * sin(distanceToObject * 10.0 - uTime * 5.0);
        }

        // Combine wave and ripple for displacement
        float displacement = wave + ripple;

        vec3 displacedPosition = position + normal * displacement;
        gl_Position = projectionMatrix * modelViewMatrix * vec4(displacedPosition, 1.0);
    }
`;
```

#### Fragment Shader

```glsl
const fragmentShader = `
    uniform vec3 uWaterColor;
    uniform vec3 uFoamColor;
    uniform float uTime;

    varying vec2 vUv;
    varying vec3 vPosition;

    void main() {
        // Basic wave pattern for diffuse lighting
        float wavePattern = 0.5 + 0.5 * sin(vPosition.y * 10.0 + uTime * 5.0);

        // Foam effect near crests (high y-values)
        float foam = smoothstep(0.7, 1.0, wavePattern);

        // Combine water and foam colors
        vec3 color = mix(uWaterColor, uFoamColor, foam);

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
        uTime: { value: 0.0 },                  // Animation time
        uWaveSpeed: { value: 1.0 },            // Speed of the waves
        uWaveAmplitude: { value: 0.5 },        // Amplitude of the waves
        uWaveFrequency: { value: 0.5 },        // Frequency of the waves
        uRippleStrength: { value: 5.0 },       // Ripple propagation strength
        uObjectPositions: { value: Array(10).fill(new THREE.Vector3()) }, // Object positions
        uObjectCount: { value: 0 },            // Number of active objects
        uWaterColor: { value: new THREE.Color(0.2, 0.4, 0.8) }, // Water color
        uFoamColor: { value: new THREE.Color(1.0, 1.0, 1.0) },   // Foam color
    },
    transparent: true,
});
```

#### Interacting Objects eg.,

```js
const objects = [];
for (let i = 0; i < 3; i++) {
    const objectGeometry = new THREE.SphereGeometry(0.3);
    const objectMaterial = new THREE.MeshStandardMaterial({ color: Math.random() * 0xffffff });
    const object = new THREE.Mesh(objectGeometry, objectMaterial);
    object.position.set(Math.random() * 20 - 10, 0.3, Math.random() * 20 - 10);
    objects.push(object);
    scene.add(object);
}

// Animation Loop
let clock = new THREE.Clock();
function animate() {
    const delta = clock.getDelta();
    waterMaterial.uniforms.uTime.value += delta;

    // Update object positions in the shader
    const positions = waterMaterial.uniforms.uObjectPositions.value;
    objects.forEach((obj, index) => {
        obj.position.x += Math.sin(clock.elapsedTime + index) * delta;
        obj.position.z += Math.cos(clock.elapsedTime + index) * delta;
        positions[index].copy(obj.position);
    });
    waterMaterial.uniforms.uObjectCount.value = objects.length;

    renderer.render(scene, camera);
    requestAnimationFrame(animate);
}
animate();

```


