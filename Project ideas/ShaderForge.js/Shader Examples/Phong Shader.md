
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
    uniform vec3 uLightPosition;   // Light position in world space
    uniform vec3 uLightColor;      // Light color
    uniform vec3 uAmbientColor;    // Ambient color
    uniform vec3 uDiffuseColor;    // Diffuse color
    uniform vec3 uSpecularColor;   // Specular color
    uniform float uShininess;      // Shininess factor

    varying vec3 vNormal;
    varying vec3 vViewPosition;

    void main() {
        // Normalize the normal vector
        vec3 normal = normalize(vNormal);

        // Compute the light direction
        vec3 lightDir = normalize(uLightPosition - vViewPosition);

        // Compute the view direction
        vec3 viewDir = normalize(-vViewPosition);

        // Ambient term
        vec3 ambient = uAmbientColor;

        // Diffuse term
        float diff = max(dot(normal, lightDir), 0.0);
        vec3 diffuse = uDiffuseColor * diff;

        // Specular term
        vec3 reflectDir = reflect(-lightDir, normal);
        float spec = pow(max(dot(viewDir, reflectDir), 0.0), uShininess);
        vec3 specular = uSpecularColor * spec;

        // Combine the components
        vec3 finalColor = ambient + diffuse + specular;

        gl_FragColor = vec4(finalColor, 1.0);
    }
`;
```

#### Shader Material

``` javascript
const phongMaterial = new THREE.ShaderMaterial({
    vertexShader,
    fragmentShader,
    uniforms: {
        uLightPosition: { value: new THREE.Vector3(10, 10, 10) },
        uLightColor: { value: new THREE.Color(1, 1, 1) },
        uAmbientColor: { value: new THREE.Color(0.2, 0.2, 0.2) },
        uDiffuseColor: { value: new THREE.Color(0.5, 0.5, 0.5) },
        uSpecularColor: { value: new THREE.Color(1, 1, 1) },
        uShininess: { value: 30.0 },
    }
});
```

