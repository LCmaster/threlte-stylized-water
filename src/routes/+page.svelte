<script>
  import { Canvas, T } from "@threlte/core";
  import { OrbitControls } from "@threlte/extras";
  import Scene from "./Scene.svelte";
</script>

<div class="scene">
  <Canvas>
    <T.AmbientLight color={"#ffffff"} intensity={1} />
    <T.DirectionalLight
      color={"#ffffff"}
      intensity={1}
      position={[50, 50, 50]}
    />
    <T.PerspectiveCamera
      makeDefault
      position={[-135, 125, -135]}
      on:create={({ ref }) => {
        ref.lookAt(0, 0, 0);
      }}
    >
      <OrbitControls enableDamping />
    </T.PerspectiveCamera>
    <Scene />
  </Canvas>
</div>

<style>
  .scene {
    width: 100vw;
    height: 100vh;
  }
</style>
