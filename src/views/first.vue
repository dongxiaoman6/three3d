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
  
      // 创建长方体形状
      const geometry = new THREE.BoxGeometry(100, 100, 100);
      // 创建长方体材质
      const material = new THREE.MeshBasicMaterial({ color: 0x0000ff });
      // 创建长方体网格模型
      const mesh = new THREE.Mesh(geometry, material);
      // 将长方体添加到场景中
      scene.add(mesh);
  
      // 动画循环
      function animate() {
        requestAnimationFrame(animate);
  
        // 旋转长方体
        mesh.rotation.x += 0.01;
        mesh.rotation.y += 0.01;
  
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
  