<template>
  <div class="animated fadeIn" v-if="this.item">
        <b-form-group style="text-align: center;">
          <b-form-radio-group v-model="activeStatus"
                              :options="options"
                              name="radioInline"
                              >
          </b-form-radio-group>
        </b-form-group>
    <b-row>
        <div class="text-center col-md-12 mb-3">
          <b-button
            v-show="activeStatus !== this.item.status"
            type="submit"
            @click="changeStatus(activeStatus)"
            size="sm"
            v-bind:class="{
              'btn-primary': activeStatus === 'Новый',
              'btn-success': activeStatus === 'В обработке',
              'btn-secondary': activeStatus === 'Обработан',
              'btn-info': activeStatus === 'Отправлен',
              'btn-warning': activeStatus === 'Отменен',
              'btn-danger': activeStatus === 'Возврат'
            }">
            <i class="fa fa-dot-circle-o"></i> Изменить статус
          </b-button>
        </div>
      <b-col md="6">
        <b-card
           v-bind:class="{
              'border-primary': this.item.status === 'Новый',
              'border-success': this.item.status === 'В обработке',
              'border-secondary': this.item.status ===  'Обработан',
              'border-info': this.item.status === 'Отправлен',
              'border-warning': this.item.status === 'Отменен',
              'border-danger': this.item.status === 'Возврат'
            }">
          <!-- class="border-success" -->

          <div slot="header">
            <span class="float-right badge" style="font-size: 18px;"
                v-bind:class="{
                  'badge-primary': this.item.status === 'Новый',
                  'badge-success': this.item.status === 'В обработке',
                  'badge-secondary': this.item.status ===  'Обработан',
                  'badge-info': this.item.status === 'Отправлен',
                  'badge-warning': this.item.status === 'Отменен',
                  'badge-danger': this.item.status === 'Возврат'
                }"
            >
              {{ this.item.status }}
            </span>

            <strong>Заказ</strong> <hr>


            <b-form-fieldset
              label="ID заказа |"
              :label-cols="5"
              :horizontal="true">
              <!-- <b-form-input v-model="form_descr.name" type="text"></b-form-input> -->
              <p class="card-text col-form-legend">
                {{ this.item._id }}
              </p>
            </b-form-fieldset>

            <b-form-fieldset
              label="Дата поступления заказа |"
              :label-cols="5"
              :horizontal="true">
              <!-- <b-form-input v-model="form_descr.name" type="text"></b-form-input> -->
              <p class="card-text col-form-legend">
                  {{ new Date(this.item.created).toLocaleString('ru') }}
              </p>
            </b-form-fieldset>

            <b-form-fieldset
              label="Примечания к заказу |"
              :label-cols="5"
              :horizontal="true">
              <!-- <b-form-input v-model="form_descr.name" type="text"></b-form-input> -->
              <p class="card-text col-form-legend">
                  {{ this.item.notes }}
              </p>
            </b-form-fieldset>

          </div>

          <strong>Клиент</strong>

          <div class="float-right" v-if="item.countryid === 'UA'">
            <img src="static/img/flags/Ukraine.png" alt="USA" style="height: 40px; margin-top: -8px;">
          </div>

          <hr>
          <!-- Name -->
          <b-form-fieldset
            label="ФИО |"
            :label-cols="5"
            :horizontal="true">
            <!-- <b-form-input v-model="form_descr.name" type="text"></b-form-input> -->
            <p class="card-text col-form-legend">
              {{ this.item.firstName }}
              {{ this.item.lastName }}
              {{ this.item.middleName }}
            </p>
          </b-form-fieldset>

          <b-form-fieldset
            label="Номер Телефона |"
            :label-cols="5"
            :horizontal="true">
            <!-- <b-form-input v-model="form_descr.name" type="text"></b-form-input> -->
            <p class="card-text col-form-legend">
              {{ this.item.phone }}
            </p>
          </b-form-fieldset>

          <b-form-fieldset
            label="Email |"
            :label-cols="5"
            :horizontal="true">
            <!-- <b-form-input v-model="form_descr.name" type="text"></b-form-input> -->
            <p class="card-text col-form-legend">
              {{ this.item.email }}
            </p>
          </b-form-fieldset>

          <!-- Price -->
          <b-form-fieldset
            label="Cтрана |"
            :label-cols="5"
            :horizontal="true">
            <p class="card-text col-form-legend">
              {{ this.item.countryname }}
            </p>
          </b-form-fieldset>

          <!-- Material2 -->
          <b-form-fieldset
            label="Область |"
            :label-cols="5"
            :horizontal="true"
          >
            <p class="card-text col-form-legend">
              {{ this.item.regionname }}
            </p>
          </b-form-fieldset>

          <!-- Material2 -->
          <b-form-fieldset
            label="Город |"
            :label-cols="5"
            :horizontal="true"
          >
            <p class="card-text col-form-legend">
              {{ this.item.city }}
            </p>
          </b-form-fieldset>

          <!-- Material2 -->
          <b-form-fieldset
            label="Адрес |"
            :label-cols="5"
            :horizontal="true"
          >
            <p class="card-text col-form-legend">
              {{ this.item.address }}
            </p>
          </b-form-fieldset>

          <!-- Material2 -->
          <b-form-fieldset
            label="Почтовый индекс |"
            :label-cols="5"
            :horizontal="true"
          >
            <p class="card-text col-form-legend">
              {{ this.item.post }}
            </p>
          </b-form-fieldset>
        </b-card>
      </b-col>

      <b-col md="6">
          <b-card
              v-bind:class="{
                'border-primary': this.item.status === 'Новый',
                'border-success': this.item.status === 'В обработке',
                'border-secondary': this.item.status ===  'Обработан',
                'border-info': this.item.status === 'Отправлен',
                'border-warning': this.item.status === 'Отменен',
                'border-danger': this.item.status === 'Возврат'
              }">
            <div slot="header">
              <span class="float-right badge badge-pill"
                    v-bind:class="{
                      'badge-primary': this.item.status === 'Новый',
                      'badge-success': this.item.status === 'В обработке',
                      'badge-secondary': this.item.status ===  'Обработан',
                      'badge-info': this.item.status === 'Отправлен',
                      'badge-warning': this.item.status === 'Отменен',
                      'badge-danger': this.item.status === 'Возврат'
                    }"
                    style="font-size: 18px">
                Кол-во: {{ this.item.products.reduce((summ, product) => Number(summ) + Number(product.qty), 0), }} шт.
              </span>

              <strong>Товар</strong> <hr>

              <b-form-fieldset
                label="На сумму:"
                :label-cols="5"
                :horizontal="true">
                <!-- <b-form-input v-model="form_descr.name" type="text"></b-form-input> -->
                <p class="card-text col-form-legend">
                  {{ this.item.products.reduce((summ, product) => summ + (product.price.total * (product.qty || 1)), 0) }} грн.
                </p>
              </b-form-fieldset>

            </div>

            <strong>Список</strong>
            <label class="switch switch-sm switch-text switch-info float-right mb-0">
              <input type="checkbox" class="switch-input" v-model="showInfoChecked">
              <span class="switch-label" data-on="+" data-off="-"></span>
              <span class="switch-handle"></span>
            </label> <hr>

            <div v-for="product in this.item.products">
            <div style="height: 20px;">
              <span class="float-left badge badge-pill">
                {{ product.qty }} шт.
              </span>
            </div>
                  <div v-if="!showInfoChecked">
                      <div class="card-text col-form-legend">
                        {{ product.name }} |  {{ product.model }} |  {{ product.price.total * product.qty }} грн.
                      </div>
                  </div>

                  <div v-if="showInfoChecked">
                      <b-form-fieldset
                      label="Наименование |"
                      :label-cols="5"
                      :horizontal="true">
                      <p class="card-text col-form-legend">
                        {{ product.name }}
                      </p>
                    </b-form-fieldset>
                      <b-form-fieldset
                      label="Артикул |"
                      :label-cols="5"
                      :horizontal="true">
                      <p class="card-text col-form-legend">
                        {{ product.model }}
                      </p>
                    </b-form-fieldset>
                    <b-form-fieldset
                      label="Изображение |"
                      :label-cols="5"
                      :horizontal="true">
                      <p class="card-text col-form-legend">
                        <img :src="product.images" alt="Product" style="height: 100px; margin-top: -8px;">
                      </p>
                    </b-form-fieldset>
                  <b-form-fieldset
                      label="Размер |"
                      :label-cols="5"
                      :horizontal="true">
                      <p class="card-text col-form-legend">
                        {{ product.size }}
                      </p>
                    </b-form-fieldset>
                  <b-form-fieldset
                      label="Материал |"
                      :label-cols="5"
                      :horizontal="true">
                      <p class="card-text col-form-legend">
                      </p>
                        <!-- {{ TYPE_OF_TEXTILE.find(textile => textile.id === product.textile[0]).name }} -->
                        <span v-if="product.textile" v-for="(t, index) in product.textile" :key="t">
                          {{t}} <span v-if="index < product.textile.length - 1">,</span>
                        </span>
                    </b-form-fieldset>
                    <b-form-fieldset
                      label="Цена |"
                      :label-cols="5"
                      :horizontal="true">
                      <p class="card-text col-form-legend">
                        <span v-if="product.price.discount === 0">{{ product.price.current }}грн./шт.</span>
                        <span v-if="product.price.discount > 0">
                          {{ Math.ceil(product.price.current - (product.price.current * product.price.discount / 100)) }}грн./шт.
                          ({{ product.price.current }}грн. - {{ product.price.discount }}%)
                        </span>
                      </p>
                    </b-form-fieldset>
                    <b-form-fieldset
                      label="Количество |"
                      :label-cols="5"
                      :horizontal="true">
                      <!-- <b-form-input v-model="form_descr.name" type="text"></b-form-input> -->
                      <p class="card-text col-form-legend">
                        {{ product.qty }}шт.
                      </p>
                    </b-form-fieldset>
                    <b-form-fieldset
                      label="Итого |"
                      :label-cols="5"
                      :horizontal="true">
                      <!-- <b-form-input v-model="form_descr.name" type="text"></b-form-input> -->
                      <p class="card-text col-form-legend">
                        {{ product.price.total * product.qty }}грн.
                      </p>
                    </b-form-fieldset>
                  </div>
                <hr>
                </div>
          </b-card>
        </b-col>
    </b-row>
  </div>
</template>

<script>
import axios from 'axios'
import Multiselect from 'vue-multiselect'

import { TYPE_OF_TEXTILE } from '../data/textile.js'

const linkOrders = `${process.env.ENDPOINT}/api/orders`

// const queryString = require('query-string')

export default {
  name: 'Order',
  components: {
    Multiselect
  },
  data () {
    return {
      item: null,
      productsWithInfo: [],
      showInfoChecked: false,
      activeStatus: 'Новый',
      options: [
        { text: 'Новый', value: 'Новый' },
        { text: 'В обработке', value: 'В обработке' },
        { text: 'Обработан', value: 'Обработан' },
        { text: 'Отправлен', value: 'Отправлен' },
        { text: 'Отменен', value: 'Отменен' },
        { text: 'Возврат', value: 'Возврат' }
      ],
      TYPE_OF_TEXTILE
    }
  },
  created () {
    const orderId = this.$route.params.id || null
    console.log(this.$route.params.id)
    axios.get(`${linkOrders}/${orderId}`)
      .then(response => {
        this.item = response.data
        this.activeStatus = this.item.status || 'Новый'
      })
      .catch(function (error) {
        console.log(error)
      })
    // console.log(TYPE_OF_TEXTILE.find(textile => textile.id === 1).name)
  },
  methods: {
    // getProductsInfo () {
    //   const queryIds = queryString.stringify({ ids: this.item.products }, { arrayFormat: 'bracket' })
    //   console.log(queryIds)
    //   axios.get('http://localhost:4040/api/products/?' + queryIds)
    //     .then(response => {
    //       this.productsWithInfo = response.data
    //     })
    //     .catch(function (error) {
    //       console.log(error)
    //     })
    // },
    changeStatus (status) {
      axios.put(`${linkOrders}/${this.item._id}`, { status })
        .then(response => {
          this.item = response.data
          this.activeStatus = this.item.status
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  }
}
</script>

<style src='vue-multiselect/dist/vue-multiselect.min.css'></style>
<style>
  .form-row > legend {
    text-align: right;
  }

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
