<script>
  import { T, forwardEventHandlers } from "@threlte/core";
  import { Suspense, useSuspense } from "@threlte/extras";
  import {
    Group,
    Mesh,
    MeshStandardMaterial,
    RepeatWrapping,
    ShaderMaterial,
    Texture,
  } from "three";
  import PirateIsland from "./PirateIsland.svelte";
  import Water from "./Water.svelte";

  export const ref = new Group();
  const component = forwardEventHandlers();
</script>

<T is={ref} dispose={false} {...$$restProps} bind:this={$component}>
  <Suspense>
    <PirateIsland />
    <Water />
  </Suspense>
  <slot {ref} />
</T>
