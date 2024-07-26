<template>
    <canvas ref="canvas" style="background: black;" width="800" height="800"></canvas>
  </template>
  
  <script>
  import * as THREE from 'three';
  
  export default {
    mounted() {
      // 获取 canvas 元素
      const canvas = this.$refs.canvas;
  
      // 创建 Three.js 场景
      const scene = new THREE.Scene();
  
      // 创建相机
      const camera = new THREE.PerspectiveCamera(75, canvas.width / canvas.height, 0.1, 1000);
      camera.position.z = 500; // 设置相机位置
  
      // 创建渲染器
      const renderer = new THREE.WebGLRenderer({ canvas: canvas });
      renderer.setSize(canvas.width, canvas.height);
      renderer.shadowMap.enabled = true; // 启用阴影
  
      // 加载背景纹理
      const textureLoader = new THREE.TextureLoader();
      const backgroundTexture = textureLoader.load('/models/gongchang/photo.jpg'); // 替换为实际图片路径
  
      // 调整背景纹理平铺模式
      backgroundTexture.wrapS = THREE.RepeatWrapping;
      backgroundTexture.wrapT = THREE.RepeatWrapping;
      backgroundTexture.repeat.set(0.5, 0.5); // 调整纹理的重复次数，0.5 表示每个方向重复两次
  
      // 设置场景背景纹理
      scene.background = backgroundTexture;
  
      // 创建球体形状
      const radius = 100; // 球体半径
      const widthSegments = 32; // 水平分段数
      const heightSegments = 32; // 垂直分段数
      const geometry = new THREE.SphereGeometry(radius, widthSegments, heightSegments);
  
      // 创建材质，使用纹理
      const sphereTexture = textureLoader.load('/models/gongchang/bg.jpg'); // 替换为实际图片路径
      const sphereMaterial = new THREE.MeshStandardMaterial({
        map: sphereTexture, // 应用纹理
        roughness: 0.7, // 材质粗糙度
        metalness: 0.2 // 材质金属感
      });
  
      // 创建球体网格模型
      const sphere = new THREE.Mesh(geometry, sphereMaterial);
      sphere.castShadow = true; // 使球体投射阴影
      sphere.receiveShadow = true; // 使球体接收阴影
      scene.add(sphere);
  
      // 添加方向光
      const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
      directionalLight.position.set(1, 1, 1).normalize();
      directionalLight.castShadow = true; // 使光源投射阴影
      scene.add(directionalLight);
  
      // 添加环境光
      const ambientLight = new THREE.AmbientLight(0x404040); // 环境光
      scene.add(ambientLight);
  
      // 动画循环
      function animate() {
        requestAnimationFrame(animate);
  
        // 旋转球体
        sphere.rotation.x += 0.01;
        sphere.rotation.y += 0.01;
  
        // 渲染场景
        renderer.render(scene, camera);
      }
      animate();
  
      // 处理窗口大小调整
      window.addEventListener('resize', () => {
        camera.aspect = canvas.width / canvas.height;
        camera.updateProjectionMatrix();
        renderer.setSize(canvas.width, canvas.height);
      });
    }
  };
  </script>
  
  <style scoped>
  canvas {
    display: block;
  }
  </style>
  