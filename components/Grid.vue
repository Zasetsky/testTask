<template>
  <div>
    <div class="grid-wrapper">
      <!-- <div class="filter-wrapper">
        <select class="filter" v-model="selected">
          <option disabled value="По умолчанию">По умолчанию</option>
          <option>По имени</option>
          <option>По возрастанию цены</option>
          <option>По убыванию цены</option>
        </select>
      </div> -->
      <div v-for="(item, index) in products" :key="index" class="grid">
        <button @click="removeProduct(index)" />
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
  name: 'FormPage',

  props: {
    products: {
      type: Array,
      required: true
    }
  },

  methods: {
    removeProduct (i) {
      this.$emit('remove', i)
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
.filter-wrapper {
  padding: 35px;
}
.filter {
  float: right;
  width: 150px;
  height: 30px;
}
button {
}
h3 {
  margin-left: 10px;
}
p {
  margin-left: 10px;
}
</style>
