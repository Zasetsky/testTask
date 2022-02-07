<template>
  <div>
    <div class="header-wrapper">
      <h1 class="header">Добавление товара</h1>
      <select v-model="sortBy" class="sorter">
        <option value="По умолчанию">По умолчанию</option>
        <option value="name">По имени</option>
        <option value="price-increase">По возрастанию цены</option>
        <option value="price-decrease">По убыванию цены</option>
      </select>
    </div>
    <div v-scroll="handleScroll" class="product-form" :class="{active: isActive}">
      <form @submit.prevent="addProduct()">
        <img class="required-img name" src="../data/images/red-dot.png">
        <label for="name" class="input-description">Наименование товара</label>
        <input
          id="name"
          v-model="inputName"
          class="input"
          :class="{active: isErrorName}"
          type="text"
          :maxlength="limitName"
          placeholder="Введите наименование товара"
        >
        <p v-if="isErrorName" class="error">Поле является обязательным</p>
        <label for="description" class="input-description textarea">Описание товара</label>
        <textarea
          id="description"
          v-model="inputDesc"
          class="input-desc"
          type="text"
          placeholder="Введите описание товара"
          :maxlength="limitDesc"
        />
        <img class="required-img link" src="../data/images/red-dot.png">
        <label for="link" class="input-description">Ссылка на изображение товара</label>
        <input
          id="link"
          v-model="inputLink"
          class="input"
          :class="{active: isErrorLink}"
          type="url"
          placeholder="Введите ссылку"
        >
        <p v-if="isErrorLink" class="error">Поле является обязательным</p>
        <img class="required-img price" src="../data/images/red-dot.png">
        <label for="price" class="input-description" >Цена товара</label>
        <input
          id="price"
          v-model="inputPrice"
          class="input"
          :class="{active: isErrorPrice}"
          type="number"
          min="0"
          placeholder="Введите цену товара"
        >
        <p v-if="isErrorPrice" class="error">Поле является обязательным</p>
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
      isActive: false,
      limitDesc: 160,
      limitName: 30,
      showErrors: false,
      sortBy: 'По умолчанию',
      inputName: '',
      inputDesc: '',
      inputLink: '',
      inputPrice: ''
    }
  },

  computed: {
    maxId () {
      let max = 0
      for (let i = 0; i < this.availableProducts.length; i++) {
        if (this.availableProducts[i].id > max) {
          max = this.availableProducts[i].id
        }
      }
      return max
    },

    isFilled () {
      return this.inputName && this.inputLink && this.inputPrice
    },

    isErrorName () {
      return !this.inputName && this.showErrors
    },

    isErrorLink () {
      return !this.inputLink && this.showErrors
    },

    isErrorPrice () {
      return !this.inputPrice && this.showErrors
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
      } else if (this.sortBy === 'По умолчанию') {
        products = products.sort((a, b) => {
          return a.id - b.id
        })
      }
      return products
    }
  },

  methods: {
    addProduct () {
      this.showErrors = true
      if (!this.isErrorName && !this.isErrorLink && !this.isErrorPrice) {
        this.availableProducts.push(
          {
            id: this.maxId + 1,
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
        this.showErrors = false
      }
      console.log(this.availableProducts)
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
  font-size: 1.2vw;
  font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif ;
  color: rgba(0, 0, 0, 0.747);
  scroll-behavior: smooth;
}
body {
  background: rgb(240, 240, 240);
}

  /* Form */

.header-wrapper {
    display: flex;
    justify-content: space-between;
}
.header {
  margin-left: 1.2vw;
}
.product-form {
  float: left;
  position: fixed;
  margin-left: 1vw;
  box-shadow: 1px 5px 10px 0px rgb(236, 236, 236);
  align-items: center;
}
.product-form:not(.active) {
  transition: all ease 0.2s;
}
.product-form.active {
  top: 1vw;
  transition: all ease 0.2s;
}
.product-form, form {
  display: flex;
  flex-direction: column;
  background: rgb(255, 255, 255);
  border-radius: 0.5vw;
  padding: 0.7vw;
}
::-webkit-input-placeholder {
              color: #0000004f;
            }
input, textarea {
  outline: none;
  border: none;
  font-size: 0.9vw;
  border-radius: 0.4vw;
  box-shadow: 0.1vw 0.5vw 1vw 0vw rgb(230, 230, 230);
  font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif ;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
}
.input-desc {
    resize: none;
    padding: 0.7vw;
    height: 6vw;
}
.input-desc:focus {
  box-shadow: 0.1vw 0.5vw 1vw 0vw rgb(163, 163, 163);
  transition: all ease .5s;
}
.input {
  padding: 0.7vw;
  width: 19vw;
}
.input:focus {
  box-shadow: 0.1vw 0.5vw 1vw 0vw rgb(163, 163, 163);
  transition: all ease .5s;
}
.input.active {
  border: 2px solid rgba(255, 0, 0, 0.349);
}
.input-description {
  font-size: 0.75vw;
  color:rgba(0, 0, 0, 0.658);
  margin-top: 0.7vw;
}
.input-description.textarea {
  margin-top: 1.4vw;
}
.error {
  font-size: 9px;
  margin: 5px 0 -5px 0;
  color: rgba(255, 0, 0, 0.582)
}
.required-img {
  width: 0.3vw;
  opacity: 50%;
}
.required-img.name {
  margin-bottom: -1vw;
  margin-left: 7.6vw;
}
.required-img.link {
  margin-bottom: -1vw;
  margin-left: 10.9vw;
  margin-top: 1.4vw;
}
.required-img.price {
  margin-bottom: -1vw;
  margin-left: 4.2vw;
  margin-top: 1.4vw;
}
.add-button {
  padding: 0.65vw;
  border: none;
  border-radius: 0.6vw;
  margin-top: 1.3vw;
  color: rgba(0, 0, 0, 0.219);
  font-weight: bold;
  font-size: 0.9vw;
}
.add-button.active {
  color: white;
  background-color: rgba(0, 128, 0, 0.541);
  cursor: pointer;
}

/* Sorter */

.sorter {
  float: right;
  width: 10vw;
  text-align: center;
  height: 2.9vw;
  outline: none;
  border: none;
  border-radius: 0.5vw;
  box-shadow: 0.1vw 0.5vw 1vw 0vw rgb(230, 230, 230);
  color: rgba(0, 0, 0, 0.349);
  cursor: pointer;
  font-size: 0.9vw;
  margin: 1vw 1vw 0 0;
}

.sorter:focus {
  box-shadow: 0.1vw 0.5vw 1vw 0vw rgb(163, 163, 163);
}

/* MEDIA */
/* tablet */
@media (min-width: 768px) and (max-width: 1024px) {
  .sorter{
    margin: 1vw 2vw 0 0;
  }
  .input {
    height: 2vw;
    width: 24.5vw;
  }
  .add-button {
    padding: 1vw;
    margin-top: 2vw;
  }
}
@media (min-width: 1024px) and (max-width: 1240px)  {
  .sorter{
    margin: 1vw 2.5vw 0 0;
  }
  .product-form {
    margin-top: 0;
  }
}
/* monitors and laptops */
@media (min-width: 1240px)  {
  .sorter{
    margin: 1.5vw 3.2vw 0 0;
  }
  .product-form {
    margin-top: 0;
    margin-left: 1vw;
  }
}
/* mobile */
@media (max-width: 768px) {
  .sorter{
    margin: 1vw 4vw 0 0;
  }
  .input {
    height: 2vw;
    width: 24.5vw;
  }
  .add-button {
    padding: 1vw;
    margin-top: 2vw;
  }
}
</style>
