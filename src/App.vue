<template>
  <div v-if="ready">
    <NavBar :first="colors.firstColor" :second="colors.secondColor" />
    <div class="cont">
      <InputForm 
        v-if="!validInput" 
        @search="search" 
        :first="colors.firstColor" 
        :second="colors.secondColor"
      />
      <ViewData 
        v-if="validInput" 
        :first="colors.firstColor" 
        :second="colors.secondColor"
        :city="cityObject"
        @go-home="home"
      />
    </div>

  </div>
</template>

<script>
import NavBar from './components/NavBar';
import ViewData from './components/ViewData';
import InputForm from './components/InputForm';

import getColors from 'get-image-colors';

import { ref, computed, onMounted, reactive } from 'vue';

let colors = ref({
  firstColor: "",
  secondColor: ""
});

let ready = ref(false);
let validInput = ref(false);
let cityObject = reactive({});

const BG_IMG = 'https://source.unsplash.com/1600x950/?nature';
export default {
  name: "App",
  components: {
    NavBar,
    InputForm,
    ViewData
  },
  setup() {

    onMounted(async () => {
      const body = document.querySelector('body');

      const response = await fetch(BG_IMG);

      const palette = await getColors(response.url);

      body.style.backgroundImage = `url(${response.url})`;
      body.style.backgroundColor = `rgba(${[...palette[0]._rgb]})`;

      colors.value.firstColor = `rgba(${[...palette[0]._rgb]})`;
      colors.value.secondColor = `rgba(${[...palette[1]._rgb]})`;

      ready.value = true;
    });

    const search = (city) => {
      Object.assign(cityObject, city);
      validInput.value = true;
    }

    const home = () => {
      validInput.value = false;
    }

    return {
      colors,
      ready,
      validInput,
      cityObject,
      search,
      home
    }

  }
};
</script>

<style>
body{
  margin: 0;  
  background-repeat: no-repeat;
  background-position-x: center;
}
.cont {
  text-align: center;
  text-align: -webkit-center;
}

</style>
