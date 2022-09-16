<template>
  <div class="wrapper">
    <HeaderComponent/>
    <SlideComponent/>
    <MainComponent />
    <FormComponent />
  </div>
</template>

<script>
import MainComponent from './components/MainComponent.vue'
import HeaderComponent from "@/components/HeaderComponent";
import SlideComponent from "@/components/SlideComponent";
import FormComponent from "@/components/FormComponent";
export default {
  name: 'App',
  components: {
    HeaderComponent,
    MainComponent,
    SlideComponent,
    FormComponent
  }
}
</script>

<style>
.wrapper{
  text-align: center;
  margin: 0 auto;
  max-width: 1024px;
  min-width: 360px;
}
</style>
