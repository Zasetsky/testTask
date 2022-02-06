<template>
  <div class="grid-wrapper">
    <div v-for="(item, index) in products" :key="index" class="grid" @mouseover="item.isActive = true" @mouseleave="item.isActive = false">
      <transition name="btn-fade">
        <img
          v-show="item.isActive"
          class="button"
          width="45px"
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
  margin-left: 371px;
}
.grid {
  display: inline-table;
  position: relative;
  width: 20%;
  min-width: 350px;
  min-height: 412px;
  background: rgb(250, 250, 250);
  margin: 0 11px 20px 11px;
  border-radius: 5px;
  box-shadow: 3px 3px 10px 3px rgb(219, 219, 219);
  cursor: pointer;
}
.grid:hover {
  box-shadow: 5px 5px 5px rgb(187, 187, 187);
  }
h3, p {
  position: absolute;
}
.description {
  bottom: 50px;
  hyphens: auto;
}
.price {
  bottom: 5px;
}
.image {
  width: 350px;
  max-height: 200px;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;

}
.button {
    position: absolute;
    left: 315px;
    bottom: 385px;
    cursor: pointer;
    border-radius: 5px;
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
  margin-left: 10px;
}
p {
  margin-left: 10px;
}
</style>
