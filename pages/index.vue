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
        <label for="name" class="input-description">Наименование товара</label>
        <input
          id="name"
          v-model="inputName"
          class="input"
          :class="{active: isErrors}"
          type="text"
          placeholder="Введите наименование товара"
        >
        <p v-if="isErrors" class="error">Поле является обязательным</p>
        <label for="description" class="input-description textarea">Описание товара</label>
        <textarea
          id="description"
          v-model="inputDesc"
          class="input-desc"
          type="text"
          placeholder="Введите описание товара"
          :maxlength="limit"
        />
        <img class="required-img link" src="../data/images/red-dot.png">
        <label for="link" class="input-description">Ссылка на изображение товара</label>
        <input
          id="link"
          v-model="inputLink"
          class="input"
          :class="{active: isErrors}"
          type="url"
          placeholder="Введите ссылку"
        >
        <p v-if="isErrors" class="error">Поле является обязательным</p>
        <img class="required-img price" src="../data/images/red-dot.png">
        <label for="price" class="input-description" >Цена товара</label>
        <input
          id="price"
          v-model="inputPrice"
          class="input"
          :class="{active: isErrors}"
          type="number"
          min="0"
          placeholder="Введите цену товара"
        >
        <p v-if="isErrors" class="error">Поле является обязательным</p>
        <button
          :class="{active: isFilled}"
          class="add-button"
          type="submit">Добавить товар
        </button>
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
      availableProducts,
      value: 0,
      isErrors: false,
      isActive: false,
      limit: 120,
      sortBy: 'По умолчанию',
      inputName: '',
      inputDesc: '',
      inputLink: '',
      inputPrice: ''
    }
  },

  computed: {
    isFilled () {
      return this.inputName && this.inputLink && this.inputPrice
    },

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
      if (this.inputName && this.inputLink && this.inputPrice) {
        this.isErrors = false
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
      } else {
        this.isErrors = true
      }
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
  color: rgba(0, 0, 0, 0.747);
}
body {
  background: rgb(238, 238, 238);
}
.header-wrapper {
    display: flex;
    justify-content: space-between;
}
.header {
  margin-left: 30px;
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
.input:focus {
  box-shadow: 1px 5px 10px 0px rgb(211, 211, 211);
  transition: all ease .5s;
}
.input.active {
  border: 2px solid rgba(255, 0, 0, 0.349);
}
.input-description {
  font-size: 12px;
  color:rgba(0, 0, 0, 0.658);
  margin-top: 10px;
}
.input-description.textarea {
  margin-top: 20px;
}
.error {
  font-size: 9px;
  margin: 5px 0 -5px 0;
  color: rgba(255, 0, 0, 0.582)
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
  margin-top: 20px;
}
.required-img.price {
  margin-bottom: -13px;
  margin-left: 69px;
  margin-top: 20px;
}
.add-button {
  padding: 10px;
  border: none;
  border-radius: 10px;
  margin-top: 20px;
  color: rgba(0, 0, 0, 0.219);
  font-weight: bold;
}
.add-button.active {
  color: white;
  background-color: rgba(0, 128, 0, 0.541);
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
    padding: 10px;
    height: 90px;
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

</style>
