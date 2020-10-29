<template>
    
    <div class="container">
        <span>
            Meteo {{cityData.name}}, {{cityData.country}}
        </span>

        <div v-if="ready" class="data-cont">
            <div>
                {{weather.main.temp}}°C
            </div>
            <div class="secondary">
                <span>max {{weather.main.temp_max}}°C</span>
                <span>min {{weather.main.temp_min}}°C</span>
            </div>
            <div class="secondary" style="margin-top:0.3em;font-size:0.8em;">
                {{weather.description}}
            </div>
            <div>
                <img :src="icon" />
            </div>
            <input type="button" value="Indietro" @click="back">
        </div>
        
    </div>
</template>

<script>
import axios from 'axios';
import { onMounted, reactive, ref, computed } from 'vue';
const API_URL = 'https://api.openweathermap.org/data/2.5/weather';

const cityData = reactive({
    name: String,
    country: String
});

const ready = ref(false);

let weather = reactive({});
const icon = ref('');

export default {
    props: {
        first: String,
        second: String,
        city: Object
    },
    emits: ['go-home'],
    setup(props, context){
        onMounted(async () => {
            const bar = document.querySelector('.container');
            bar.style.backgroundImage = `linear-gradient(to bottom, ${props.first}, ${props.second})`;
            
            cityData.name = props.city.name;
            cityData.country = props.city.country;

            try {
                const resp = await axios.get(urlData());

                Object.assign(weather, resp.data);

                ready.value = true;

                // Capitalize description
                weather.description = computed(() => 
                    weather.weather[0].description.charAt(0).toUpperCase() 
                    + weather.weather[0].description.slice(1)
                );

                icon.value = `https://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`;
            } catch (err) {
                
            }
        });

        const back = () => {
          context.emit('go-home');
        };

        const urlData = () => API_URL + `?q=${cityData.name},${cityData.country}&units=metric&lang=it&appid=${process.env.VUE_APP_KEY}`;

        return {
            cityData,
            weather,
            ready,
            icon,
            back
        }
    }
}
</script>

<style scoped>
.secondary {
    font-size: 0.4em;
    display: grid;
    width: fit-content;
    margin: 0;
    margin-top: 0.1rem;
}

.data-cont {
    margin-top: 0.4em;
    font-size: 1.8em;
    justify-content: space-between;
}

img {
    margin-top:0.4em;
    box-shadow: 0px 4px 10px 0px rgba(0, 0, 0, 0.3);
    border-radius: 30%;
    background: #eaeaea58;
}

input {
    font-family: Varela Round; 
    font-size: 0.8em;
    border-radius: 8px;
    border: 1px solid transparent;
    outline: none;
    background: transparent;
    margin-top: 0.4em;
    background: transparent;
    text-shadow: 0px 1px 11px #00000070;
    color: rgba(255, 255, 255, 0.751);
}

input:hover {
  box-shadow: 0px 4px 10px 0px rgba(0, 0, 0, 0.3);
  cursor: pointer;
}

.container {
  padding: 0.5em;
  font-size: 2em;
  color: white;
  margin-top: 20vh;
  width: 60vw;
  /* height: 30vh; */
  border-radius: 10px;
  font-family: Varela Round;  
  box-shadow: 0px 20px 30px 0px rgb(0, 0, 0);
  text-shadow: 0px 1px 20px #00000090;
  overflow: hidden;
}
</style>