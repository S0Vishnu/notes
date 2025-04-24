
 Imagine we’re building a magical 3D world where you can look around and play with a ball floating in space. I’ll show you step-by-step how to make that happen with a special tool called **Three.js**. It's like a magic wand for creating cool 3D things on a screen.

### **Step 1: We Need Some Tools**

Before we start, we need to set up our magic workshop.

1. **Install Node.js**  
    Go to [Node.js](https://nodejs.org/) and download it. It’s like installing a box of magic tools on your computer.
    
2. **Open Your Terminal**  
    A terminal is like talking to your computer using words. Open it up (don’t worry, it’s friendly).
    

---

### **Step 2: Make a New Project**

Let’s make a new folder where all your magic stuff will go:

1. In the terminal, type this:
    
    ```bash
    mkdir my-magic-sphere
    cd my-magic-sphere
    ```
    
    Now you’re inside your new magic folder!
    
2. Tell the computer to create a file to remember all your tools:
    
    ```bash
    npm init -y
    ```
    
    Boom! You just made a file called `package.json`.
    

---

### **Step 3: Add the Magic Wand (Three.js)**

We’re going to install **Three.js**, our 3D magic tool:

1. Type this in the terminal:
    
    ```bash
    npm install three
    ```
    
    Now your computer has learned how to use Three.js!

---

### **Step 4: Make the Magic Paper (HTML File)**

#### Step 1: **Set Up the Magic (Three.js Basics)**

First, we need to make sure our wand works. For that, you’ll need a special file called **three.js**. Let’s say we already have that ready.

---

#### Step 2: **Create the Canvas (Your Magic Paper)**

We’ll need a place to draw, like a paper for painting. Here’s how we tell the computer to make a canvas:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>3D Sphere</title>
</head>
<body>
  <script type="module" src="main.js"></script>
</body>
</html>
```

---

#### Step 3: **Set Up the Scene (Our Playground)**

Think of a scene as your playground where everything happens (main.js).

```javascript
    import * as THREE from 'three'; 
    import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
    const scene = new THREE.Scene(); // This is our magical world!
```

---

#### Step 4: **Create the Camera (Your Eyes)**

The camera lets us see the magic. We’ll place it a little far from the ball so we can see everything.

```javascript
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 5; // Move the camera back so we can see the ball
```

---

#### Step 5: **Set Up the Renderer (Paint the Scene)**

The renderer is the one who does all the painting on our magic canvas.

```javascript
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight); // Make it full screen
    document.body.appendChild(renderer.domElement); // Attach it to our page
```

---

#### Step 6: **Make the Sphere (The Ball!)**

Let’s make a round ball using a sphere geometry and give it a color.

```javascript
    const geometry = new THREE.SphereGeometry(1, 32, 32); // This makes the ball shape
    const material = new THREE.MeshBasicMaterial({ color: 0x0077ff }); // This gives it color
    const sphere = new THREE.Mesh(geometry, material); // Combine shape and color
    scene.add(sphere); // Add the ball to the playground
```

---

#### Step 7: **Add OrbitControls (Move Around the Ball!)**

Now let’s add something super cool: the ability to look around the ball by dragging with the mouse!

```javascript
    const controls = new THREE.OrbitControls(camera, renderer.domElement);
```

---

#### Step 8: **Make It Move (The Magic Loop)**

We need a loop that keeps updating the screen, so the magic never stops!

```javascript
    function animate() {
      requestAnimationFrame(animate); // Call this function over and over
      controls.update(); // Update the controls
      renderer.render(scene, camera); // Paint the scene and camera
    }
    animate(); // Start the loop
```

---
### **Step 5: Start the Magic Show**

We need a helper to show our magic, so we’ll use **vite**, a friendly helper.

1. Install vite:
    
    ```bash
    npm install vite
    ```
    
2. Tell the computer how to start the show. Open `package.json` and add this:
    
    ```json
    "scripts": {
      "dev": "vite"
    }
    ```
    
3. Run this in the terminal:
    
    ```bash
    npm run dev
    ```
    
    The computer will give you a magic link, like `http://localhost:3000`.
    
4. Open that link in your browser, and **TA-DAAA**! There’s your shiny blue ball!

#### And That’s It! 🎉

When you run this in your browser, you’ll see a blue ball floating in the middle of the screen. You can drag your mouse to look around it like a real explorer!

### What Next?

Now you can spin the ball, change its color, or even add stars! What do you think, little one? Want to make the ball fly? 😊

### **Complete code**

1. Create another file called `main.js`.  
    This is where the real magic happens!
2. Write this inside `main.js`:

    ```javascript
    import * as THREE from 'three';
    import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
    
    const scene = new THREE.Scene(); // Make your magic playground
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 5;
    
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement); // Show the magic on your paper
    
    const geometry = new THREE.SphereGeometry(1, 32, 32); // Create the ball shape
    const material = new THREE.MeshBasicMaterial({ color: 0x0077ff }); // Give it a color
    const sphere = new THREE.Mesh(geometry, material); // Combine shape and color
    scene.add(sphere); // Add it to the playground
    
    const controls = new OrbitControls(camera, renderer.domElement); // Let us look around!
    
    function animate() {
      requestAnimationFrame(animate); // Keep the magic moving
      controls.update(); // Update the looking-around tool
      renderer.render(scene, camera); // Show the magic
    }
    animate(); // Start the magic loop
    ```

### **Make the Ball Spin?**

To make the ball spin, we need to change the code just a little bit. In the `animate()` function (that’s the magic loop), we will add some lines to make the ball spin around!

Here’s how you do it:

1. Open the `main.js` file.
    
2. Find the part where it says `function animate()`.
    
3. Inside the `animate()` function, right before `renderer.render(scene, camera);`, add this line:
```
    sphere.rotation.x += 0.01; // Spins the ball around the X-axis 
    sphere.rotation.y += 0.01; // Spins the ball around the Y-axis
```

This will make the ball spin slowly, both up and down (the X-axis) and side to side (the Y-axis).
