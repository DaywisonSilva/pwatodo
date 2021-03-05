<template>
  <div class="tasks-progress">
      <span class="progress-value"> {{ progress }}% </span>
      <div class="progress-bar" :style="{width: progress + '%'}"></div>
  </div>
</template>

<script>
export default {
    props: {
        progress: { type: Number, default: 0}
    }
}
</script>

<style>
    .tasks-progress {
        position: relative;
        box-shadow: 0px 0px 3px 3px #4caf4f, 0px 0px 0px 6px rgba(172, 255, 47, 0.555);
        width: 80%;
        height: 30px;
        border: 1px solid white;
        color: white;
        border-radius: 8px;
        margin-bottom: 15px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .progress-bar {
        position: absolute;
        border-radius: 8px;
        height: 100%;
        background-color: #4caf50;
        transition: all .2s linear;
        align-self: flex-start;
    }

    .progress-value {
        z-index: 1;
    }
</style>