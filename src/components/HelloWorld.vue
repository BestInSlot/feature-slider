<template>
  <div class="container">
    <slider :slides="featured" :width="1100"></slider>
  </div>
</template>

<script>
import Slider from "./slider/Slider";
export default {
  components: { Slider },
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App",
      featured: [
        {
          img:
            "https://images.pexels.com/photos/414171/pexels-photo-414171.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
        },
        {
          img:
            "https://images.pexels.com/photos/55821/pyrenees-mountains-snow-zenith-55821.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
        },
        {
          img:
            "https://images.pexels.com/photos/48178/mountains-ice-bergs-antarctica-berg-48178.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
        }
      ]
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  min-width: 960px;
  max-width: 1100px;
  margin: 0 auto;
}
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
