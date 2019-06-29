<template lang="pug">
  .app-container
    .image-container
      .background-image
        img(src="./assets/foos.png" alt="foosball table")
    router-view
</template>


<style lang="scss">
body {
  margin: 0;
}

.app-container {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #2c3e50;
  min-height: 100vh;
}

.background-image {
  display: flex;
  justify-content: center;
}

img {
  position: absolute;
  top: 100px;
  opacity: 0.05;
  height: 500px;
  z-index: -1;
}

@media(max-width: 505px) {
  img {
    height: 300px;
  }
}

@media(min-width: 1000px) {
  img {
    height: 800px;
  }
}
</style>
