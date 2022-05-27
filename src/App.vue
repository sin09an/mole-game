<script>
import Mole from "./components/Mole.vue";
import list from "./components/list.vue";

export default {
  components: {
    Mole,
    list,
  },
  data() {
    return {
      record_list: [],  //부모로 빈 배열만 가지고 있고
    };
  },   
}; 
</script>       
   
<template>
  <main>
    <!-- list값 각각 컴포넌트에 바인딩 -->
    <Mole :list_adds="record_list" /> 
  </main>

  <main>
    <list :list_adds="record_list" />
  </main>
</template> 
 
<style>
@import "./assets/base.css";

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

/* a, */
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }

  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
  #mmmm {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
}
</style>
