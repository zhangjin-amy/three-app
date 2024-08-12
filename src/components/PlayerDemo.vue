<template>
  <div ref="threeContainer" class="three-container"></div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

const threeContainer = ref(null);

onMounted(() => {
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
  const renderer = new THREE.WebGLRenderer({ antialias: true });

  renderer.setClearColor(0xff05ff, 1);
  renderer.setSize(window.innerWidth, window.innerHeight);
  threeContainer.value.appendChild(renderer.domElement);

  // 添加环境光
  const ambientLight = new THREE.AmbientLight(0xffffff, 1);
  scene.add(ambientLight);

  // 添加点光源
  const pointLight = new THREE.PointLight(0xffffff, 2);
  pointLight.position.set(5, 5, 5);
  scene.add(pointLight);

  const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
  directionalLight.position.set(1, 1, 1).normalize();
  scene.add(directionalLight);

  // 加载 .glb 文件
  const loader = new GLTFLoader();
  let model;
  loader.load('/res/player.glb', (gltf) => {
    model = gltf.scene;

    // 调整模型的缩放和位置
    model.scale.set(3, 3, 3); // 根据需要调整缩放
    model.position.set(0, 0, 0); // 根据需要调整位置
    model.rotation.y = Math.PI * 1.5;

    scene.add(model);
  }, undefined, (error) => {
    console.error('An error happened', error);
  });

  camera.position.z = 5;

  // 添加 OrbitControls
  const controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true; // 启用阻尼效果
  controls.dampingFactor = 0.25; // 阻尼系数
  controls.screenSpacePanning = false; // 禁用屏幕空间平移

  const animate = function () {
    requestAnimationFrame(animate);

    // 更新控制器
    controls.update();

    renderer.render(scene, camera);
  };

  animate();

  // 处理窗口大小调整
  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });
});
</script>

<style>
.three-container {
  width: 100%;
  height: 100vh;
  overflow: hidden;
}
</style>