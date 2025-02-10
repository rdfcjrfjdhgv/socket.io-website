<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Multiplayer Game</title>
    <style>
        body { margin: 0; overflow: hidden; }
        #game { position: absolute; width: 100%; height: 100%; }
        #player-count { position: fixed; top: 10px; left: 10px; color: white; font-size: 20px; }
    </style>
</head>
<body>
    <canvas id="game"></canvas>
    <div id="player-count">Players: 0</div>

    <!-- Import Three.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <!-- Import Socket.IO -->
    <script src="/socket.io/socket.io.js"></script>

    <script>
        // Your game logic goes here
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ canvas: document.getElementById('game') });
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // Your other game code...
    </script>
</body>
</html>
Explanation:
Three.js: Loaded using the <script src="..."> tag from a CDN link (this loads the Three.js library).
Socket.IO Client: Similarly, we load Socket.IO using the <script src="..."> tag. This script should be available from the server after you've installed Socket.IO on your server side.
2. Using import Statement (For Module-Based Setup)
If you're using ES6 modules (like in a Node.js-based environment), you'll use the import statement to import modules.

2.1 Set Up the package.json
You need to set up a Node.js project and indicate that you're using ES6 modules by adding "type": "module" to the package.json file.

package.json:
json
Copy
Edit
{
  "name": "multiplayer-game",
  "version": "1.0.0",
  "description": "A multiplayer 3D game",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.17.1",
    "socket.io": "^4.3.2"
  }
}
After creating package.json, run npm install to install Express and Socket.IO packages.
2.2 Use import in JavaScript
Now, you can use the import statement to bring in modules.

server.js (for the backend):
javascript
Copy
Edit
import express from 'express';
import { Server } from 'socket.io';
import http from 'http';

const app = express();
const server = http.createServer(app);
const io = new Server(server);

app.use(express.static('public'));

io.on('connection', (socket) => {
    console.log('New player connected:', socket.id);
    socket.on('disconnect', () => {
        console.log('Player disconnected:', socket.id);
    });
});

server.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
client.js (for the frontend):
javascript
Copy
Edit
import * as THREE from 'three';
import { io } from 'socket.io-client';

const socket = io();

// Setup Three.js scene
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

const playerGeometry = new THREE.BoxGeometry();
const playerMaterial = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const player = new THREE.Mesh(playerGeometry, playerMaterial);
scene.add(player);

camera.position.z = 5;

function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
}

animate();
index.html:
html
Copy
Edit
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Multiplayer Game</title>
    <style>
        body { margin: 0; overflow: hidden; }
    </style>
</head>
<body>
    <script type="module" src="client.js"></script>
</body>
</html>
