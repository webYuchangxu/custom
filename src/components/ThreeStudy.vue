<template>
  <div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import * as THREE from "three";
@Component
export default class ThreeStudy extends Vue {
  public camera: any;
  public scene: any;
  public renderer: any;
  public geometry: any;
  public material: any;
  public mesh: any;

  public created() {
    console.log(THREE);
    this.init();
  }
  public init() {
    this.camera = new THREE.PerspectiveCamera(
      70,
      window.innerWidth / window.innerHeight,
      0.01,
      10,
    );
    this.camera.position.z = 1;

    this.scene = new THREE.Scene();

    this.geometry = new THREE.BoxGeometry(0.2, 0.2, 0.2);
    this.material = new THREE.MeshNormalMaterial();

    this.mesh = new THREE.Mesh(this.geometry, this.material);
    this.scene.add(this.mesh);

    this.renderer = new THREE.WebGLRenderer({ antialias: true });
    this.renderer.setSize(window.innerWidth, window.innerHeight);
    this.renderer.setAnimationLoop(this.animation);
    document.body.appendChild(this.renderer.domElement);
  }
  public animation(time:any) {
    this.mesh.rotation.x = time / 2000;
    this.mesh.rotation.y = time / 1000;

    this.renderer.render(this.scene, this.camera);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
