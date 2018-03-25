<template>
  <div class="animated fadeIn">
    <b-row>
      <b-col md="6">
        <b-card>
          <div slot="header">
            <strong>Описание</strong>
            <button style="float: right;" @click="makeFakeProduct">full +</button>
            <button style="float: right;" @click="makeFakeProduct1">half +</button>
            <button style="float: right;" @click="makeFakeProduct2">empty +</button>
          </div>

          <!-- Name -->
          <b-form-fieldset
            description="наименовние для товара"
            label="Наименование"
            :label-cols="3"
            :horizontal="true">
            <b-form-input v-model="form_descr.name" type="text"></b-form-input>
          </b-form-fieldset>

          <!-- Price -->
          <b-form-fieldset
            description="стоимость товара (грн.)"
            label="Цена"
            :label-cols="3"
            :horizontal="true">
            <b-form-input v-model="form_descr.price.current" type="text"></b-form-input>
          </b-form-fieldset>

          <!-- Material2 -->
          <b-form-fieldset
            description="материал товара"
            label="Материал"
            :label-cols="3"
            :horizontal="true"
          >
            <div>
              <multiselect v-model="form_descr.textile.value" :options="form_descr.textile.options" :multiple="true" :close-on-select="false" :clear-on-select="false" :hide-selected="true" :preserve-search="true" placeholder="" label="name" track-by="id">
                <template slot="tag" slot-scope="props">
                  <span class="custom__tag">
                    <span>{{ props.option.name }}</span>
                  <span class="custom__remove" @click="props.remove(props.option)">❌</span>
                </span>
              </template>
              </multiselect>
            </div>
          </b-form-fieldset>

          <!-- Gender -->
          <b-form-fieldset
            label="Пол"
            :label-cols="3"
            :horizontal="true">
            <b-form-radio-group
              v-model="form_descr.gender.active"
              :options="form_descr.gender.options">
            </b-form-radio-group>
          </b-form-fieldset>

          <!-- TypeOfProduct -->
          <b-form-fieldset
            label="Тип"
            :label-cols="3"
            :horizontal="true">
            <b-form-radio-group
              v-model="form_descr.type.active"
              :options="form_descr.type.options[form_descr.gender.active]">
            </b-form-radio-group>
          </b-form-fieldset>

          <!-- Size -->
          <b-form-fieldset
            label="Размеры"
            :label-cols="3"
            :horizontal="true">

           <!-- Traditional sizes -->
            <div v-if="form_descr.gender.active !== 'Child'">
              <b-form-checkbox
                v-model="form_descr.sizes.traditional.allSelected"
                :indeterminate="form_descr.sizes.traditional.indeterminate"
                aria-describedby="form_descr.sizes.traditional.data"
                aria-controls="form_descr.sizes.traditional.data"
                @change="toggleAllSizeTraditional"
                :disabled="form_descr.sizes.noSize"
              >
                {{ form_descr.sizes.traditional.allSelected ? 'Отменить все' : 'Выбрать все' }}
              </b-form-checkbox>
              <b-form-checkbox
                v-model="form_descr.sizes.noSize"
                aria-describedby="form_descr.sizes.traditional.data"
                aria-controls="form_descr.sizes.traditional.data"
                @change="toggleAllSizeDisabled"
              >
                Нет размера
              </b-form-checkbox>
              <b-form-checkbox-group
                :disabled="form_descr.sizes.noSize"
                id="flavors"
                stacked
                v-model="form_descr.sizes.traditional.selected"
                name="flavs"
                :options="form_descr.sizes.traditional.data"
                class="ml-4"
                aria-label="Traditional sizes"
              ></b-form-checkbox-group>
            </div>

          <!-- Kids sized -->
            <div v-if="form_descr.gender.active === 'Child'">
              <b-form-checkbox
                :disabled="form_descr.sizes.noSize"
                v-model="form_descr.sizes.kids.allSelected"
                :indeterminate="form_descr.sizes.kids.indeterminate"
                aria-describedby="form_descr.sizes.kids.data"
                aria-controls="form_descr.sizes.kids.data"
                @change="toggleAllSizeKids"
              >
                {{ form_descr.sizes.kids.allSelected ? 'Отменить все' : 'Выбрать все' }}
              </b-form-checkbox>
             <b-form-checkbox
                v-model="form_descr.sizes.noSize"
                aria-describedby="form_descr.sizes.kids.data"
                aria-controls="form_descr.sizes.kids.data"
                @change="toggleAllSizeDisabled"
              >
                Нет размера
              </b-form-checkbox>
              <b-form-checkbox-group
                :disabled="form_descr.sizes.noSize"
                id="flavors"
                stacked
                v-model="form_descr.sizes.kids.selected"
                name="flavs"
                :options="form_descr.sizes.kids.data"
                class="ml-4"
                aria-label="Kids sizes"
              ></b-form-checkbox-group>
            </div>
          </b-form-fieldset>

          <!-- Tags -->
          <b-form-fieldset
            description="теги для товара (через запятую)"
            label="Теги"
            :label-cols="3"
            :horizontal="true">
            <b-form-input v-model="form_descr.tags" placeholder="Цветы, Микки Маус, Танки, Нежность" type="text"></b-form-input>
          </b-form-fieldset>

          <!-- Description -->
          <b-form-fieldset
            label="Описание"
            description="описание товара"
            :label-cols="3"
            :horizontal="true">
            <b-textarea
              v-model="form_descr.description"
              :textarea="true"
              :rows="3"
              :max-rows="12"
            ></b-textarea>
          </b-form-fieldset>

          <!-- Model -->
          <b-form-fieldset
            description="модель/артикул товара (чужой)"
            label="Модель"
            :label-cols="3"
            :horizontal="true">
            <b-form-input v-model="form_descr.model" type="text"></b-form-input>
          </b-form-fieldset>

          <!-- Manufacturer -->
          <b-form-fieldset
            label="Поставщик"
            description="откуда взят товар"
            :label-cols="3"
            :horizontal="true">
            <b-form-input v-model="form_descr.manufacturer" type="text"></b-form-input>
          </b-form-fieldset>

          <!-- ImagesPC -->
          <b-form-fieldset
            label="Изобр-я с ПК"
            :label-cols="3"
            :horizontal="true">
              <b-form-file @change.native="previewFileFromPC" v-model="form_descr.images.PC" choose-label="Тыц" accept="image/*" :multiple="true">
              </b-form-file>
          </b-form-fieldset>

          <!-- ImagesWEB -->
          <b-form-fieldset
            description="изображения товара с web"
            label="Изобр-я с WEB"
            :label-cols="3"
            :horizontal="true">
            <b-input-group>
              <b-form-input v-for="(image, index) in form_descr.images.WEB" :key="image" type="text" v-model="form_descr.images.WEB[index]"></b-form-input>
              <b-input-group-button slot="right">
                <b-button @click="addInputForWEBimage" variant="primary">+</b-button>
              </b-input-group-button>
            </b-input-group>
          </b-form-fieldset>


          <!-- Labels -->
          <b-form-fieldset
            label="Метки"
            :label-cols="3"
            :horizontal="true">
            <div>
              <b-form-checkbox-group
                id="flavors"
                stacked
                v-model="form_descr.labels.selected"
                name="flavs"
                :options="form_descr.labels.data"
                class="ml-4"
                aria-label="Traditional sizes"
              ></b-form-checkbox-group>
               <b-form-input v-if="form_descr.labels.selected.includes('Распродажа')" v-model="form_descr.price.discount" type="text" placeholder="Размер скидки, %"></b-form-input>
               <div v-if="form_descr.labels.selected.includes('Распродажа')">{{form_descr.price.current}} - {{form_descr.price.current * form_descr.price.discount/100}} = {{ Math.ceil(form_descr.price.current - (form_descr.price.current * form_descr.price.discount/100)) }}</div>
            </div>
          </b-form-fieldset>

          <div slot="footer" class="text-center">
            <b-button type="submit" @click="submit" size="sm" variant="primary"><i class="fa fa-dot-circle-o"></i> Submit</b-button>
            <b-button type="reset" size="sm" variant="danger"><i class="fa fa-ban"></i> Reset</b-button>
          </div>
        </b-card>
      </b-col>

      <b-col md="6">
        <b-card>
          <div slot="header">
            <strong>Превью</strong>
          </div>
          <div class="box-product-outer">
            <div class="box-product">
              <div class="img-wrapper">
                <a>
                  <img id="preview" alt="Product" :src="form_descr.images.WEB[0] || form_descr.images.noImage">
                </a>
                <div class="tags tags-left">
                  <span v-if="form_descr.labels.selected.includes('Новинка')" class="label-tags"><span class="label label-success arrowed-right">Новинка</span></span>
                  <span v-if="form_descr.labels.selected.includes('Распродажа')" class="label-tags"><span class="label label-danger arrowed-right">Распродажа</span></span>
                </div>
                <div class="tags">
                  <span v-if="form_descr.labels.selected.includes('Коллекция')" class="label-tags"><span class="label label-info arrowed">Коллекция</span></span>
                  <span v-if="form_descr.labels.selected.includes('Популярное')" class="label-tags"><span class="label label-primary arrowed">Популярное</span></span>
                  <span v-if="form_descr.labels.selected.includes('Товар недели')" class="label-tags"><span class="label label-default arrowed">Товар недели</span></span>
                </div>
                <div class="option" style="pointer-events: none">
                  <a data-toggle="tooltip" title="Add to Cart"><i class="fa fa-shopping-cart"></i></a>
                  <a data-toggle="tooltip" title="Add to Compare"><i class="fa fa-align-left"></i></a>
                  <a data-toggle="tooltip" title="Add to Wishlist" class="wishlist"><i class="fa fa-heart"></i></a>
                </div>
              </div>
              <h6 v-if="form_descr.name"><a>{{ form_descr.name || 'Введите название товара' }}</a></h6>
              <div class="price">
                <!-- Price without discount -->
                <div v-if="form_descr.price.current && !form_descr.labels.selected.includes('Распродажа')">{{ form_descr.price.current || 0 }}грн. <span v-if="form_descr.labels.selected.includes('Распродажа')" class="label-tags"><span class="label label-danger arrowed">-{{ form_descr.price.discount }}%</span></span></div>

                <!-- Price WITH discount -->
                <div v-if="form_descr.price.discount > 0 && form_descr.labels.selected.includes('Распродажа')">{{ Math.ceil(form_descr.price.current - (form_descr.price.current * form_descr.price.discount/100)) || 0 }}грн. <span v-if="form_descr.labels.selected.includes('Распродажа')" class="label-tags"><span class="label label-danger arrowed">-{{ form_descr.price.discount }}%</span></span></div>

                <span v-if="form_descr.labels.selected.includes('Распродажа')" class="price-old">
                  {{ form_descr.price.current }}грн.
                </span>
              </div>
            </div>
          </div>

          <div slot="footer" class="text-center">
            <!-- <b-button type="submit" size="sm" variant="primary"><i class="fa fa-dot-circle-o"></i> Submit</b-button>
            <b-button type="reset" size="sm" variant="danger"><i class="fa fa-ban"></i> Reset</b-button> -->
          </div>
        </b-card>
      </b-col>
        <!-- <div>src: (http://vilno.com.ua/data.html) http://vilno.com.ua/csv/export-06032017.csv</div> -->

        <!-- <b-form-input v-model="csvload" type="text" placeholder="CSV load"></b-form-input> -->
        <div>
          <!-- <div>src: (http://garda.com.ua/data/) http://garda.com.ua/wp-content/uploads/exports/Garda.csv</div> -->
          <b-button type="submit" size="sm" variant="danger" @click="deleteProductsByManufac('garda.com.ua')">
            <i class="fa fa-dot-circle-o"></i> Удалить товары Gardы из Базы Данных (Будут удалены ТОЛЬКО товары, у которых Поставщик - garda.com.ua)
          </b-button>
          <br>
          <br>
          <b-button type="submit" size="sm" variant="danger" @click="deleteAllProducts()">
            <i class="fa fa-dot-circle-o"></i> Удалить ВСЕ ТОВАРЫ!
          </b-button>
          <br>
          <br>
          <b-button type="submit" size="sm" variant="primary" @click="fetchProductsSRC(csvload)">
            <i class="fa fa-dot-circle-o"></i> Загрузить в базу товары из Garda (!!! Осторожно! Если товары уже загружены, сначала нужно удалить старые!!!)
          </b-button>
        </div>
    </b-row>
  </div>
</template>

<script>
import axios from 'axios'
import Multiselect from 'vue-multiselect'

import { TYPE_OF_TEXTILE } from '../data/textile.js'

const GENDER_OPTIONS = [
  {text: 'Женский', value: 'Woman'},
  {text: 'Мужской', value: 'Man'},
  {text: 'Детский', value: 'Child'},
  {text: 'Унисекс', value: 'Unisex'}
]

const TYPE_OPTIONS = {
  Woman: ['Брюки', 'Купальники', 'Леггинсы', 'Лонгсливы', 'Майки', 'Платья', 'Свитшоты', 'Туники', 'Футболки', 'Худи и Кенги', 'Шорты', 'Юбки'],
  Man: ['Свитшоты', 'Футболки', 'Брюки'],
  Child: ['Свитшоты', 'Футболки'],
  Unisex: ['Рюкзаки']
}

export default {
  name: 'addNewProduct',
  components: {
    Multiselect
  },
  data () {
    return {
      fr: new FileReader(),
      form_descr: {
        name: '',
        price: {
          current: '',
          total: '',
          discount: 0
        },
        gender: {
          active: 'Woman',
          options: GENDER_OPTIONS
        },
        type: {
          active: 'Woman',
          options: TYPE_OPTIONS
        },
        sizes: {
          noSize: false,
          traditional: {
            data: [
              'XXS',
              'XS',
              'S',
              'M',
              'L',
              'XL',
              'XXL'
            ],
            selected: [],
            allSelected: false,
            indeterminate: false
          },
          kids: {
            data: [
              'Возраст 1 год, рост 80-86',
              'Возраст 2 года, рост 92-98',
              'Возраст 3 года, рост 98-104',
              'Возраст 4 года, рост 104-110',
              'Возраст 5 лет, рост 110-116',
              'Возраст 6 лет, рост 116-122',
              'Возраст 7 лет, рост 122-128',
              'Возраст 8 лет, рост 128-134',
              'Возраст 9-10 лет, рост 134-140',
              'Возраст 11-12 лет, рост 140-152'
            ],
            selected: [],
            allSelected: false,
            indeterminate: false
          }
        },
        tags: '',
        description: '',
        textile: {
          value: null,
          options: TYPE_OF_TEXTILE
        },
        manufacturer: '',
        model: '',
        images: {
          PC: [],
          WEB: [''],
          noImage: 'https://mimity29.netlify.com/images/demo/p1-1.jpg'
        },
        labels: {
          data: [
            'Новинка',
            'Распродажа',
            'Коллекция',
            'Популярное',
            'Товар недели'
          ],
          selected: []
        }
      },
      csvload: ''
    }
  },
  methods: {
    toggleAllSizeTraditional (checked) {
      this.form_descr.sizes.traditional.selected = checked ? this.form_descr.sizes.traditional.data.slice() : []
    },
    toggleAllSizeKids (checked) {
      this.form_descr.sizes.kids.selected = checked ? this.form_descr.sizes.kids.data.slice() : []
    },
    toggleAllSizeDisabled (checked) {
      this.form_descr.sizes.noSize = !this.form_descr.sizes.noSize
    },
    addInputForWEBimage () {
      this.form_descr.images.WEB.push('')
      console.log(this.form_descr.images.WEB.length)
    },
    previewFileFromPC () {
      var preview = document.getElementById('preview')
      var file = document.querySelector('input[type=file]').files[0]
      var reader = new FileReader()

      reader.onloadend = function () {
        preview.src = reader.result
      }

      if (file) {
        reader.readAsDataURL(file)
      } else {
        preview.src = this.form_descr.images.noImage
      }
    },
    submit () {
      console.log('submit the form')
      console.log(this.form_descr)
      console.log(axios)

      const {
        description,
        gender,
        images,
        labels,
        manufacturer,
        textile,
        model,
        name,
        sizes,
        // tags,
        type,
        price
      } = this.form_descr

      const newProduct = {
        description,
        gender: gender.active,
        images: images.WEB,
        labels: labels.selected,
        manufacturer,
        textile: textile.value ? textile.value.map((el) => el.name) : [],
        model,
        name,
        price: {
          current: price.current,
          discount: price.discount,
          total: price.discount > 0 ? Math.ceil(price.current - (price.current * price.discount / 100)) : price.current
        },
        size: gender.active === 'Child' ? sizes.kids.selected : sizes.traditional.selected,
        // tags: tags && tags.split(',').map((elem) => elem.trim()).map((elem) => elem[0].toUpperCase() + elem.slice(1)),
        type: type.active
      }

      console.log(newProduct)
      axios.post('http://localhost:4040/api/products', newProduct)
        .then(function (response) {
          console.log(response)
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    makeFakeProduct () {
      this.form_descr.name = 'Тест Наименование'
      this.form_descr.type.active = 'Футболки'
      this.form_descr.gender.active = 'Man'
      this.form_descr.description = 'Тестовое описание для тестового товара должно выглядеть примерно так'
      this.form_descr.images.WEB = ['http://vilno.com.ua/image/cache/data/t-shirts/t_back%20man%20%D0%9C%D0%B8%D1%80%20%D1%82%D0%B0%D0%BD%D0%BA%D0%BE%D0%B2-600x800.jpg']
      this.form_descr.labels.selected = ['Новинка', 'Коллекция', 'Популярное', 'Товар недели', 'Распродажа']
      this.form_descr.price = {
        current: 450,
        discount: 10
      }
      this.form_descr.textile.value = [{ id: 3, name: 'Батист' }]
      this.form_descr.model = '9379992'
      this.form_descr.manufacturer = 'Мебельный завод им. Фунтика'
      this.form_descr.sizes.traditional.selected = ['XL', 'XXL']
      this.form_descr.tags = ['Cупер футболочка', 'Лето2018', 'Война']
    },
    makeFakeProduct1 () {
      this.form_descr.name = 'Тест Наименование1'
      this.form_descr.type.active = 'Футболки'
      this.form_descr.gender.active = 'Woman'
      this.form_descr.description = 'Женскаое Тестовое описание для тестового товара должно выглядеть примерно так'
      this.form_descr.images.WEB = ['http://vilno.com.ua/image/cache/data/dress2/Dress_shirt_front_wale-600x800.jpg']
      this.form_descr.labels.selected = ['Новинка', 'Коллекция']
      this.form_descr.price = {
        current: 200,
        discount: 0
      }
      this.form_descr.textile.value = [{ id: 3, name: 'Батист' }, { id: 2, name: 'Бархат, Велюр' }, { id: 1, name: 'Байка' }]
      this.form_descr.model = '111111a'
      this.form_descr.manufacturer = 'Мебельный завод им. Дуси'
      this.form_descr.sizes.traditional.selected = ['L', 'M']
      this.form_descr.tags = ['Зима 2014', 'Мир', 'Love story']
    },
    makeFakeProduct2 () {
      this.form_descr.name = 'Тест Наименование2'
      this.form_descr.type.active = 'Футболки'
      this.form_descr.gender.active = 'Woman'
      this.form_descr.description = 'Женскаое Тестовое описание для тестового товара должно выглядеть примерно так'
      this.form_descr.images.WEB = ['http://vilno.com.ua/image/cache/data/top-breteli/mayka%20na%20korotkih%20bretelkah_FRONT%20British%20skull-525x700.jpg', 'http://vilno.com.ua/image/cache/data/top-breteli/mayka%20na%20korotkih%20bretelkah%20BACK%20British%20skull-600x800.jpg']
      this.form_descr.labels.selected = ['Новинка', 'Коллекция']
      this.form_descr.price = {
        current: 420,
        discount: 0
      }
      this.form_descr.textile.value = [{ id: 1, name: 'Байка' }]
      this.form_descr.model = '222222dd'
      this.form_descr.manufacturer = 'Мебельный завод им. Дизика'
      this.form_descr.sizes.traditional.selected = ['XXL']
      this.form_descr.tags = ['Cупер prikid', 'Лето1999', 'COffee']
    },
    fetchProductsSRC (url) {
      axios.get('http://localhost:4040/api/util/csvtojson?url=' + url)
        .then((json) => {
          console.log(json)
          alert('Товары с Гарда были добавлены!')
        })
        .catch((error) => {
          console.log(error)
        })
    },
    deleteProductsByManufac (manufac) {
      axios.delete('http://localhost:4040/api/util/deleteProductsByManufacturer?manufac=' + manufac)
        .then((json) => {
          console.log(json)
          alert(`Все товары у которых Поставщик - ${manufac} были удалены!`)
        })
        .catch((error) => {
          console.log(error)
        })
    },
    deleteAllProducts () {
      axios.delete('http://localhost:4040/api/util/deleteAllProducts')
        .then((json) => {
          console.log(json)
          alert('Все товары были удалены!')
        })
        .catch((error) => {
          console.log(error)
        })
    }
  }
}
</script>

<style src='vue-multiselect/dist/vue-multiselect.min.css'></style>
<style>
  .custom__tag {
    display: inline-block;
    padding: 3px 12px;
    background: #d2d7ff;
    margin-right: 8px;
    margin-bottom: 8px;
    border-radius: 10px;
    cursor: pointer;
  }
  .custom__remove {
    padding: 0;
    font-size: 10px;
    margin-left: 5px;
  }
  .multiselect, .multiselect__tags, .multiselect__select {
    min-height: 35px !important;
  }
  .multiselect {
    border: 1px solid #c2cfd6;
  }
  .multiselect__tags {
    border: none;
  }
  .multiselect__select {
    padding: 16px 8px;
  }
  .input-inline-90w {
    width: 86%;
    display: inline-block;
    margin-bottom: 10px;
  }
  .btn-add-input-link {
    width: 34px;
    height: 34px;
    float: right;
  }


  .box-product-outer {
    margin-bottom: 5px;
    padding: 15px;
    border: 1px solid transparent;
    border-radius: 4px;
    /* &:hover {
      @include box-shadow(0 6px 12px rgba(0, 0, 0, .175));
    } */
  }
  .box-product .img-wrapper {
    position: relative;
    overflow: hidden;
  }
  .box-product .img-wrapper > :first-child {
    position: relative;
    display: block;
  }
  .box-product .img-wrapper > a > img {
    width: 100%;
  }
  .box-product .img-wrapper .tags {
    position: absolute;
    top: 0;
    right: 0;
    display: table;
    overflow: visible;
    width: auto;
    height: auto;
    margin: 0;
    padding: 0;
    vertical-align: inherit;
    border-width: 0;
    background-color: transparent;
    direction: rtl;
  }
  .label {
    display: inline;
    padding: .2em .6em .3em;
    font-size: 75%;
    font-weight: bold;
    line-height: 1;
    color: #fff;
    text-align: center;
    white-space: nowrap;
    vertical-align: baseline;
    border-radius: .25em;
}
  .box-product .img-wrapper .tags > .label-tags {
    display: table;
    margin: 0 -1px 1px 0;
    text-align: left;
    opacity: .80;
    filter: alpha(opacity=80);
    direction: ltr;
  }
  .box-product .img-wrapper .tags > .label-tags:hover {
    opacity: 1;
    filter: alpha(opacity=100);
  }
  .box-product .img-wrapper .tags > .label-tags a:hover {
    text-decoration: none;
  }
  .box-product .img-wrapper .tags-left {
    left: 0;
    direction: ltr;
  }
  .box-product .img-wrapper > .option {
    position: absolute;
    top: auto;
    right: 0;
    bottom: -30px;
    left: 0;
    width: auto;
    height: 28px;
    -webkit-transition: all 0.2s ease;
    -o-transition: all 0.2s ease;
    transition: all 0.2s ease;
    text-align: center;
    vertical-align: middle;
    border-radius: 4px;
    background-color: rgba(0, 0, 0, 0.55);
  }
  .box-product .img-wrapper > .option > a {
    font-size: 18px;
    font-weight: normal;
    display: inline-block;
    padding: 0 4px;
    color: #fff;
  }
  .box-product .img-wrapper > .option > a:hover {
    color: #9E9E9E;
  }
  .box-product .img-wrapper > .option > a.wishlist:hover {
    color: #ef5350;
  }
  .box-product h6 a {
    font-size: 13px;
    line-height: 1.4;
    display: block;
    color: #666;
  }
  .box-product .img-wrapper > .option {
    bottom: 0;
  }

  .price {
    margin-bottom: 5px;
    color: #ef5350;
  }

  .price-old {
    position: relative;
    display: inline-block;
    margin-right: 7px;
    color: #666;
  }
  .price-old:before {
    position: absolute;
    width: 100%;
    height: 60%;
    content: '';
    border-bottom: 1px solid #666;
  }

  .rating i {
    color: #ffc107;
  }
  @media (max-width: 479px) {
    .box-product-outer {
      padding: 0 !important;
    }
    .box-product-outer:hover {
      border-color: transparent;
    }

    .box-product .img-wrapper > .option {
      bottom: 0;
    }
  }
  .label {
    font-size: 12px;
    font-weight: 400;
    line-height: 1.25;
    display: inline-block;
    height: 20px;
    border-radius: 4px;
  }
  .label.arrowed-right {
    position: relative;
    z-index: 1;
    display: inline-block;
    height: 20px;
    margin-right: 5px;
    border-top-right-radius: 1px !important;
    border-bottom-right-radius: 1px !important;
  }
  .label-danger {
      background-color: #d9534f;
  }

  .label.arrowed-right:after {
    position: absolute;
    z-index: -1;
    top: 0;
    right: -10px;
    display: inline-block;
    content: '';
    border: 1px solid transparent;
    border-width: 10px 5px;
  }

  .label-danger.arrowed-right:after {
    border-left-color: #d9534f;
  }


.label {
  font-size: 12px;
  font-weight: 400;
  line-height: 1.25;
  display: inline-block;
  height: 20px;
  border-radius: 4px;
}

.label.arrowed {
  position: relative;
  z-index: 1;
  display: inline-block;
  height: 20px;
  margin-left: 5px;
  border-top-left-radius: 1px !important;
  border-bottom-left-radius: 1px !important;
}
.label.arrowed:before {
  position: absolute;
  z-index: -1;
  top: 0;
  left: -10px;
  display: inline-block;
  content: '';
  border: 1px solid transparent;
  border-width: 10px 5px;
}

.label.arrowed-right {
  position: relative;
  z-index: 1;
  display: inline-block;
  height: 20px;
  margin-right: 5px;
  border-top-right-radius: 1px !important;
  border-bottom-right-radius: 1px !important;
}
.label.arrowed-right:after {
  position: absolute;
  z-index: -1;
  top: 0;
  right: -10px;
  display: inline-block;
  content: '';
  border: 1px solid transparent;
  border-width: 10px 5px;
}

.label-default.arrowed:before {
  border-right-color: #777;
}

.label-default.arrowed-right:after {
  border-left-color: #777;
}

.label-primary.arrowed:before {
  border-right-color: #337ab7;
}

.label-primary.arrowed-right:after {
  border-left-color: #337ab7;
}

.label-success.arrowed:before {
  border-right-color: #5cb85c;
}

.label-success.arrowed-right:after {
  border-left-color: #5cb85c;
}

.label-info.arrowed:before {
  border-right-color: #5bc0de;
}

.label-info.arrowed-right:after {
  border-left-color: #5bc0de;
}

.label-warning.arrowed:before {
  border-right-color: #f0ad4e;
}

.label-warning.arrowed-right:after {
  border-left-color: #f0ad4e;
}

.label-danger.arrowed:before {
  border-right-color: #d9534f;
}

.label-danger.arrowed-right:after {
  border-left-color: #d9534f;
}

.tooltip-inner {
  border-radius: 4px;
}

.label-success {
    background-color: #5cb85c;
}

.label-info {
    background-color: #5bc0de;
}

.label-primary {
    background-color: #337ab7;
}

.label-default {
    background-color: #777;
}

</style>


// a.split(',').map((elem) => elem.trim()).map((elem) =>  elem[0].toUpperCase() + elem.slice(1))// a.split(',').map((elem) => elem.trim()).map((elem) =>  elem[0].toUpperCase() + elem.slice(1))
