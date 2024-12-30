
#### Vertex Shader

```glsl
const vertexShader = `varying vec2 vUv;

void main() {
    vUv = uv;
    gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}`
```

#### Fragment shader

``` glsl
const fragmentShader = `uniform float uTime;               // Animation time
uniform vec3 uFireColor;           // Base fire color
uniform vec3 uSmokeColor;          // Base smoke color
uniform bool uSmokeEnabled;        // Toggle for smoke
uniform float uIntensity;          // Intensity of the fire
uniform float uParticleSpeed;      // Speed of particles
uniform float uParticleDensity;    // Density of particles
varying vec2 vUv;

// Simplex noise function (2D)
vec3 permute(vec3 x) {
    return mod(((x * 34.0) + 1.0) * x, 289.0);
}
float snoise(vec2 v) {
    const vec4 C = vec4(0.211324865405187,  // (3.0-sqrt(3.0))/6.0
                        0.366025403784439,  // 0.5*(sqrt(3.0)-1.0)
                       -0.577350269189626,  // -1.0+2.0*(3.0-sqrt(3.0))/6.0
                        0.024390243902439); // 1.0/41.0
    vec2 i  = floor(v + dot(v, C.yy));
    vec2 x0 = v -   i + dot(i, C.xx);
    vec2 i1 = (x0.x > x0.y) ? vec2(1.0, 0.0) : vec2(0.0, 1.0);
    vec4 x12 = x0.xyxy + C.xxzz;
    x12.xy -= i1;
    i = mod(i, 289.0);
    vec3 p = permute(permute(i.y + vec3(0.0, i1.y, 1.0))
                    + i.x + vec3(0.0, i1.x, 1.0));
    vec3 m = max(0.5 - vec3(dot(x0, x0), dot(x12.xy, x12.xy), dot(x12.zw, x12.zw)), 0.0);
    m = m * m;
    m = m * m;
    vec3 x = 2.0 * fract(p * C.www) - 1.0;
    vec3 h = abs(x) - 0.5;
    vec3 ox = floor(x + 0.5);
    vec3 a0 = x - ox;
    m *= 1.79284291400159 - 0.85373472095314 * (a0 * a0 + h * h);
    vec3 g;
    g.x  = a0.x  * x0.x  + h.x  * x0.y;
    g.yz = a0.yz * x12.xz + h.yz * x12.yw;
    return 130.0 * dot(m, g);
}

void main() {
    vec2 p = vUv * 2.0 - 1.0;

    // Base noise for fire
    float fireNoise = snoise(vec2(p.x * 2.0, p.y * 5.0 + uTime * 0.5));
    fireNoise = smoothstep(0.0, 1.0, fireNoise);
    float intensity = fireNoise * uIntensity;
    vec3 fireColor = mix(vec3(1.0, 0.0, 0.0), uFireColor, intensity);

    // Smoke layer (if enabled)
    vec3 smoke = vec3(0.0);
    if (uSmokeEnabled) {
        float smokeNoise = snoise(vec2(p.x * 3.0, p.y * 2.0 + uTime * 0.2));
        smokeNoise = smoothstep(0.2, 0.8, smokeNoise);
        smoke = mix(fireColor, uSmokeColor, smokeNoise * 0.5);
    }

    // Procedural sparks (particles)
    float sparkNoise = snoise(vec2(p.x * uParticleDensity, (p.y + uTime * uParticleSpeed) * uParticleDensity));
    float sparkMask = step(0.8, sparkNoise); // Threshold for sparks
    vec3 sparks = vec3(1.0, 0.8, 0.4) * sparkMask;

    // Combine fire, smoke, and sparks
    vec3 color = fireColor + smoke + sparks;
    gl_FragColor = vec4(color, 1.0 - intensity); // Add transparency to the fire edges
}`
```

#### Fire Shader Material

```js
const fireMaterial = new THREE.ShaderMaterial({
    vertexShader,
    fragmentShader,
    uniforms: {
        uTime: { value: 0.0 },
        uFireColor: { value: new THREE.Color(1.0, 0.5, 0.0) }, // Fire color
        uSmokeColor: { value: new THREE.Color(0.3, 0.3, 0.3) }, // Smoke color
        uSmokeEnabled: { value: true }, // Smoke enabled by default
        uIntensity: { value: 1.0 },    // Intensity of the fire
        uParticleSpeed: { value: 1.0 }, // Speed of the sparks
        uParticleDensity: { value: 20.0 }, // Density of the sparks
    },
    transparent: true,
    blending: THREE.AdditiveBlending,
});

//  animate as --- fireMaterial.uniforms.uTime.value += delta; ---
```

