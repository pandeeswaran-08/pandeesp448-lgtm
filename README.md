
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body { background: transparent; }
#canvas3d { width: 100%; height: 520px; display: block; }
.profile-overlay {
  position: absolute; top: 0; left: 0; right: 0;
  pointer-events: none;
  display: flex; flex-direction: column; align-items: center;
  padding-top: 28px; gap: 6px;
}
.profile-name {
  font-family: var(--font-sans);
  font-size: 22px; font-weight: 500;
  color: #fff; text-shadow: 0 0 20px rgba(100,180,255,0.8);
  letter-spacing: 1px;
}
.profile-title {
  font-family: var(--font-sans);
  font-size: 13px; font-weight: 400;
  color: rgba(160,210,255,0.9);
  letter-spacing: 2px; text-transform: uppercase;
}
.typing-line {
  font-family: var(--font-mono);
  font-size: 12px;
  color: rgba(100,255,180,0.85);
  letter-spacing: 1px;
  min-height: 18px;
}
.stack-badges {
  display: flex; flex-wrap: wrap; justify-content: center; gap: 6px;
  padding: 0 40px; margin-top: 6px;
}
.badge {
  font-family: var(--font-mono);
  font-size: 10px; font-weight: 400;
  padding: 3px 10px;
  border-radius: 20px;
  border: 1px solid rgba(100,180,255,0.4);
  color: rgba(160,220,255,0.9);
  background: rgba(20,60,120,0.5);
  backdrop-filter: blur(4px);
}
.goals-panel {
  position: absolute; bottom: 18px; left: 50%; transform: translateX(-50%);
  display: flex; gap: 10px; pointer-events: none;
}
.goal-chip {
  font-family: var(--font-sans);
  font-size: 10px;
  padding: 4px 12px;
  border-radius: 20px;
  border: 1px solid rgba(120,255,180,0.35);
  color: rgba(150,255,200,0.85);
  background: rgba(10,40,30,0.5);
  white-space: nowrap;
}
</style>

<div style="position: relative; border-radius: var(--border-radius-lg); overflow: hidden; background: #030c1a;">
  <canvas id="canvas3d"></canvas>
  <div class="profile-overlay">
    <div class="profile-name">Pandeeswaran Pichaipandi</div>
    <div class="profile-title">AI Engineer · Backend Developer · Agentic AI</div>
    <div class="typing-line" id="typing-out"></div>
    <div class="stack-badges">
      <span class="badge">LangChain</span>
      <span class="badge">LangGraph</span>
      <span class="badge">LlamaIndex</span>
      <span class="badge">FastAPI</span>
      <span class="badge">OpenAI</span>
      <span class="badge">Gemini</span>
      <span class="badge">RAG</span>
      <span class="badge">MCP</span>
    </div>
  </div>
  <div class="goals-panel">
    <span class="goal-chip">🚀 Ship AI Agents</span>
    <span class="goal-chip">🧩 Multi-Agent Systems</span>
    <span class="goal-chip">🔌 MCP Architecture</span>
    <span class="goal-chip">💡 AI SaaS</span>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
const canvas = document.getElementById('canvas3d');
const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true });
renderer.setPixelRatio(window.devicePixelRatio || 1);
renderer.setClearColor(0x030c1a, 1);

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(60, canvas.clientWidth / canvas.clientHeight, 0.1, 1000);
camera.position.set(0, 0, 22);

function resize() {
  const w = canvas.clientWidth, h = canvas.clientHeight;
  renderer.setSize(w, h, false);
  camera.aspect = w / h;
  camera.updateProjectionMatrix();
}
resize();

const ambientLight = new THREE.AmbientLight(0x1a3a6a, 0.8);
scene.add(ambientLight);
const pointLight1 = new THREE.PointLight(0x4488ff, 2.5, 60);
pointLight1.position.set(10, 10, 10);
scene.add(pointLight1);
const pointLight2 = new THREE.PointLight(0x00ffaa, 1.8, 60);
pointLight2.position.set(-10, -5, 8);
scene.add(pointLight2);
const pointLight3 = new THREE.PointLight(0xff6644, 1.2, 40);
pointLight3.position.set(0, -10, 6);
scene.add(pointLight3);

// Central core sphere
const coreGeo = new THREE.IcosahedronGeometry(2.2, 2);
const coreMat = new THREE.MeshStandardMaterial({
  color: 0x0a1f4a,
  metalness: 0.9,
  roughness: 0.15,
  emissive: 0x0a2a6e,
  emissiveIntensity: 0.4,
  wireframe: false
});
const core = new THREE.Mesh(coreGeo, coreMat);
scene.add(core);

// Wireframe overlay on core
const wireGeo = new THREE.IcosahedronGeometry(2.25, 2);
const wireMat = new THREE.MeshBasicMaterial({ color: 0x4488ff, wireframe: true, opacity: 0.25, transparent: true });
const wireCore = new THREE.Mesh(wireGeo, wireMat);
scene.add(wireCore);

// Orbital ring 1
function makeRing(innerR, outerR, color, tilt, rotSpeed) {
  const geo = new THREE.TorusGeometry((innerR + outerR) / 2, (outerR - innerR) / 2, 8, 80);
  const mat = new THREE.MeshStandardMaterial({
    color, metalness: 0.7, roughness: 0.3,
    emissive: color, emissiveIntensity: 0.15,
    transparent: true, opacity: 0.55
  });
  const ring = new THREE.Mesh(geo, mat);
  ring.rotation.x = tilt;
  ring.rotation.y = Math.random() * Math.PI;
  ring.userData.rotSpeed = rotSpeed;
  return ring;
}
const ring1 = makeRing(3.8, 4.1, 0x2255cc, Math.PI / 4, 0.003);
const ring2 = makeRing(5.0, 5.25, 0x00cc88, Math.PI / 2.5, -0.002);
const ring3 = makeRing(6.4, 6.6, 0xff5533, Math.PI / 1.7, 0.0015);
scene.add(ring1); scene.add(ring2); scene.add(ring3);

// Floating nodes (tech stack)
const nodeLabels = ['LangChain','LangGraph','LlamaIndex','FastAPI','Python','Docker','OpenAI','Gemini','RAG','MCP','PostgreSQL','ChromaDB'];
const nodeColors = [0x4499ff,0x00cc88,0xff8844,0x55aaff,0x88ff44,0x44ddff,0xaa44ff,0xff4488,0xffcc00,0x44ffcc,0xff6644,0x44aaff];
const nodes = [];

for (let i = 0; i < nodeLabels.length; i++) {
  const angle = (i / nodeLabels.length) * Math.PI * 2;
  const radius = 7.5 + Math.sin(i * 1.3) * 1.5;
  const yOffset = Math.cos(i * 0.9) * 3.5;

  const geo = new THREE.OctahedronGeometry(0.35 + Math.random() * 0.15, 0);
  const mat = new THREE.MeshStandardMaterial({
    color: nodeColors[i],
    metalness: 0.8, roughness: 0.2,
    emissive: nodeColors[i], emissiveIntensity: 0.4
  });
  const node = new THREE.Mesh(geo, mat);
  node.position.set(
    Math.cos(angle) * radius,
    yOffset,
    Math.sin(angle) * radius * 0.5
  );
  node.userData = { baseY: yOffset, phase: Math.random() * Math.PI * 2, speed: 0.008 + Math.random() * 0.005 };
  scene.add(node);
  nodes.push(node);
}

// Connection lines from core to nodes
const lineMat = new THREE.LineBasicMaterial({ color: 0x2266aa, opacity: 0.18, transparent: true });
nodes.forEach(node => {
  const pts = [new THREE.Vector3(0,0,0), node.position.clone()];
  const geo = new THREE.BufferGeometry().setFromPoints(pts);
  scene.add(new THREE.Line(geo, lineMat));
});

// Floating particles
const particleCount = 280;
const positions = new Float32Array(particleCount * 3);
for (let i = 0; i < particleCount; i++) {
  positions[i*3]   = (Math.random() - 0.5) * 40;
  positions[i*3+1] = (Math.random() - 0.5) * 28;
  positions[i*3+2] = (Math.random() - 0.5) * 30;
}
const particleGeo = new THREE.BufferGeometry();
particleGeo.setAttribute('position', new THREE.BufferAttribute(positions, 3));
const particleMat = new THREE.PointsMaterial({ color: 0x5599ff, size: 0.08, transparent: true, opacity: 0.55 });
scene.add(new THREE.Points(particleGeo, particleMat));

// Typing animation
const lines = [
  'Backend Developer → AI Engineer',
  'Building Production RAG Apps',
  'LangGraph Multi-Agent Workflows',
  'Exploring MCP Architecture',
  'Crafting Agentic AI Systems',
];
let lineIdx = 0, charIdx = 0, deleting = false;
const typingEl = document.getElementById('typing-out');
function typeStep() {
  const cur = lines[lineIdx];
  if (!deleting) {
    charIdx++;
    typingEl.textContent = cur.slice(0, charIdx) + '▌';
    if (charIdx >= cur.length) { deleting = true; setTimeout(typeStep, 1600); return; }
  } else {
    charIdx--;
    typingEl.textContent = cur.slice(0, charIdx) + '▌';
    if (charIdx === 0) { deleting = false; lineIdx = (lineIdx + 1) % lines.length; setTimeout(typeStep, 300); return; }
  }
  setTimeout(typeStep, deleting ? 35 : 55);
}
typeStep();

// Mouse interaction
let mouseX = 0, mouseY = 0;
document.addEventListener('mousemove', e => {
  const rect = canvas.getBoundingClientRect();
  mouseX = ((e.clientX - rect.left) / rect.width - 0.5) * 2;
  mouseY = -((e.clientY - rect.top) / rect.height - 0.5) * 2;
});

let t = 0;
function animate() {
  requestAnimationFrame(animate);
  t += 0.01;

  core.rotation.y += 0.004;
  core.rotation.x += 0.002;
  wireCore.rotation.y -= 0.003;
  wireCore.rotation.z += 0.001;

  ring1.rotation.z += ring1.userData.rotSpeed;
  ring2.rotation.z += ring2.userData.rotSpeed;
  ring3.rotation.z += ring3.userData.rotSpeed;

  nodes.forEach((n, i) => {
    n.rotation.x += 0.02;
    n.rotation.y += 0.015;
    n.position.y = n.userData.baseY + Math.sin(t * n.userData.speed * 100 + n.userData.phase) * 0.6;
  });

  // Gentle camera drift following mouse
  camera.position.x += (mouseX * 3 - camera.position.x) * 0.03;
  camera.position.y += (mouseY * 2 - camera.position.y) * 0.03;
  camera.lookAt(0, 0, 0);

  pointLight1.position.x = Math.sin(t * 0.7) * 12;
  pointLight1.position.z = Math.cos(t * 0.7) * 12;
  pointLight2.position.x = Math.cos(t * 0.5) * 10;
  pointLight2.position.z = Math.sin(t * 0.5) * 10;

  renderer.render(scene, camera);
}
animate();

window.addEventListener('resize', resize);
</script>
