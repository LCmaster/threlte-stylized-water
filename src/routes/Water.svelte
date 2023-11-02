<script lang="ts">
  import { T, forwardEventHandlers } from "@threlte/core";
  import { useSuspense, useTexture } from "@threlte/extras";
  import {
    Group,
    Mesh,
    MeshStandardMaterial,
    RepeatWrapping,
    ShaderMaterial,
    Texture,
  } from "three";

  export const ref = new Group();

  // const vertexShader = `
  //   void main() {
  //     gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
  //   }
  // `;
  // const fragmentShader = `
  //   void main() {
  //     gl_FragColor = vec4(0.0, 0.0, 1.0, 1.0);
  //   }
  // `;
  // const uniforms = {};

  let textureParams = {
    transform: (t: Texture) => {
      t.wrapS = RepeatWrapping;
      t.wrapT = RepeatWrapping;
      t.repeat.set(10, 10);
      return t;
    },
  };

  let textures = Promise.all([
    useTexture("/foam.png", textureParams),
    useTexture("/normal.jpg", textureParams),
  ]);

  const suspend = useSuspense();
  suspend(textures);

  let waterRef: Mesh;

  const component = forwardEventHandlers();
</script>

<T is={ref} dispose={false} {...$$restProps} bind:this={$component}>
  {#await textures}
    <slot name="fallback" />
  {:then [albedo, normal]}
    <T.Mesh
      bind:ref={waterRef}
      rotation={[-Math.PI * 0.5, 0, 0]}
      position={[0, 0, 0]}
    >
      <T.PlaneGeometry args={[800, 800, 128, 128]} />
      <T.MeshStandardMaterial map={albedo} normalMap={normal} />
    </T.Mesh>
  {/await}
  <slot {ref} />
</T>
