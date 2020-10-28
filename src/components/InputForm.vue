<template>
<div>
  <div class="container">
      <form @submit.prevent="submit" autocomplete="off">
        <transition name="fade">
          <div v-if="notValid" class="error-message">
            Città o stato non validi
          </div>
        </transition>
        <input v-model="city" id="cityName" type="text" placeholder="Città">
        <input type="submit" value="Cerca">
      </form>
  </div>
</div>
</template>

<script>
import { onMounted, ref, watch } from 'vue';
import cityList from '../../public/city.json';

let city = ref('');
let notValid = ref(false);

export default {
  props: {
    first: String,
    second: String
  },
  emits: ['search'],
  setup(props, context){
    onMounted(() => {
      const text = document.querySelector('#cityName').focus();
      const bar = document.querySelector('.container');
      bar.style.backgroundImage = `linear-gradient(to bottom, ${props.first}, ${props.second})`;
    });


    watch(city, () => {
      notValid.value = false;
    });

    const submit = () => {

      if(notValid.value){
        return;
      }

      let [name, country] = city.value.split(',');

      name = name.trim();
      if (country){
        country = country.trim();
      }

      if (name === '') {
        return;
      }

      // Input validation

      const cityFound = cityList
      .filter((city) => {
        if (city.name.toLowerCase() !== name.toLowerCase()) {
          return false;
        } else {
          return country === undefined 
          ? true 
          : country.toUpperCase() === city.country
        }
      });

      if(cityFound.length > 0){
        context.emit('search', cityFound[0]);
        city.value = '';
      } else {
        notValid.value = true;
        document.getElementById('cityName').select()
      }
      
    }

    return {
        submit,
        notValid,
        city
    }
  }
}
</script>

<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: opacity .3s ease-in-out;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}

.error-message {
  margin-bottom: 0.4em;
}

.container {
  padding: 0.5em;
  font-size: 2em;
  color: white;
  margin-top: 20vh;
  width: 48vw;
  border-radius: 10px;
  font-family: Varela Round;  
  box-shadow: 0px 20px 30px 0px rgb(0, 0, 0);
  text-shadow: 0px 1px 20px #00000090;
  overflow: hidden;
}

form {
    display: grid;
}

p {
    margin: 0;
}

::placeholder {
  color: white;
  opacity: 0.5; 
}

input {
    font-family: Varela Round; 
    font-size: 1em;
    border-radius: 4px;
    border: 1px solid transparent;
    outline: none;
    background: transparent;
    width: 90%;
}

input[type='text'] {
    margin-top: 0.5em;
    font-size: 0.9em;
    background: transparent;
    color: white;
    box-shadow: 0px 4px 10px 0px rgba(0, 0, 0, 0.3);
}

input[type='submit']{
    margin-top: 0.5em;
    background: transparent;
    color: rgba(255, 255, 255, 0.71);
    width: min-content;
}

input[type='submit']:hover {
  box-shadow: 0px 4px 10px 0px rgba(0, 0, 0, 0.3);
  cursor: pointer;
}

</style>