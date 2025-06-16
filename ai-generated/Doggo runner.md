

``` html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Doggo Runner - 3D Infinite Runner Game</title>

    <script src="https://cdn.tailwindcss.com"></script>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>

    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/GLTFLoader.js"></script>

    <style>

        body {

            margin: 0;

            overflow: hidden;

            font-family: 'Arial', sans-serif;

        }

        #game-container {

            position: relative;

            width: 100%;

            height: 100vh;

        }

        #ui-container {

            position: absolute;

            top: 0;

            left: 0;

            width: 100%;

            height: 100%;

            pointer-events: none;

            z-index: 10;

        }

        #score-display {

            position: absolute;

            top: 20px;

            left: 20px;

            color: white;

            font-size: 24px;

            font-weight: bold;

            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);

        }

        #game-over {

            position: absolute;

            top: 50%;

            left: 50%;

            transform: translate(-50%, -50%);

            background-color: rgba(0, 0, 0, 0.8);

            color: white;

            padding: 20px;

            border-radius: 10px;

            text-align: center;

            display: none;

        }

        #start-screen {

            position: absolute;

            top: 0;

            left: 0;

            width: 100%;

            height: 100%;

            background-color: rgba(0, 0, 0, 0.7);

            display: flex;

            flex-direction: column;

            justify-content: center;

            align-items: center;

            color: white;

            z-index: 20;

        }

        .jump-btn {

            position: absolute;

            bottom: 20px;

            right: 20px;

            width: 80px;

            height: 80px;

            background-color: rgba(255, 255, 255, 0.2);

            border-radius: 50%;

            display: flex;

            justify-content: center;

            align-items: center;

            pointer-events: auto;

            cursor: pointer;

            backdrop-filter: blur(5px);

        }

        .jump-btn span {

            font-size: 14px;

            color: white;

            text-transform: uppercase;

            letter-spacing: 1px;

        }

        .obstacle {

            position: absolute;

            width: 50px;

            height: 50px;

            background-color: red;

            border-radius: 5px;

        }

        #ground {

            position: absolute;

            bottom: -100px;

            left: 0;

            width: 200%;

            height: 100px;

            background-color: #5E3811;

        }

        @keyframes dogRun {

            0%, 100% { transform: translateY(0); }

            50% { transform: translateY(-5px); }

        }

        .animate-run {

            animation: dogRun 0.5s infinite;

        }

    </style>

</head>

<body class="bg-blue-100">

    <div id="game-container">

        <div id="ui-container">

            <div id="score-display" class="text-white text-2xl font-bold">Score: 0</div>

            <div class="jump-btn" id="jump-btn"><span>Jump</span></div>

            <div id="game-over" class="bg-black bg-opacity-80 text-white p-6 rounded-lg">

                <h2 class="text-3xl font-bold mb-4">Game Over!</h2>

                <p id="final-score" class="text-xl mb-6">Your score: 0</p>

                <button id="restart-btn" class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-6 rounded pointer-events-auto">

                    Play Again

                </button>

            </div>

            <div id="start-screen" class="bg-black bg-opacity-70 flex flex-col justify-center items-center">

                <h1 class="text-4xl md:text-6xl font-bold mb-4 text-yellow-400">Doggo Runner</h1>

                <p class="text-xl mb-8 text-center px-4">Help the dog avoid obstacles and run as far as you can!</p>

                <button id="start-btn" class="bg-green-500 hover:bg-green-600 text-white font-bold py-3 px-8 rounded-full text-xl pointer-events-auto">

                    Start Game

                </button>

                <div class="mt-8 text-sm text-gray-300">

                    <p>Tap or click to jump over obstacles</p>

                    <p class="mt-2">Use keyboard arrow keys (← →) to move sideways</p>

                </div>

            </div>

        </div>

    </div>

  

    <script>

        // Game variables

        let score = 0;

        let gameSpeed = 5;

        let isGameOver = false;

        let gameStarted = false;

        let jumping = false;

        let moveLeft = false;

        let moveRight = false;

        let dogPosition = { x: 0, y: 0, z: 0 };

        // Three.js variables

        let scene, camera, renderer, dog, obstacles = [], ground;

        let speedLines, speedLineParticles = 200;

        // DOM elements

        const scoreDisplay = document.getElementById('score-display');

        const gameOverScreen = document.getElementById('game-over');

        const finalScoreDisplay = document.getElementById('final-score');

        const restartBtn = document.getElementById('restart-btn');

        const startScreen = document.getElementById('start-screen');

        const startBtn = document.getElementById('start-btn');

        const jumpBtn = document.getElementById('jump-btn');

        // Initialize the game

        function init() {

            // Set up Three.js scene with skybox

            scene = new THREE.Scene();

            const loader = new THREE.CubeTextureLoader();

            scene.background = loader.load([

                'https://threejs.org/examples/textures/cube/skybox/px.jpg',

                'https://threejs.org/examples/textures/cube/skybox/nx.jpg',

                'https://threejs.org/examples/textures/cube/skybox/py.jpg',

                'https://threejs.org/examples/textures/cube/skybox/ny.jpg',

                'https://threejs.org/examples/textures/cube/skybox/pz.jpg',

                'https://threejs.org/examples/textures/cube/skybox/nz.jpg'

            ]);

            // Create speed lines

            const lineGeometry = new THREE.BufferGeometry();

            const positions = new Float32Array(speedLineParticles * 3);

            const sizes = new Float32Array(speedLineParticles);

            for (let i = 0; i < speedLineParticles; i++) {

                // Position lines randomly at the screen edges

                const side = Math.random() > 0.5 ? 1 : -1;

                const posX = side * (2 + Math.random());

                const posY = Math.random() * 4 - 2;

                const posZ = -5 - Math.random() * 5;

                positions[i * 3] = posX;

                positions[i * 3 + 1] = posY;

                positions[i * 3 + 2] = posZ;

                sizes[i] = Math.random() * 0.3 + 0.1;

            }

            lineGeometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));

            lineGeometry.setAttribute('size', new THREE.BufferAttribute(sizes, 1));

            const lineMaterial = new THREE.PointsMaterial({

                color: 0xffffff,

                size: 0.05,

                transparent: true,

                opacity: 0.6,

                sizeAttenuation: true

            });

            speedLines = new THREE.Points(lineGeometry, lineMaterial);

            scene.add(speedLines);

  

            // Set up camera

            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

            camera.position.set(0, 3, 5);

            camera.lookAt(0, 0, 0);

            // Set up renderer

            renderer = new THREE.WebGLRenderer({ antialias: true });

            renderer.setSize(window.innerWidth, window.innerHeight);

            renderer.shadowMap.enabled = true;

            document.getElementById('game-container').appendChild(renderer.domElement);

            // Add lights

            const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);

            scene.add(ambientLight);

            const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);

            directionalLight.position.set(1, 1, 1);

            directionalLight.castShadow = true;

            directionalLight.shadow.mapSize.width = 1024;

            directionalLight.shadow.mapSize.height = 1024;

            scene.add(directionalLight);

            // Create textured terrain

            const groundTexture = new THREE.TextureLoader().load('https://threejs.org/examples/textures/terrain/grasslight-big.jpg');

            groundTexture.wrapS = groundTexture.wrapT = THREE.RepeatWrapping;

            groundTexture.repeat.set(25, 25);

            const groundGeometry = new THREE.PlaneGeometry(100, 50);

            const groundMaterial = new THREE.MeshStandardMaterial({

                map: groundTexture,

                roughness: 0.9,

                metalness: 0.1

            });

            ground = new THREE.Mesh(groundGeometry, groundMaterial);

            ground.rotation.x = -Math.PI / 2;

            ground.receiveShadow = true;

            scene.add(ground);

  

            // Add some grass details

            const grassTexture = new THREE.TextureLoader().load('https://threejs.org/examples/models/gltf/Parrot.glb'); // Just a placeholder

            for (let i = 0; i < 100; i++) {

                const grass = new THREE.Mesh(

                    new THREE.PlaneGeometry(0.5, 0.5),

                    new THREE.MeshStandardMaterial({

                        color: 0x4CAF50,

                        side: THREE.DoubleSide,

                        transparent: true,

                        opacity: 0.8

                    })

                );

                grass.position.set(

                    Math.random() * 80 - 40,

                    0.1,

                    Math.random() * 40 - 20

                );

                grass.rotation.x = -Math.PI / 2;

                grass.rotation.z = Math.random() * Math.PI;

                scene.add(grass);

            }

            // Load dog model (simple example - would use glTF in production)

            const dogGeometry = new THREE.ConeGeometry(0.25, 1, 4); // Temporary stand-in

            const dogMaterial = new THREE.MeshStandardMaterial({

                color: 0xF4A460,

                roughness: 0.3,

                metalness: 0.2

            });

            dog = new THREE.Mesh(dogGeometry, dogMaterial);

            dog.castShadow = true;

            dog.position.y = 0.5;

            dog.rotation.y = Math.PI;

            const dogBody = new THREE.Mesh(

                new THREE.BoxGeometry(0.6, 0.4, 0.8),

                dogMaterial

            );

            dogBody.position.set(0, -0.3, -0.4);

            const legGeometry = new THREE.CylinderGeometry(0.05, 0.05, 0.3);

            const legs = [];

            [ [-0.2, 0, -0.2], [0.2, 0, -0.2], [-0.2, 0, -0.6], [0.2, 0, -0.6] ].forEach((pos) => {

                const leg = new THREE.Mesh(legGeometry, dogMaterial);

                leg.position.set(pos[0], -0.5, pos[2]);

                legs.push(leg);

            });

            const dogGroup = new THREE.Group();

            dogGroup.add(dog);

            dogGroup.add(dogBody);

            legs.forEach(leg => dogGroup.add(leg));

            scene.add(dogGroup);

            dog = dogGroup; // Update reference

            // Add wagging tail animation

            const tail = new THREE.Mesh(

                new THREE.ConeGeometry(0.04, 0.3, 8),

                dogMaterial

            );

            tail.position.set(0, -0.2, -0.8);

            tail.rotation.x = Math.PI / 4;

            dog.add(tail);

            // Event listeners

            startBtn.addEventListener('click', startGame);

            restartBtn.addEventListener('click', restartGame);

            jumpBtn.addEventListener('click', jump);

            document.addEventListener('keydown', (e) => {

                if (!gameStarted) return;

                if (e.code === 'Space' || e.key === ' ' || e.key === 'ArrowUp') {

                    jump();

                } else if (e.key === 'ArrowLeft') {

                    moveLeft = true;

                } else if (e.key === 'ArrowRight') {

                    moveRight = true;

                }

            });

            document.addEventListener('keyup', (e) => {

                if (e.key === 'ArrowLeft') {

                    moveLeft = false;

                } else if (e.key === 'ArrowRight') {

                    moveRight = false;

                }

            });

            window.addEventListener('resize', () => {

                camera.aspect = window.innerWidth / window.innerHeight;

                camera.updateProjectionMatrix();

                renderer.setSize(window.innerWidth, window.innerHeight);

            });

            // Start the game loop

            animate();

        }

        // Game loop

        function animate() {

            requestAnimationFrame(animate);

            if (!gameStarted || isGameOver) return;

            // Move obstacles

            obstacles.forEach((obstacle, index) => {

                obstacle.position.z += gameSpeed * 0.05;

                // Remove obstacles that are behind the camera

                if (obstacle.position.z > 5) {

                    scene.remove(obstacle);

                    obstacles.splice(index, 1);

                    score++;

                    scoreDisplay.textContent = `Score: ${score}`;

                    // Increase game speed every 10 points

                    if (score % 10 === 0) {

                        gameSpeed += 0.5;

                    }

                }

                // Check for collisions

                if (checkCollision(dog, obstacle)) {

                    gameOver();

                }

            });

            // Horizontal movement

            if (moveLeft && dog.position.x > -2) {

                dog.position.x -= 0.1;

            }

            if (moveRight && dog.position.x < 2) {

                dog.position.x += 0.1;

            }

            // Jumping

            if (jumping) {

                if (dogJumpHeight < 1) {

                    dogJumpHeight += 0.05;

                    dog.position.y = dogJumpHeight;

                } else {

                    jumping = false;

                    falling = true;

                }

            } else if (falling) {

                if (dogJumpHeight > 0.25) {

                    dogJumpHeight -= 0.05;

                    dog.position.y = dogJumpHeight;

                } else {

                    falling = false;

                }

            }

            // Add new obstacles randomly

            if (Math.random() < 0.02) {

                addObstacle();

            }

            // Update speed lines

            if (gameStarted && !isGameOver) {

                const positions = speedLines.geometry.attributes.position.array;

                for (let i = 0; i < speedLineParticles; i++) {

                    // Move lines forward

                    positions[i * 3 + 2] += gameSpeed * 0.1;

                    // Reset lines that go behind the camera

                    if (positions[i * 3 + 2] > 5) {

                        const side = Math.random() > 0.5 ? 1 : -1;

                        positions[i * 3] = side * (2 + Math.random());

                        positions[i * 3 + 1] = Math.random() * 4 - 2;

                        positions[i * 3 + 2] = -5 - Math.random() * 5;

                    }

                }

                speedLines.geometry.attributes.position.needsUpdate = true;

                // Make lines more visible at higher speeds

                speedLines.material.opacity = Math.min(0.6, 0.4 + gameSpeed * 0.02);

            }

            renderer.render(scene, camera);

        }

        // Jump variables

        let dogJumpHeight = 0.25;

        let falling = false;

        function jump() {

            if (!jumping && !falling && gameStarted && !isGameOver) {

                jumping = true;

            }

        }

        function addObstacle() {

            const obstacleTypes = [

                { // Tree

                    parts: [

                        { geom: new THREE.CylinderGeometry(0.2, 0.2, 0.5),

                          mat: new THREE.MeshStandardMaterial({ color: 0x8B4513 }),

                          pos: [0, 0.25, 0] },

                        { geom: new THREE.ConeGeometry(0.6, 1.5, 5),

                          mat: new THREE.MeshStandardMaterial({ color: 0x228B22 }),

                          pos: [0, 1.25, 0] }

                    ]

                },

                { // Fence

                    parts: [

                        { geom: new THREE.BoxGeometry(0.8, 0.1, 0.1),

                          mat: new THREE.MeshStandardMaterial({ color: 0x8B4513 }),

                          pos: [0, 0.3, 0] },

                        { geom: new THREE.BoxGeometry(0.05, 0.6, 0.05),

                          mat: new THREE.MeshStandardMaterial({ color: 0x8B4513 }),

                          pos: [0.35, 0, 0] },

                        { geom: new THREE.BoxGeometry(0.05, 0.6, 0.05),

                          mat: new THREE.MeshStandardMaterial({ color: 0x8B4513 }),

                          pos: [-0.35, 0, 0] }

                    ]

                },

                { // Rock

                    parts: [

                        { geom: new THREE.SphereGeometry(0.4, 6, 6),

                          mat: new THREE.MeshStandardMaterial({ color: 0x777777 }),

                          pos: [0, 0.4, 0] }

                    ]

                }

            ];

  

            const type = Math.floor(Math.random() * obstacleTypes.length);

            const obstacle = new THREE.Group();

            obstacleTypes[type].parts.forEach(part => {

                const mesh = new THREE.Mesh(part.geom, part.mat);

                mesh.position.set(part.pos[0], part.pos[1], part.pos[2]);

                mesh.castShadow = true;

                obstacle.add(mesh);

            });

  

            obstacle.position.set(

                Math.random() * 4 - 2,

                0,

                -10

            );

  

            scene.add(obstacle);

            obstacles.push(obstacle);

        }

        function checkCollision(obj1, obj2) {

            // Improved collision detection with bounding spheres

            obj1.updateMatrixWorld();

            obj2.updateMatrixWorld();

            const box1 = new THREE.Box3().setFromObject(obj1);

            const box2 = new THREE.Box3().setFromObject(obj2);

            return box1.intersectsBox(box2) && !(jumping || falling);

        }

        function gameOver() {

            isGameOver = true;

            finalScoreDisplay.textContent = `Your score: ${score}`;

            gameOverScreen.style.display = 'block';

        }

        function startGame() {

            startScreen.style.display = 'none';

            gameStarted = true;

            isGameOver = false;

            score = 0;

            gameSpeed = 5;

            scoreDisplay.textContent = `Score: ${score}`;

            // Clear existing obstacles

            obstacles.forEach(obstacle => scene.remove(obstacle));

            obstacles = [];

            // Reset dog position

            dog.position.set(0, 0.25, 0);

        }

        function restartGame() {

            gameOverScreen.style.display = 'none';

            gameStarted = false;

            startGame();

        }

        // Start the game

        init();

    </script>

</body>

</html>


```