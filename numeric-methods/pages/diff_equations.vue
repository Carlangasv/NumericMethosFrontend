<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="10" md="12">
      <v-card>
        <v-card-title class="headline"> Ecuaciones Diferenciales </v-card-title>
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

        <div v-show="show_rk">
          <v-form ref="form_rk_4">
            <v-row justify="space-between">
              <v-col cols="12" sm="5">
                <v-text-field
                  v-model="data_rk.f"
                  label="Ingrese la funcion"
                  type="text"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_rk.xi"
                  label="Ingrese el valor de xi"
                  type="number"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_rk.x"
                  label="Ingrese el valor de x"
                  type="number"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" sm="5">
                <v-text-field
                  v-model="data_rk.h"
                  label="Ingrese el valor de h"
                  type="number"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_rk.xf"
                  label="Ingrese el valor de xf"
                  type="number"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_rk.y"
                  label="Ingrese el valor de y"
                  type="number"
                  required
                ></v-text-field>
              </v-col>
            </v-row>

            <v-btn @click="submitRK">Aceptar</v-btn>
          </v-form>
        </div>

        <div v-show="show_rk_edo">
          <v-form ref="form_rk_edo">
            <v-row justify="space-between">
              <v-col cols="12" sm="5">
                <v-text-field
                  v-model="data_rk_edo.f[0]"
                  label="Ingrese la funcion 1"
                  type="text"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_rk_edo.xi"
                  label="Ingrese el valor de xi"
                  type="number"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_rk_edo.ci"
                  label="Ingrese el valor de ci"
                  type="number"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" sm="5">
                <v-text-field
                  v-model="data_rk_edo.f[1]"
                  label="Ingrese la funcion 2"
                  type="text"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_rk_edo.xf"
                  label="Ingrese el valor de xf"
                  type="number"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="data_rk_edo.h"
                  label="Ingrese el valor de h"
                  type="number"
                  required
                ></v-text-field>
              </v-col>
            </v-row>

            <v-btn @click="submitRK4EDO">Aceptar</v-btn>
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
          name: 'Runge-Kutta 4to Orden',
          value: 'cuarto_orden',
        },
        {
          name: 'Runge-Kutta Orden Superior',
          value: 'orden_superior',
        },

        {
          name: 'Runge-Kutta 4to Orden ED Ordinaria',
          value: 'cuarto_orden_edo',
        },
      ],

      data_rk: {
        f: '',
        h: null,
        xi: null,
        xf: null,
        xi: null,
        y: null,
      },

      data_rk_edo: {
        f: ['', ''],
        h: null,
        xi: null,
        xf: null,
        xi: null,
        ci: null,
      },

      show_rk: false,
      show_rk_edo: false,
    }
  },

  methods: {
    onChange(item) {
      switch (item.value) {
        case 'cuarto_orden':
          this.show_rk = true
          this.show_rk_edo = false
          break

        case 'orden_superior':
          this.show_rk = true
          this.show_rk_edo = false
          break

        case 'cuarto_orden_edo':
          this.show_rk = false
          this.show_rk_edo = true
          break
      }
    },

    async submitRK() {
      console.log(this.data_rk)

      const URL = `${config.api}/` + this.selected.value

      var rk = {
        h: parseFloat(this.data_rk.h),
        xf: parseFloat(this.data_rk.xf),
        xi: parseFloat(this.data_rk.xi),
        x: parseFloat(this.data_rk.x),
        y: parseFloat(this.data_rk.y),
        funcion: this.data_rk.f,
      } 
      let { data } = await axios.post(URL, rk)
    },

   async submitRK4EDO() {
      console.log(this.data_rk_edo)

      const URL = `${config.api}/` + this.selected.value

      var rk4EDO = {
        h: parseFloat(this.data_rk_edo.h),
        xf: parseFloat(this.data_rk_edo.xf),
        xi: parseFloat(his.data_rk_edo.xi),
        ci: this.data_rk_edo.ci,
        funcion: this.data_rk_edo.f,
      } 
      let { data } = await axios.post(URL, rk4EDO)
    },
  },
}
</script>