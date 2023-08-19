<template>
  <div class="card" :style="`--clr2: ${color[0]}; --clr1: ${color[1]};width:${width};`">
    <span></span>
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script setup>
defineProps({
  color: {
    type: Array,
    default: () => ['#00edef', '#a1f3f5']
  },
  width: {
    type: String,
    default: '360px'
  }
})
</script>

<style scoped lang="scss">
.card {
  position: relative;
  color: #fff;
  background: rgba(255, 255, 255, 0.1);
  display: flex;
  justify-content: center;
  align-items: center;
  transition: 0.5s;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(315deg, var(--clr1), var(--clr2));
    opacity: 0.5;
  }

  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(315deg, var(--clr1), var(--clr2));
    filter: blur(30px);
    opacity: 0.4;
  }

  span {
    position: absolute;
    inset: 6px;
    background: rgb(0, 0, 0, 0.6);
    z-index: 2;
  }

  span::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 50%;
    height: 100%;
    background: rgba(255, 255, 255, 0.1);
    pointer-events: none;
  }

  .content {
    position: relative;
    z-index: 10;
    padding: 20px 40px;
  }

  .content h2 {
    font: 2em '';
    color: #fff;
    margin-bottom: 10px;
  }

  .content p {
    font: 1.1em/1.4em '';
    color: #fff;
    margin-bottom: 10px;
  }
}
</style>
