<template>
  <div class="grid-wrapper">
    <div v-for="(item, index) in products" :key="index" class="grid" @mouseover="item.isActive = true" @mouseleave="item.isActive = false">
      <transition name="btn-fade">
        <img
          v-show="item.isActive"
          class="button"
          src="../data/images/pngwing.com.png"
          @click="removeProduct(index)"
        >
      </transition>
      <img class="image" :src="item.imgSrc">
      <h3>{{ item.name }}</h3>
      <p class="description">{{ item.description }}</p>
      <p class="price">{{ item.price | format }} руб.</p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'FormPage',

  filters: {
    format: val => `${val}`.replace(/(\d)(?=(\d{3})+([^\d]|$))/g, '$1 ')
  },

  props: {
    products: {
      type: Array,
      required: true
    }
  },

  data () {
    return {
      active: false
    }
  },

  methods: {
    removeProduct (i) {
      this.$emit('remove', i)
    },
    buttonHover (i) {
      this.$emit('hover', i)
    }
  }
}

</script>

<style scoped>
.grid-wrapper {
  margin-left: 24.8vw;
}
.grid {
  display: inline-table;
  position: relative;
  width: 20%;
  min-width: 20vw;
  min-height: 28vw;
  background: rgb(250, 250, 250);
  margin: 0 0.6vw 1.4vw 0.6vw;
  border-radius: 0.5vw;
  box-shadow: 0.1vw 0.5vw 1vw 0vw rgb(230, 230, 230);
  transition: all ease .5s;
  cursor: pointer;
}
.grid:hover {
  box-shadow: 0.1vw 0.5vw 1vw 0vw rgb(163, 163, 163);
  transition: all ease .5s;
  }
h3, p {
  position: absolute;
}
.description {
  bottom: 3.6vw;
  hyphens: auto;
}
.price {
  bottom: 0.5vw;
  font-weight: bold;
}
.image {
  width: 22.99vw;
  min-width: 22.8vw;
  border-top-left-radius: 0.5vw;
  border-top-right-radius: 0.5vw;

}
.button {
    position: absolute;
    width: 3vw;
    left: 20.7vw;
    bottom: 26vw;
    cursor: pointer;
}
.btn-fade-enter-active {
  animation: btn-fade-in .5s;
}
.btn-fade-leave-active {
  animation: btn-fade-in .5s reverse;
}
@keyframes btn-fade-in {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}
h3 {
  margin-left: 1vw;
}
p {
  margin-left: 1vw;
}

/* MEDIA */
/* tablet */
@media (min-width: 768px) and (max-width: 1024px) {
  .grid-wrapper {
    margin-left: 31vw;
  }
  .grid {
    height: 34vw;
    margin: 0 1vw 2vw 1vw;
  }
  .image {
    width: 31vw;
  }
  .button {
    width: 3vw;
    left: 28.7vw;
    bottom: 32vw;
}
}
@media (min-width: 1024px) and (max-width: 1240px)  {

}
/* monitors and laptops */
@media (min-width: 1240px)  {

}
/* mobile */
@media (max-width: 768px) {
  .grid-wrapper {
    margin-left: 30.5vw;
  }
  .grid {
    height: 34vw;
    margin: 0 0.5vw 2vw 0.5vw;
  }
  .image {
    width: 30vw;
  }
  .button {
    width: 3vw;
    left: 27.5vw;
    bottom: 32vw;
}
}
</style>
