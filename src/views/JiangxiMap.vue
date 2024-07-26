<template>
  <canvas ref="canvas" style="background: black;" width="800" height="800"></canvas>
</template>

<script>
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

export default {
  mounted() {
    const canvas = this.$refs.canvas;

    // 创建 Three.js 场景
    const scene = new THREE.Scene();
    
    // 创建相机
    const camera = new THREE.PerspectiveCamera(75, canvas.width / canvas.height, 0.1, 1000);
    camera.position.z = 500; // 设置相机位置




    // 创建渲染器
    const renderer = new THREE.WebGLRenderer({ canvas: canvas });
    renderer.setSize(canvas.width, canvas.height);

    // 添加 OrbitControls 控制器
    const controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.05;
    controls.screenSpacePanning = false;
    controls.minDistance = 1;
    controls.maxDistance = 1000;

    // 添加方向光
    const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
    directionalLight.position.set(1, 1, 1).normalize();
    scene.add(directionalLight);

    // 添加环境光
    const ambientLight = new THREE.AmbientLight(0x404040); // 环境光
    scene.add(ambientLight);

    // 创建圆锥体
    const geometry = new THREE.ConeGeometry(50, 200, 32); // 半径、高度、段数
    const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
    const cone = new THREE.Mesh(geometry, material);
    scene.add(cone);

    // 动画循环
    function animate() {
      requestAnimationFrame(animate);
      cone.rotation.y += 0.01; // 旋转动画
      controls.update();
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