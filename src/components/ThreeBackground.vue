<template>
  <div ref="container" class="fixed inset-0 z-0 bg-[#050508]"></div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';
import * as THREE from 'three';

const container = ref<HTMLElement | null>(null);
let renderer: THREE.WebGLRenderer;
let scene: THREE.Scene;
let camera: THREE.PerspectiveCamera;
let points: THREE.Points;
let animationFrameId: number;

onMounted(() => {
  if (!container.value) return;

  scene = new THREE.Scene();
  camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
  camera.position.z = 500;

  renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setClearColor('#050508');
  container.value.appendChild(renderer.domElement);

  const count = 5000;
  const geometry = new THREE.BufferGeometry();
  const positions = new Float32Array(count * 3);

  for (let i = 0; i < count; i++) {
    positions[i * 3] = (Math.random() - 0.5) * 2000;
    positions[i * 3 + 1] = (Math.random() - 0.5) * 2000;
    positions[i * 3 + 2] = (Math.random() - 0.5) * 2000;
  }

  geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
  
  const material = new THREE.PointsMaterial({
    color: '#00f2ff',
    size: 2,
    transparent: true,
    sizeAttenuation: true,
    depthWrite: false
  });

  points = new THREE.Points(geometry, material);
  scene.add(points);

  const animate = () => {
    animationFrameId = requestAnimationFrame(animate);
    points.rotation.y += 0.001;
    points.rotation.x += 0.0005;
    renderer.render(scene, camera);
  };

  animate();

  window.addEventListener('resize', handleResize);
});

const handleResize = () => {
  if (!container.value) return;
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
};

onBeforeUnmount(() => {
  cancelAnimationFrame(animationFrameId);
  window.removeEventListener('resize', handleResize);
  renderer.dispose();
});
</script>
