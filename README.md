# Aron-web-avatar
<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8">
<title>آرون – دستیار هوش مصنوعی</title>
<style>
  body { margin:0; background:#f0f0f0; font-family:sans-serif; }
  #canvas { width:100vw; height:80vh; background:#fff; }
  #chat { position: fixed; bottom:10px; left:10px; width:calc(100% - 20px); }
  #chat input { width:100%; padding:10px; font-size:16px; }
  #chat .log { background:#fff; max-height:200px; overflow-y:auto; padding:10px; border:1px solid #ccc; margin-bottom:5px; }
</style>
</head>
<body>
  <div id="canvas"></div>
  <div id="chat">
    <div class="log" id="log"></div>
    <input id="prompt" placeholder="برای آرون بنویس..." />
  </div>
<script type="module">
import * as THREE from 'https://cdn.skypack.dev/three';
import { GLTFLoader } from 'https://cdn.skypack.dev/three/examples/jsm/loaders/GLTFLoader.js';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(35, window.innerWidth/window.innerHeight, 0.1, 1000);
camera.position.set(0, 1.6, 2.5);
const renderer = new THREE.WebGLRenderer({ antialias:true });
renderer.setSize(window.innerWidth, window.innerHeight * 0.8);
document.getElementById('canvas').appendChild(renderer.domElement);
scene.add(new THREE.HemisphereLight(0xffffff, 0x444444, 1));

new GLTFLoader().load(
  'https://models.readyplayer.me/687d253d50cf0bbaeeedbb94.glb',
  gltf => {
    const avatar = gltf.scene;
    avatar.position.set(0, -1, 0);
    avatar.scale.set(0.6, 0.6, 0.6);
    scene.add(avatar);
    animate();
    window.avatar = avatar;
  }
);

function animate() {
  requestAnimationFrame(animate);
  if(window.avatar) window.avatar.rotation.y += 0.002;
  renderer.render(scene, camera);
}

const log = document.getElementById('log');
document.getElementById('prompt').onkeypress = async e => {
  if(e.key === 'Enter') {
    const question = e.target.value;
    e.target.value = '';
    log.innerHTML += '<div><b>شما:</b> ' + question + '</div>';
    const answer = 'سلام! من آرون هستم و این یک پاسخ اولیه است.';
    log.innerHTML += '<div><b>آرون:</b> ' + answer + '</div>';
    log.scrollTop = log.scrollHeight;
  }
};
</script>
</body>
</html>
