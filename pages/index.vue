<template>
  <div>
    <div class="product-form">
      <h1>Добавление товаров</h1>
      <form  @submit.prevent="addProduct()">
        <p>Наименование товара</p>
        <input v-model="inputName" type="text" placeholder="Введите наименование товара" required>
        <p>Описание товара</p>
        <input v-model="inputDesc" type="text" placeholder="Введите описание товара">
        <p>Ссылка на изображение товара</p>
        <input v-model="inputLink" type="url" placeholder="Введите ссылку на изображение товара" required>
        <p>Цена товара</p>
        <input v-model="inputPrice" type="number" placeholder="Введите цену товара" required>
        <button type="submit">Добавить товар</button>
      </form>
    </div>
    <Grid />
  </div>
</template>

<script>

export default {
  name: 'IndexPage',

  data () {
    return {
      addedProducts: [],
      inputName: '',
      inputDesc: '',
      inputLink: '',
      inputPrice: ''
    }
  },

  methods: {
    addProduct () {
      this.addedProducts.push(
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
      console.log(this.addedProducts)
    }
  }
}
</script>

<style scoped>
.product-form {
  float: left;
}
.product-form form {
  display: flex;
  flex-direction: column;
  background: rgb(226, 226, 226)
}
</style>
