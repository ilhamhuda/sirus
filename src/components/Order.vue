<template>
    <div style="padding:20px">
        <v-layout>
   <v-card style="width:40%">
        <v-card-title>
          <span class="headline">Tambah Transaksi</span>
        </v-card-title>
        <v-card-text>
          <v-container grid-list-md>
            <v-layout wrap>
            <v-flex xs12 sm8>
               <span>Pilih Produk*</span>
                <select v-model="nmProduk"  v-on:change="hitungjumlah($event)" class="drop" >
                  <option selected >Pilih Produk</option>
                 <option v-for="post in product" :key="post.id" :value="post.id" aria-placeholder="Merk" >
               {{ post.name }}
                </option>
                  </select>
              </v-flex>
              <v-flex xs12 sm8
              >
                <span>Pilih Jumlah*</span>
               <select v-model="quantityProduk"  v-on:change="hitungquantity($event)" class="drop" >
                  <option selected >Pilih Jumlah</option>
                 <option v-for="post in quantitybelanja" :key="post.id" :value="post.no" aria-placeholder="Merk" >
               {{ post.no }}
                </option>
                  </select>
              </v-flex>
                   <!-- <button  style="width:30px;height:30px;background-color:green;margin:25px;color:white" @click="btnsubmit()">+</button> -->
               

            </v-layout>
          </v-container>
        </v-card-text>
      </v-card>
   <v-card style="width:55%;margin-left:30px;padding:10px;">
      <v-container grid-list-md>
            <v-layout wrap>
       <v-flex xs12 sm4>
                <span>Date*</span>
                <input  required 
                v-model="tanggalProduk"
                type="text" class="drop"
                 disabled/>
                 
                 </v-flex>
               <v-flex xs12 sm4>
               <span>Total Price*</span>
                 <input  required 
                 v-model="totalProduk"
                type="text" class="drop" disabled
                 />
              </v-flex>
              
            </v-layout>
          </v-container>
<v-card >
    <v-card-title>
      List of Purchase
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="search"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="transaksi"
      :search="search"
      ref="printTable"
      id="printMe"
    >
     <template slot="items" slot-scope="props">
        <!-- <td>{{ props.item.id }}</td> -->
        <td>{{ props.item.namaProduk }}</td>
        <td>{{ props.item.jumlahProduk }}</td>
        <td>{{ props.item.hargaProduk }}</td>
        <td> <v-btn style="background:red;" class="ma-2" tile outlined @click="deleteData(props.item)" color="error">
      <v-icon left dark style="width:30px;">delete</v-icon> Delete
    </v-btn></td>
        
      </template>
      
      <v-alert slot="no-results" :value="true" color="error" icon="warning">
        Your search for "{{ search }}" found no results.
      </v-alert>
    
    </v-data-table>
  </v-card>
      <v-btn class="ma-2" tile outlined @click="printData()" color="success">
      <v-icon left>check</v-icon> Selesai
    </v-btn>
   </v-card>
   </v-layout>
    <v-dialog v-model="dialog" persistent max-width="100%" style="width:1000px;height:1000px">
    <v-card >
         <v-card-text>
          <v-container grid-list-md>
            <v-layout wrap>
        <v-flex xs12 sm8>
                <span>Date*</span>
                <input  required 
                v-model="tanggalProduk"
                type="text" class="drop"
                 disabled/>
                 
                 </v-flex>
               <v-flex xs12 sm8>
               <span>Total Price*</span>
                 <input  required 
                 v-model="totalProduk"
                type="text" class="drop" disabled
                 />
              </v-flex>

            </v-layout>
          </v-container>
           </v-card-text>
           <v-btn class="ma-2" tile outlined color="warning" @click="printData()">
      <v-icon left>print</v-icon> Print
    </v-btn>
      </v-card>
   
    </v-dialog>
  </div>
  
</template>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.5.17-beta.0/vue.js"></script>
<script>
  import moment from 'moment'
  import Vue from 'vue'
  import {HTTP} from '../views/httpcommon.js'
  import Print from 'vue-print-nb'
  import { EventBus } from './event-bus.js'
  Vue.use(Print)
  export default {
    data: () => ({
      drawer: null,
      testclick: function () {
      },
      search: '',
      select: '',
      product: [],
      dialog: false,
      totalProduk: 0,
      printObj:
      {
        id: 'printMe',
        popTitle: 'good print',
        extraCss: 'https://www.google.com,https://www.google.com',
        extraHead: '<meta http-equiv="Content-Language"content="zh-cn"/>'
      },
      hargaProduk: 0,
      tanggalProduk: moment().format('YYYY-MM-DD'),
      transaksi: [],
      produk: [
        {
          id: 0,
          namaProduk: 'Obat Tidur',
          hargaProduk: 90000,
          jumlahProduk: 20
        },
        {
          id: 1,
          namaProduk: 'Obat Sakit Perut',
          hargaProduk: 38000,
          jumlahProduk: 20
        }
      ],
      quantitybelanja: [
        {
          no: '1'
        },
        {
          no: '2'
        },
        {
          no: '3'
        },
        {
          no: '4'
        },
        {
          no: '5'
        },
        {
          no: '6'
        },
        {
          no: '7'
        },
        {
          no: '8'
        },
        {
          no: '9'
        },
        {
          no: '10'
        }
      ],
      quantityProduk: 'Pilih Jumlah',
      nmProduk: 'Pilih Produk',
      idproduk: null,
      headers: [
        { text: 'Product Name', value: 'name' },
        { text: 'Quantity', value: 'quantity' },
        { text: 'Total Price', value: 'price' },
        { text: 'Edit', value: 'edit' }
      ]
    }),
    props: {
      appTitle: {
        type: String,
        default: '',
        description: 'Application Title'
      }
    },
    created () {
      var me = this
      HTTP.get(`produk/lists`)
        .then(response => {
          me.product = response.data.data
          console.log('produk', me.product, me.produk)
        })
    },
    mounted () {
      this.hitungjumlah()
    },
    methods: {
      doLogout (event) {
        console.log('logout', event)
      },
      hitungjumlah: function (event) {
        for (var n = 0; n < this.product.length; n++) {
          if (this.product[n].id === event.target.value) {
            this.hargaProduk = this.product[n].price
            // this.nmProduk = this.product[event.target.value].name
            this.idproduk = event.target.value
          }
        }
        console.log('data jumlah', event.target.value)
      },
      hitungquantity: function (event) {
        var a = this.hargaProduk * 1
        for (var n = 0; n < this.product.length; n++) {
          if (this.product[n].id === this.idproduk) {
            this.nmProduk = this.product[n].name
          }
        }
        var b = event.target.value * 1
        this.totalProduk = a * b
        this.transaksi.push({namaProduk: this.nmProduk, idproduk: this.idproduk, jumlahProduk: this.quantityProduk, hargaProduk: this.totalProduk})
        this.hargaProduk = 0
        this.quantityProduk = 'Pilih Jumlah'
        this.nmProduk = 'Pilih Produk'
      },
      btncancel () {
        this.totalProduk = 0
        this.hargaProduk = 0
        this.quantityProduk = ''
        this.namaProduk = ''
      },
      printData () {
        var me = this
        console.log('harga produk total', this.totalProduk)
        var query = 0
        for (var i = 0; i < this.transaksi.length; i++) {
          query = query + this.transaksi[i].hargaProduk
        }
        me.totalProduk = query
        const formData = {
          totalprice: me.totalProduk * 1
        }
        HTTP.post('transaksi/insert', formData)
          .then(res => {
            console.log(res)
            var payload = this.transaksi
            EventBus.$emit('DATA_PUBLISHED', payload)
            window.open('#/invoice')
            var hitungtotal = this.produk.hargaProduk.reduce((a, b) => a + b, 0)
            console.log(hitungtotal)
          })
          .catch(error => {
            console.log('error', error)
          })
      },
      deleteData (event) {
        this.transaksi.splice(this.transaksi.indexOf(event), 1)
      }
    }
  }
</script>

<style scoped>
body {
  padding: 90px;
  margin: 50px;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.drop {
    display: block;
    width: 100%;
    height: 34px;
    padding: 6px 12px;
    font-size: 14px;
    line-height: 1.428571429;
    color: #999;
    background-color: #ffffff;
    border: 1px solid #cccccc;
}
</style>