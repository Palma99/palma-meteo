<template>
<div id="bar" class="topnav">
  <a>Meteo</a>
</div>
</template>

<script>
import { onMounted } from 'vue';
export default {
  props: {
    first: String,
    second: String
  },
  setup(props){
    onMounted(() => {
      const bar = document.getElementById('bar');
      bar.style.backgroundImage = `linear-gradient(90deg, ${props.first}, ${props.second})`;
    });

  }
}
</script>

<style scoped>
.topnav {
  box-shadow: 0px 2px 13px 0px rgba(0, 0, 0, 0.471);
  overflow: hidden;
}

.topnav a {
  float: left;
  color: #f2f2f2;
  text-align: center;
  padding: 0.5rem;
  text-decoration: none;
  font-size: 2rem;
  font-family: Varela Round;
  text-shadow: 0px 1px 20px #00000090;
}

</style>