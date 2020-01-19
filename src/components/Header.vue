<template>
  <div>
    <b-nav tabs class="justify-content-center">
      <b-nav-item disabled>Fancy Quiz</b-nav-item>
      <b-nav-item disabled>Couter :{{correctNums}}/{{totalNums}}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["correctNums", "totalNums"]
};
</script>