<template>
  <div class="container">
    <slider :slides="featured" :width="1100"></slider>
  </div>
</template>

<script>
import Slider from "./slider/Slider";

const desc =
  "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged.";

export default {
  components: { Slider },
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App",
      featured: [
        {
          img:
            "https://images.pexels.com/photos/414171/pexels-photo-414171.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260",
          desc,
          title: "Trees N Shit"
        },
        {
          img:
            "https://images.pexels.com/photos/55821/pyrenees-mountains-snow-zenith-55821.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260",
          desc,
          title: "Earth N Shit"
        },
        {
          img:
            "https://images.pexels.com/photos/48178/mountains-ice-bergs-antarctica-berg-48178.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260",
          desc,
          title: "Mountains N Shit"
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
