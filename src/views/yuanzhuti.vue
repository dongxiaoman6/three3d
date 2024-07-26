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
      camera.position.z =500; // 设置相机位置
  
      // 创建渲染器
      const renderer = new THREE.WebGLRenderer({ canvas: canvas });
      renderer.setSize(canvas.width, canvas.height);
  
      // 创建圆柱体形状
      const radiusTop = 50; // 圆柱体顶部半径
      const radiusBottom = 50; // 圆柱体底部半径
      const height = 390; // 圆柱体高度
      const radialSegments = 32; // 圆柱体的径向分段数
  
      const geometry = new THREE.CylinderGeometry(radiusTop, radiusBottom, height, radialSegments);
  
      // 创建物理材质
      const material = new THREE.MeshStandardMaterial({
        color: 0xff0000, // 红色
        roughness: 0.5, // 粗糙度
        metalness: 0.5 // 金属度
      });
  
      // 创建圆柱体网格模型
      const cylinder = new THREE.Mesh(geometry, material);
      scene.add(cylinder);
  
      // 添加环境光源
      const ambientLight = new THREE.AmbientLight(0x404040); // 环境光
      scene.add(ambientLight);
  
      // 添加方向光源
      const directionalLight = new THREE.DirectionalLight(0xffffff, 1); // 方向光
      directionalLight.position.set(1, 1, 1).normalize();
      scene.add(directionalLight);
  
      // 动画循环
      function animate() {
        requestAnimationFrame(animate);
  
        // 旋转圆柱体
        cylinder.rotation.x += 0.01;
        cylinder.rotation.y += 0.01;
  
        // 渲染场景
        renderer.render(scene, camera);
      }
      animate();
    }
  };
  </script>
  
  <style scoped>
  canvas {
    display: block;
  }
  </style>
  