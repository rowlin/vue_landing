<template>
  <nav class="navbar">
    <a href="/" class="logo" >
      <img src="../assets/Logo.svg" alt="Logo">
    </a>
    <div class="buttons">
    <button  class="y_btn">
      Users
    </button>
    <button  class="y_btn">
      Sign up
    </button>
    </div>
  </nav>
</template>

<script>
export default {
  name: "HeaderComponent"
}
</script>

<style lang="scss"  scoped>
.navbar {
  min-height: 60px;
  overflow: auto;
  .logo{
    margin: 17px 32px;
    float: left;
    @media only screen and (max-width : 768px) {
      margin:17px 20px;
    }
    @media only screen and (max-width : 360px) {
      margin:17px 10px;
    }
  }
  .buttons{
    margin-top: 9px;
    margin-right:32px;
    float: right;
    @media only screen and (max-width : 768px) {
      margin-right:20px;
    }
    @media only screen and (max-width : 360px) {
      margin-right:10px;
    }
    button:first-child{
      margin-right: 10px;
    }
  }

}

</style>