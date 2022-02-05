<template>
  <div>
    <div class="grid-wrapper">
      <div v-for="(item, index) in products" :key="index" class="grid">
        <img width="375px" :src="item.imgSrc">
        <h3>{{ item.name }}</h3>
        <p>{{ item.description }}</p>
        <p>{{ item.price }}</p>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'NuxtTutorial',

  props: {
    products: {
      type: Array,
      required: true
    }
  }
}

</script>

<style scoped>
.grid-wrapper {
  position: absolute;
  margin-left: 350px;
}
.grid {
  display: inline-table;
  width: 25%;
  background: rgb(250, 250, 250);
  margin: 20px 5px 20px 5px;
}
h3 {
  margin-left: 10px;
}
p {
  margin-left: 10px;
}
</style>
