<template>
  <v-app>

    <v-dialog v-model="modal" max-width="400">
        <v-card>
          <v-card-title>Добавить категорию</v-card-title>
        <v-text-field
          v-model="name"
          label="Наименование"
        >
        </v-text-field>
        <v-btn style="background-color: chocolate" @click="createMod">Добавить</v-btn>
        </v-card>
    </v-dialog>
  <v-card>
    <v-row>
      <v-col>
        <v-btn @click="$router.push('/main')">Назад</v-btn>
      </v-col>
      <v-col>
        <v-btn @click="modal = !modal">Добавить</v-btn>
      </v-col>
    </v-row>
    <template v-if="items != null">
      <v-data-table
        :items="items"
        :headers="headers"
        no-data-text="Данные отсувствуют"
        no-results-text="По запросу ничего не найдено"
      >
      </v-data-table>
    </template>

  </v-card>
  </v-app>
</template>

<script>
import Drawner from "~/components/Drawner.vue";
import {axios} from 'axios';
export default {
  name: 'CategoriesPage',
  beforeMount() {
    this.getItems();
  },
  data(){
    return{
      items: null,
      modal: false,
      name: '',
      headers: [
        {
          text: "Наименование",
          align: 'center',
          value: "name",
          sortable: true,
        }
      ]
    }
  },
  methods:{
    getItems(){
      this.$axios.get('/categories-api')
        .then((response) => {
          console.log(response.data)
          this.items = response.data
        })
    },
    createMod(){
      var fm = new FormData();
      fm.append('name', this.name);
      this.$axios.post('/categories-api', fm)
        .then((response) => {
          this.modal = false
          this.name = ""
          this.getItems()
        })
    }
  }
}
</script>
