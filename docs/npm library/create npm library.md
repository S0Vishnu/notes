
### **Configure the Project for npm**

1. **Edit `package.json`**: Update the `main` field to point to the entry file:

```json
{
    "name": "sky-shader-material",
    "version": "1.0.0",
    "description": "A Three.js ShaderMaterial for a customizable sky gradient",
    "main": "dist/index.js",
    "module": "dist/index.js",
    "files": [
	    "dist"
    ],
    "keywords": ["three.js", "shader", "sky", "gradient", "three"],
    "author": "Your Name",
    "license": "MIT",
    "dependencies": {
	    "three": "^0.155.0"
	}
}
```

2. **Install a Bundler**: Use a bundler like Rollup or ESBuild to compile your library. For this example, let’s use Rollup:

```bash
npm install --save-dev rollup rollup-plugin-node-resolve rollup-plugin-terser
```

3. **Create a Rollup Config**: Create a file named `rollup.config.js`:

```javascript
import resolve from '@rollup/plugin-node-resolve';
import { terser } from 'rollup-plugin-terser';

export default {
    input: 'src/index.js',
	output: [
    {
      file: 'dist/index.js',
      format: 'es',
      sourcemap: true,
	},
],
plugins: [resolve(), terser()],
external: ['three'], // Don't bundle Three.js
};
```

4. **Add Build Script**: Add a build script in `package.json`:

```json
"scripts": {
"build": "rollup -c"
}
```

5. **Build the Library**: Run the build command:

```bash
npm run build
```

---

**Test the Library Locally**

1. Link the library locally for testing:

```bash
npm link
```

2. In a test project, link your library:

```bash
npm link sky-shader-material
```

3. Use it in your test project:

```javascript
import { SkyShaderMaterial } from 'sky-shader-material';
import * as THREE from 'three';

const scene = new THREE.Scene();
const skyGeometry = new THREE.SphereGeometry(5000, 32, 32);
const skyMaterial = new SkyShaderMaterial();
const skyMesh = new THREE.Mesh(skyGeometry, skyMaterial);
scene.add(skyMesh);
```


---

### 6. **Publish to npm**

1. Log in to npm:

```bash
npm login
```

2. Publish the library:

```bash
npm publish
```

3. Install your library via npm:

```bash
npm install sky-shader-material
```

