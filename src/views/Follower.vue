<template>
    <div ref="container" style="width: 100%; height: 100vh;"></div>
  </template>
  
  <script>
  import * as THREE from "three";
  import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
  import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
  
  export default {
    mounted() {
      const container = this.$refs.container;
  
      // 创建 Three.js 场景、相机、渲染器
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(75, container.clientWidth / container.clientHeight, 0.1, 1000);
      const renderer = new THREE.WebGLRenderer({ antialias: true });
      renderer.setSize(container.clientWidth, container.clientHeight);
      container.appendChild(renderer.domElement);
      renderer.setClearColor(0x444444, 1.0);
  
      // 加载 GLTF 模型
      const loader = new GLTFLoader();
      loader.load(
        "/models/follower/scene.gltf", // 使用相对于 public 的路径
        gltf => {
          scene.add(gltf.scene);
  
          // 调整相机位置
          const box = new THREE.Box3().setFromObject(gltf.scene);
          const size = box.getSize(new THREE.Vector3()).length();
          const center = box.getCenter(new THREE.Vector3());
  
          gltf.scene.position.x -= center.x;
          gltf.scene.position.y -= center.y;
          gltf.scene.position.z -= center.z;
  
          camera.near = size / 100;
          camera.far = size * 100;
          camera.updateProjectionMatrix();
          camera.position.copy(center);
          camera.position.x += size;
          camera.position.y += size;
          camera.position.z += size;
          camera.lookAt(center);
  
          // 添加简单的光源
          const light = new THREE.DirectionalLight(0xffffff, 1);
          light.position.set(size, size, size).normalize();
          scene.add(light);
        },
        undefined,
        error => {
          console.error('An error happened', error);
        }
      );
  
      // 设置相机位置
      camera.position.z = 5;
  
      // 添加 OrbitControls 控制器
      const controls = new OrbitControls(camera, renderer.domElement);
      controls.enableDamping = true;
      controls.dampingFactor = 0.05;
      controls.screenSpacePanning = false;
      controls.minDistance = 1;
      controls.maxDistance = 1000;
  
      // 设置一个地平线网格
      const gridHelper = new THREE.GridHelper(100, 100);
      scene.add(gridHelper);
  
      // 动画循环
      const animate = function () {
        requestAnimationFrame(animate);
        controls.update();
        renderer.render(scene, camera);
      };
      animate();
    },
  };
  </script>
  
  <style scoped>
  div {
    width: 100%;
    height: 100%;
  }
  </style>
  