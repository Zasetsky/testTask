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
    <div v-scroll="handleScroll" class="product-form" :class="{active: isActive}">
      <form @submit.prevent="addProduct()">
        <img class="required-img name" src="../data/images/red-dot.png">
        <p class="input-description">Наименование товара</p>
        <input v-model="inputName" class="input" type="text" placeholder="Введите наименование товара" required>
        <p class="input-description">Описание товара</p>
        <textarea
          v-model="inputDesc"
          class="input-desc"
          type="text"
          placeholder="Введите описание товара"
          :maxlength="limit"
        />
        <img class="required-img link" src="../data/images/red-dot.png">
        <p class="input-description">Ссылка на изображение товара</p>
        <input v-model="inputLink" class="input" type="url" placeholder="Введите ссылку" required>
        <img class="required-img price" src="../data/images/red-dot.png">
        <p class="input-description" >Цена товара</p>
        <input v-model="inputPrice" class="input" type="number" placeholder="Введите цену товара" required>
        <button :disabled="required" class="add-button" type="submit">Добавить товар</button>
      </form>
    </div>
    <Grid :products="sortedArray" @remove="i => removeProduct(i)" />
  </div>
</template>

<script>
import availableProducts from '../data/products'

export default {
  name: 'IndexPage',

  directives: {
    scroll: {
      inserted (el, binding) {
        const onScrollCallback = binding.value
        window.addEventListener('scroll', () => onScrollCallback())
      }
    }
  },

  data () {
    return {
      value: 0,
      isActive: false,
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
    },
    handleScroll (evt, el) {
      this.value = window.scrollY
      if (this.value > 50) {
        this.isActive = true
      } else {
        this.isActive = false
      }
    }
  }
}
</script>

<style>
html {
  font-size: 17px;
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
  top: 94px;
  left: 20px;
  box-shadow: 1px 5px 10px 0px rgb(236, 236, 236);
  align-items: center;
}
input, textarea {
  outline: none;
  border: none;
  border-radius: 5px;
  box-shadow: 1px 5px 10px 0px rgb(236, 236, 236);
}
.input {
  padding: 10px;
  width: 300px;
}
.input-description {
  font-size: 12px;
  color:rgba(0, 0, 0, 0.658);
  margin-top: 10px;
}
.required-img {
  width: 5px;
  opacity: 50%;
}
.required-img.name {
  margin-bottom: -13px;
  margin-left: 122px;
}
.required-img.link {
  margin-bottom: -13px;
  margin-left: 175px;
  margin-top: 10px;
}
.required-img.price {
  margin-bottom: -13px;
  margin-left: 69px;
  margin-top: 10px;
}
.product-form:not(.active) {
  transition: all ease 0.2s;
}
.product-form.active {
  top: 20px;
  transition: all ease 0.2s;
}
textarea {
    resize: none;
    height: 100px;
}
.product-form, form {
  display: flex;
  flex-direction: column;
  background: rgb(255, 255, 255);
  border-radius: 5px;
  padding: 10px;
}
.sorter-wrapper {
  margin: 50px 20px 5px 0;
}
.sorter {
  float: right;
  width: 150px;
  height: 30px;
}
.add-button {
  padding: 10px;
}
</style>
