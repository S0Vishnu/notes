
# Version 1 with only 2 colors

#### Vertex Shader

```glsl
const vertexShader = `
    varying vec3 vPosition;

    void main() {
        // Pass the position to the fragment shader
        vPosition = position;
        gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
    }
`;
```

#### Fragment Shader

```glsl
const fragmentShader = `
    uniform float uScale;       // Controls the scale of the noise
    uniform float uDistortion; // Amount of distortion
    uniform float uDetail;     // Detail level (frequency of noise)
    uniform vec3 uBaseColor;   // Base color
    uniform vec3 uHighlightColor; // Highlight color

    varying vec3 vPosition;

    // Hash function for randomization
    float hash(vec2 p) {
        return fract(sin(dot(p, vec2(127.1, 311.7))) * 43758.5453);
    }

    // Interpolated noise function
    float noise(vec2 p) {
        vec2 i = floor(p);
        vec2 f = fract(p);
        f = f * f * (3.0 - 2.0 * f);

        float a = hash(i);
        float b = hash(i + vec2(1.0, 0.0));
        float c = hash(i + vec2(0.0, 1.0));
        float d = hash(i + vec2(1.0, 1.0));

        return mix(mix(a, b, f.x), mix(c, d, f.x), f.y);
    }

    // Fractal noise with adjustable detail
    float fbm(vec2 p) {
        float value = 0.0;
        float amplitude = 0.5;
        float frequency = 1.0;

        for (int i = 0; i < 5; i++) {
            value += amplitude * noise(p * frequency);
            frequency *= 2.0;
            amplitude *= 0.5;
        }
        return value;
    }

    void main() {
        // Apply scaling and distortion
        vec2 uv = vPosition.xy * uScale;
        uv += fbm(uv * uDistortion) * 0.5;

        // Compute noise value
        float n = fbm(uv * uDetail);

        // Interpolate between base color and highlight color
        vec3 color = mix(uBaseColor, uHighlightColor, n);

        gl_FragColor = vec4(color, 1.0);
    }
`;
```

#### Shader Material

```js
const noiseMaterial = new THREE.ShaderMaterial({
    vertexShader,
    fragmentShader,
    uniforms: {
        uScale: { value: 5.0 },           // Scale of the noise pattern
        uDistortion: { value: 1.0 },      // Distortion intensity
        uDetail: { value: 2.0 },          // Noise detail level
        uBaseColor: { value: new THREE.Color(0.2, 0.5, 0.8) }, // Base color
        uHighlightColor: { value: new THREE.Color(1.0, 1.0, 1.0) }, // Highlight color
    }
});
```

# Version 2 with multiple colors and alpha

#### Vertex Shader

```glsl
const vertexShader = `
    varying vec3 vPosition;

    void main() {
        // Pass the position to the fragment shader
        vPosition = position;
        gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
    }
`;
```

#### Fragment Shader

```glsl
const fragmentShader = `
    uniform float uScale;       // Controls the scale of the noise
    uniform float uDistortion; // Amount of distortion
    uniform float uDetail;     // Detail level (frequency of noise)
    uniform vec3 uColors[5];   // Array of colors for the noise bands
    uniform float uAlpha;      // Alpha value for transparency

    varying vec3 vPosition;

    // Hash function for randomization
    float hash(vec2 p) {
        return fract(sin(dot(p, vec2(127.1, 311.7))) * 43758.5453);
    }

    // Interpolated noise function
    float noise(vec2 p) {
        vec2 i = floor(p);
        vec2 f = fract(p);
        f = f * f * (3.0 - 2.0 * f);

        float a = hash(i);
        float b = hash(i + vec2(1.0, 0.0));
        float c = hash(i + vec2(0.0, 1.0));
        float d = hash(i + vec2(1.0, 1.0));

        return mix(mix(a, b, f.x), mix(c, d, f.x), f.y);
    }

    // Fractal noise with adjustable detail
    float fbm(vec2 p) {
        float value = 0.0;
        float amplitude = 0.5;
        float frequency = 1.0;

        for (int i = 0; i < 5; i++) {
            value += amplitude * noise(p * frequency);
            frequency *= 2.0;
            amplitude *= 0.5;
        }
        return value;
    }

    void main() {
        // Apply scaling and distortion
        vec2 uv = vPosition.xy * uScale;
        uv += fbm(uv * uDistortion) * 0.5;

        // Compute noise value
        float n = fbm(uv * uDetail);

        // Map the noise value to a color band
        vec3 color;
        if (n < 0.2) {
            color = uColors[0];
        } else if (n < 0.4) {
            color = mix(uColors[0], uColors[1], (n - 0.2) / 0.2);
        } else if (n < 0.6) {
            color = mix(uColors[1], uColors[2], (n - 0.4) / 0.2);
        } else if (n < 0.8) {
            color = mix(uColors[2], uColors[3], (n - 0.6) / 0.2);
        } else {
            color = mix(uColors[3], uColors[4], (n - 0.8) / 0.2);
        }

        gl_FragColor = vec4(color, uAlpha);
    }
`;
```

#### Shader Material

```js
const noiseMaterial = new THREE.ShaderMaterial({
    vertexShader,
    fragmentShader,
    uniforms: {
        uScale: { value: 5.0 },           // Scale of the noise pattern
        uDistortion: { value: 1.0 },      // Distortion intensity
        uDetail: { value: 2.0 },          // Noise detail level
        uColors: { value: [
            new THREE.Color(0.8, 0.5, 0.2), // Light brown
            new THREE.Color(0.6, 0.3, 0.1), // Darker brown
            new THREE.Color(0.4, 0.2, 0.05), // Very dark brown
            new THREE.Color(0.9, 0.9, 0.9), // Highlight white
            new THREE.Color(0.3, 0.3, 0.3), // Shadow grey
        ]},
        uAlpha: { value: 1.0 },           // Transparency
    },
    transparent: true,
});
```
