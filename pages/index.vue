<template>
  <div>
    <div class="product-form">
      <form  @submit.prevent="addProduct()">
        <p>Наименование товара</p>
        <input class="input" v-model="inputName" type="text" placeholder="Введите наименование товара" required>
        <p>Описание товара</p>
        <input class="input-desc" v-model="inputDesc" type="text" placeholder="Введите описание товара">
        <p>Ссылка на изображение товара</p>
        <input class="input" v-model="inputLink" type="url" placeholder="Введите ссылку на изображение товара" required>
        <p>Цена товара</p>
        <input class="input" v-model="inputPrice" type="number" placeholder="Введите цену товара" required>
        <button class="button" type="submit">Добавить товар</button>
      </form>
    </div>
    <Grid :products="availableProducts" />
  </div>
</template>

<script>
import availableProducts from '../data/products'

export default {
  name: 'IndexPage',

  data () {
    return {
      availableProducts,
      inputName: '',
      inputDesc: '',
      inputLink: '',
      inputPrice: ''
    }
  },

  methods: {
    addProduct () {
      this.availableProducts.push(
        {
          name: this.inputName,
          description: this.inputDesc,
          imgSrc: this.inputLink,
          price: this.inputPrice
        })
      this.inputName = ''
      this.inputDesc = ''
      this.inputLink = ''
      this.inputPrice = ''
      console.log(this.availableProducts)
    }
  }
}
</script>

<style>
html {
  background: rgb(238, 238, 238);
  font-size: 18px;
  font-family: -apple-system, BlinkMacSystemFont, sans-serif;
}
.product-form {
  float: left;
  position: fixed;
}
.product-form form {
  position: relative;
  display: flex;
  flex-direction: column;
  background: rgb(255, 255, 255);
  padding: 40px;
  margin: 20px 0 0 0;
}
.input {

}
</style>
