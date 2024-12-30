
#### Vertex Shader

``` glsl
const vertexShader = `
    varying vec3 vNormal;
    varying vec3 vViewPosition;

    void main() {
        // Compute the normal vector
        vNormal = normalize(normalMatrix * normal);

        // Compute the view position
        vec4 mvPosition = modelViewMatrix * vec4(position, 1.0);
        vViewPosition = -mvPosition.xyz;

        gl_Position = projectionMatrix * mvPosition;
    }
`;
```

#### Fragment Shader

```glsl
const fragmentShader = `
    uniform vec3 uLightPosition;   // Light position in world space
    uniform vec3 uLightColor;      // Light color
    uniform vec3 uBaseColor;       // Base object color
    uniform int uSteps;            // Number of shading steps

    varying vec3 vNormal;
    varying vec3 vViewPosition;

    void main() {
        // Normalize the normal vector
        vec3 normal = normalize(vNormal);

        // Compute the light direction
        vec3 lightDir = normalize(uLightPosition - vViewPosition);

        // Compute the diffuse factor
        float diff = max(dot(normal, lightDir), 0.0);

        // Quantize the diffuse factor into steps for toon shading
        float stepFactor = 1.0 / float(uSteps);
        float quantized = floor(diff / stepFactor) * stepFactor;

        // Compute the final color
        vec3 color = uBaseColor * uLightColor * quantized;

        gl_FragColor = vec4(color, 1.0);
    }
`;
```

#### Shader Material

``` js
const toonMaterial = new THREE.ShaderMaterial({
    vertexShader,
    fragmentShader,
    uniforms: {
        uLightPosition: { value: new THREE.Vector3(10, 10, 10) },
        uLightColor: { value: new THREE.Color(1, 1, 1) },
        uBaseColor: { value: new THREE.Color(0.2, 0.5, 0.8) },
        uSteps: { value: 5 }, // Number of shading levels
    }
});

```