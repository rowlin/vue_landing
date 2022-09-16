<template>
<div id="slide">
  <div class="text-block">
  <h1>
    Test assignment for front-end developer
  </h1>
  <p>
    What defines a good front-end developer is one that has skilled knowledge of HTML, CSS, JS with a vast understanding of User design thinking as they'll be building web interfaces with accessibility in mind. They should also be excited to learn, as the world of Front-End Development keeps evolving.
  </p>
    <button class="y_btn">
      Sign up
    </button>
  </div>
</div>
</template>

<script>
export default {
  name: "SlideComponent"
}
</script>

<style lang="scss" scoped>
#slide{
  background-image:  linear-gradient( rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5) ), url('../assets/pexels-alexandr-podvalny-1227513.jpeg');
  background-position: center center;
  background-repeat: no-repeat;
  background-size: cover;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 500px;
  .text-block{
    max-width: 380px;
    width: 100%;
    p{
      color: #f8f8f8;
    }
    h1{
      color: #f8f8f8;
    }
  }
}

</style>