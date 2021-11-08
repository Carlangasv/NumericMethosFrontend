<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="10" md="12">
      <v-card>
        <v-card-title class="headline"> Integración </v-card-title>
        <br />
        <v-card-text> Seleccione la funcion a usar </v-card-text>

        <v-combobox
          v-model="selected"
          :items="items"
          item-text="name"
          item-value="value"
          label="Seleccione"
          filled
          outlined
          :return-object="true"
          @change="onChange"
          >{{ items.name }}</v-combobox
        >

        <div v-show="show_s13">
          <v-form ref="forms13">
            <v-row justify="space-between">
              <v-col cols="12" sm="5">
                <v-text-field
                  v-model="data_s13.f"
                  label="Ingrese la funcion"
                  type="text"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_s13.n"
                  label="Ingrese el valor de n"
                  type="number"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" sm="5">
                <v-text-field
                  v-model="data_s13.a"
                  label="Ingrese el valor de a"
                  type="number"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_s13.b"
                  label="Ingrese el valor de b"
                  type="number"
                  required
                ></v-text-field>

              </v-col>
            </v-row>

            <v-btn @click="submitS13">Aceptar</v-btn>
          </v-form>
        </div>

        <div v-show="show_s38">
          <v-form ref="forms38">
            <v-row justify="space-between">
              <v-col cols="12" sm="5">
                <v-text-field
                  v-model="data_s38.f"
                  label="Ingrese la funcion"
                  type="text"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_s38.xn"
                  label="Ingrese el valor de xn"
                  type="number"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" sm="5">
                <v-text-field
                  v-model="data_s38.x0"
                  label="Ingrese el valor de x0"
                  type="number"
                  required
                ></v-text-field>


              </v-col>
            </v-row>

            <v-btn @click="submitS38">Aceptar</v-btn>
          </v-form>
        </div>

        <div v-show="show_s13l">
          <v-form ref="forms13l">
            <v-row justify="space-between">
              <v-col cols="12" sm="5">
                <v-text-field
                  v-model="data_s13l.x_list"
                  label="Ingrese los valores de x separados por comas"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" sm="5">
                <v-text-field
                   v-model="data_s13l.y_list"
                  label="Ingrese los valores de y separados por comas"
                  type="text"
                  required
                ></v-text-field>

              </v-col>
            </v-row>

            <v-btn @click="submitS13l">Aceptar</v-btn>
          </v-form>
        </div>

        <div v-show="show_s38l">
          <v-form ref="forms13">
            <v-row justify="space-between">
              <v-col cols="12" sm="5">
                <v-text-field
                  v-model="data_s38l.x_list"
                  label="Ingrese los valores de x separados por comas"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" sm="5">
                <v-text-field
                   v-model="data_s38l.y_list"
                  label="Ingrese los valores de y separados por comas"
                  type="text"
                  required
                ></v-text-field>

              </v-col>
            </v-row>

            <v-btn @click="submitS38l">Aceptar</v-btn>
          </v-form>
        </div>

        
      </v-card>
    </v-col>
  </v-row>
</template>


<script>

import axios from 'axios'
import config from '@/assets/config'


export default {
  data() {
    return {
      selected: null,
      items: [
        {
          name: 'Simpson 1/3',
          value: 'simpson13',
        },
        {
          name: 'Simpson 3/8',
          value: 'simpson38',
        },

        {
          name: 'Simpson 1/3 con listas',
          value: 'simpson13_list',
        },
        {
          name: 'Simpson 3/8 con listas',
          value: 'simpson38_list',
        },
      ],

      data_s13: {
        f: '',
        a: null,
        b: null,
        n: null,
        
      },

      data_s38: {
        f: '',
        x0: null,
        xn: null,
      },

      data_s13l: {
        x_list: '',
        y_list: '',
      },

      data_s38l: {
        x_list: '',
        y_list: '',
      },

      show_s13: true,
      show_s38: false,
      show_s13l: false,
      show_s38l: false,
      

    }

    
  },

  methods: {
    onChange(item) {
      switch (item.value) {
        case 'simpson13':
          this.show_s13 = true;
          this.show_s38 = false;
          this.show_s13l = false;
          this.show_s38l = false;
          break;

        case 'simpson38':
          this.show_s13 = false;
          this.show_s38 = true;
          this.show_s13l = false;
          this.show_s38l = false;
          break;

        case 'simpson13_list':
          this.show_s13 = false;
          this.show_s38 = false;
          this.show_s13l = true;
          this.show_s38l = false;
          break;
        case 'simpson38_list':
          this.show_s13 = false;
          this.show_s38 = false;
          this.show_s13l = false;
          this.show_s38l = true;
          break;
        
      }
    },

  async  submitS13() {

      const URL = `${config.api}` + '/simpson_13'

      var s13 = {
        a: this.data_s13.a,
        b: this.data_s13.b,
        n: this.data_s13.n,
        funcion: this.data_s13.f,
      } 

      console.log(s13)
      let { data } = await axios.post(URL, s13)
      console.log('data: ', data)

    },

   async submitS38() {
     const URL = `${config.api}` + '/simpson_38'

      var s38 = {
        x0: this.data_s38.x0,
        xn: this.data_s38.xn,
        funcion: this.data_s38.f,
      } 

      console.log(s38)
      let { data } = await axios.post(URL, s38)
      console.log('data: ', data)
    },

    submitS13l() {
      const URL = `${config.api}` + '/simpson_38'
      x_list = this.data_s13l.x_list.split(',');
      y_list = this.data_s13l.y_list.split(',');

      var s13l = {
        x: x_list,
        fx: y_list,
      } 

      console.log(s13l)
      //let { data } = await axios.post(URL, s13l)
      //console.log('data: ', data)

    },

    submitS38l() {
      console.log(this.data_s38l)
    },

  },
}
</script>