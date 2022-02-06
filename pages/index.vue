<template>
  <div>
    <div class="header-wrapper">
      <h1 class="header">Добавление товара</h1>
      <div class="sorter-wrapper">
        <select v-model="sortBy" class="sorter">
          <option disabled>По умолчанию</option>
          <option value="name">По имени</option>
          <option value="price-increase">По возрастанию цены</option>
          <option value="price-decrease">По убыванию цены</option>
        </select>
      </div>
    </div>
    <div class="product-form">
      <form @submit.prevent="addProduct()">
        <p>Наименование товара</p>
        <input v-model="inputName" class="input" type="text" placeholder="Введите наименование товара" required>
        <p>Описание товара</p>
        <textarea v-model="inputDesc" class="input-desc" type="text" placeholder="Введите описание товара" :maxlength="limit" />
        <p>Ссылка на изображение товара</p>
        <input v-model="inputLink" class="input" type="url" placeholder="Введите ссылку на изображение товара" required>
        <p>Цена товара</p>
        <input v-model="inputPrice" class="input" type="number" placeholder="Введите цену товара" required>
        <button class="button" type="submit">Добавить товар</button>
      </form>
    </div>
    <Grid :products="sortedArray" @remove="i => removeProduct(i)" />
  </div>
</template>

<script>
import availableProducts from '../data/products'

export default {
  name: 'IndexPage',

  data () {
    return {
      limit: 120,
      sortBy: 'По умолчанию',
      availableProducts,
      inputName: '',
      inputDesc: '',
      inputLink: '',
      inputPrice: ''
    }
  },

  computed: {
    sortedArray () {
      let products = this.availableProducts
      if (this.sortBy === 'name') {
        products = products.sort((a, b) => {
          const fa = a.name.toLowerCase()
          const fb = b.name.toLowerCase()
          if (fa < fb) {
            return -1
          }
          if (fa > fb) {
            return 1
          } else {
            return 0
          }
        })
      } else if (this.sortBy === 'price-decrease') {
        products = products.sort((a, b) => {
          return b.price - a.price
        })
      } else if (this.sortBy === 'price-increase') {
        products = products.sort((a, b) => {
          return a.price - b.price
        })
      }
      return products
    }
  },

  methods: {
    addProduct () {
      this.availableProducts.push(
        {
          name: this.inputName,
          description: this.inputDesc,
          imgSrc: this.inputLink,
          price: this.inputPrice,
          isActive: false
        })
      this.inputName = ''
      this.inputDesc = ''
      this.inputLink = ''
      this.inputPrice = ''
    },

    removeProduct (index) {
      if (index !== -1) {
        this.availableProducts.splice(index, 1)
      }
    }
  }
}
</script>

<style>
html {
  font-size: 18px;
  font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif ;
}
body {
  background: rgb(238, 238, 238);
}
.header-wrapper {
    display: flex;
    justify-content: space-between;
}
.product-form {
  float: left;
  position: fixed;
}
textarea {
    resize: none;
    height: 100px;
}
.product-form, form {
  position: relative;
  display: flex;
  flex-direction: column;
  background: rgb(255, 255, 255);
  padding: 20px;
}
.sorter-wrapper {
  margin: 50px 20px 5px 0;
}
.sorter {
  float: right;
  width: 150px;
  height: 30px;
}
</style>
