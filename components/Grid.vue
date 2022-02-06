<template>
  <div class="grid-wrapper">
    <div v-for="(item, index) in products" :key="index" class="grid" @mouseover="item.isActive = true" @mouseleave="item.isActive = false">
      <img v-show="item.isActive" class="button" width="35px" src="../data/images/pngwing.com.png" @click="removeProduct(index)">
      <img width="375px" :src="item.imgSrc">
      <h3>{{ item.name }}</h3>
      <p>{{ item.description }}</p>
      <p>{{ item.price }}</p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'FormPage',

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
  position: absolute;
  margin-left: 360px;
}
.grid {
  display: inline-table;
  width: 25%;
  background: rgb(250, 250, 250);
  margin: 20px 5px 20px 5px;
}
.button {
    position:absolute;
    cursor: pointer;
}
h3 {
  margin-left: 10px;
}
p {
  margin-left: 10px;
}
</style>
