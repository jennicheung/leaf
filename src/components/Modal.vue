<template>
  <div
    v-if="image && image !== ''"
    class="modal"
    v-on:click="(e) => $emit('close', e)"
  >
    <img v-bind:src="image" />
  </div>
</template>

<script>
export default {
  name: "Modal",
  props: {
    image: null,
  },
  data() {
    return {};
  },
  methods: {},
  mounted() {},
};
</script>

<style lang="scss" scoped>
.modal {
  position: fixed;
  width: 100%;
  height: 100vh;
  z-index: 100;
  top: 0;
  left: 0;
  padding: 0;
  margin: 0;
  cursor: pointer;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;

  img {
    max-width: calc(100% - 2em);
    max-height: calc(100% - 2em);
  }
}
</style>
