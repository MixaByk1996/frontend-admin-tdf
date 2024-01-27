<template>
  <v-app>

    <v-dialog v-model="modal" max-width="400">
      <v-card>
        <v-card-title>Добавить продукт</v-card-title>
        <v-text-field
        v-model="form.name"
        label="Наименование"
      >
      </v-text-field>
        <v-text-field
          v-model="form.description"
          label="Описание"
        >
        </v-text-field>
        <v-text-field
          v-model="form.price"
          label="Цена"
        >
        </v-text-field>

        <v-text-field
          v-model="form.article"
          label="Артикль"
        >
        </v-text-field>

        <v-select
          v-model="category"
          item-text="name"
          item-value='id'
          :items="this.array.categories"
          label="Выберите категорию"
          persistent-hint
          return-object
          single-line
        ></v-select>
        <v-select
          v-model="angle"
          item-text="name"
          item-value='id'
          :items="this.array.angles"
          label="Выберите угол"
          persistent-hint
          return-object
          single-line
        ></v-select>
        <v-select
          v-model="system"
          item-text="name"
          item-value='id'
          :items="this.array.systems"
          label="Выберите систему"
          persistent-hint
          return-object
          single-line
        ></v-select><v-select
          v-model="producer"
          item-text="name"
          item-value='id'
          :items="this.array.producers"
          label="Выберите производителя"
          persistent-hint
          return-object
          single-line
        ></v-select><v-select
          v-model="serial"
          item-text="name"
          item-value='id'
          :items="this.array.serials"
          label="Выберите серию"
          persistent-hint
          return-object
          single-line
        ></v-select>

        <img :src="imageUrl" height="150" v-if="imageUrl"/>
        <v-text-field label="Выберите фотографию продукта" @click='pickFile' v-model='imageName' prepend-icon="mdi-file-image"></v-text-field>
        <input
          type="file"

          style="display: none"
          ref="image"
          accept="image/jpeg, image/jpg, image/png"
          @change="onFilePicked"
        >
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
  components:{
    Drawner
  },
  beforeMount() {
    this.getItems();
    this.getSeries();
    this.getCategories();
    this.getAngles();
    this.getSystems();
    this.getProducers();
  },
  data(){
    return{
      rules: [
        v => !!v || 'Поле обязательно для заполнения!',
      ],
      items: null,
      modal: false,
      serial:null,
      system:null,
      imageUrl: '',
      imageName: '',
      imageFile: null,
      category:null,
      producer:null,
      angle:null,
      array:{
        serials: null,
        systems: null,
        categories: null,
        producers: null,
        angles: null,
      },

      form:{
        name: '',
        price: 0.0,
        description: '',
        image_path: '222',
        image_data: null,
        article: '',
        serial_id: 0,
        system_id: 0,
        producer_id: 0,
        category_id: 0,
        angle_id: 0,

      },
      name: '',
      headers: [
        {
          text: "Наименование",
          align: 'left',
          value: "name",
          sortable: true,
        },
        {
          text: "Цена",
          align: 'left',
          value: "price",
          sortable: true,
        },
        {
          text: "Производитель",
          align: 'left',
          value: "producer.name",
          sortable: true,
        }
      ]
    }
  },
  methods:{
    getItems(){
      this.$axios.get('/product-api')
        .then((response) => {
          console.log(response.data)
          this.items = response.data
        })
    },
    onFilePicked(e) {
      const files = e.target.files
      if(files[0] !== undefined) {
        this.imageName = files[0].name
        if (this.imageName.lastIndexOf('.') <= 0) {
          return
        }
        const fr = new FileReader()
        fr.readAsDataURL(files[0])
        fr.addEventListener('load', () => {
          this.imageUrl = fr.result
          this.imageFile = files[0]
          console.log(this.imageFile)
        })
      } else {
        this.imageName = ''
        this.imageFile = null
        this.imageUrl = ''
      }
    },
    getCategories(){
      this.$axios.get('/categories-api')
        .then((response) =>{
          this.array.categories = response.data
        })
    },
    getAngles(){
      this.$axios.get('/angle-api')
        .then((response) =>{
          this.array.angles = response.data
        })
    },
    getSystems(){
      this.$axios.get('/system-api')
        .then((response) =>{
          this.array.systems = response.data
        })
    },
    getProducers(){
      this.$axios.get('/producer-api')
        .then((response) =>{
          this.array.producers = response.data
        })
    },getSeries(){
      this.$axios.get('/series-api')
        .then((response) =>{
          this.array.serials = response.data
        })
    },
    pickFile() {
      this.$refs.image.click()
    },
    createMod(){
      if(this.angle == null || this.system == null || this.producer == null || this.category == null){
        alert('Не выбраны данные из списка');
        return;
      }
      var fm = new FormData();
      fm.append('angle_id', this.angle.id);
      fm.append('system_id', this.system.id);
      fm.append('serial_id', this.serial.id);
      fm.append('category_id', this.category.id);
      fm.append('producer_id', this.producer.id);
      fm.append('name', this.form.name);
      fm.append('price', this.form.price);
      fm.append('description', this.form.description);
      fm.append('image_path', this.form.image_path);
      fm.append('article', this.form.article);
      fm.append('image_data', this.imageFile);

      // this.form.angle_id = this.angle.id
      // this.form.system_id = this.system.id
      // this.form.serial_id = this.serial.id
      // this.form.category_id = this.category.id
      // this.form.producer_id = this.producer.id
      // this.form.image_data =  this.imageFile
      this.$axios.post('/product-api', this.form)
        .then((response) => {
          this.modal = false
          this.name = ""
          this.getItems()
        })
    }
  }
}
</script>
