
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

``` glsl
const fragmentShader = `
    uniform vec3 uBaseColor;       // Base metallic color
    uniform float uRoughness;      // Controls roughness of the metal
    uniform float uReflectivity;  // Reflectivity of the material
    uniform vec3 uLightPosition;  // Light position in world space
    uniform vec3 uLightColor;     // Light color

    varying vec3 vNormal;
    varying vec3 vViewPosition;

    float rand(vec2 co) {
        return fract(sin(dot(co.xy, vec2(12.9898,78.233))) * 43758.5453);
    }

    float noise(vec3 p) {
        vec3 i = floor(p);
        vec3 f = fract(p);
        f = f * f * (3.0 - 2.0 * f);
        return mix(
            mix(
                mix(rand(i.xy), rand(i.xy + vec2(1.0, 0.0)), f.x),
                mix(rand(i.xy + vec2(0.0, 1.0)), rand(i.xy + vec2(1.0, 1.0)), f.x),
                f.y
            ),
            mix(
                mix(rand(i.xy + vec2(0.0, 0.0) + vec3(0.0, 0.0, 1.0).xy), rand(i.xy + vec2(1.0, 0.0) + vec3(0.0, 0.0, 1.0).xy), f.x),
                mix(rand(i.xy + vec2(0.0, 1.0) + vec3(0.0, 0.0, 1.0).xy), rand(i.xy + vec2(1.0, 1.0) + vec3(0.0, 0.0, 1.0).xy), f.x),
                f.y
            ),
            f.z
        );
    }

    void main() {
        // Normalize the normal vector
        vec3 normal = normalize(vNormal);

        // Compute the light direction
        vec3 lightDir = normalize(uLightPosition - vViewPosition);

        // Compute the view direction
        vec3 viewDir = normalize(-vViewPosition);

        // Compute the reflection direction
        vec3 reflectDir = reflect(-lightDir, normal);

        // Procedural noise for surface details
        float metallicNoise = noise(normal * 5.0 + vViewPosition * 0.1);

        // Base metallic color with noise
        vec3 metallicColor = uBaseColor + metallicNoise * 0.1;

        // Diffuse lighting
        float diff = max(dot(normal, lightDir), 0.0);

        // Specular highlight
        float spec = pow(max(dot(viewDir, reflectDir), 0.0), 1.0 / uRoughness) * uReflectivity;

        // Final color
        vec3 color = metallicColor * diff + uLightColor * spec;

        gl_FragColor = vec4(color, 1.0);
    }
`;
```

#### Shader Material

``` js
const metallicMaterial = new THREE.ShaderMaterial({
    vertexShader,
    fragmentShader,
    uniforms: {
        uBaseColor: { value: new THREE.Color(0.8, 0.8, 0.8) }, // Base metallic color
        uRoughness: { value: 0.5 }, // Roughness (lower = shinier)
        uReflectivity: { value: 0.9 }, // Reflectivity
        uLightPosition: { value: new THREE.Vector3(10, 10, 10) }, // Light position
        uLightColor: { value: new THREE.Color(1, 1, 1) }, // Light color
    },
});
```

