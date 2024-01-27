<template>
  <v-row>
    <v-col>
      <v-btn style="background-color: chocolate" @click="$router.push('/categories')">Категории</v-btn>
      <v-btn style="background-color: chocolate" @click="$router.push('/products')">Продукты</v-btn>
      <v-btn style="background-color: chocolate" @click="$router.push('/angle')">Угол открывания</v-btn>
      <v-btn style="background-color: chocolate" @click="$router.push('/system')">Системы</v-btn>
      <v-btn style="background-color: chocolate" @click="$router.push('/producer')">Производитель</v-btn>
      <v-btn style="background-color: chocolate" @click="$router.push('/series')">Серии</v-btn>
    </v-col>
  </v-row>

</template>

<script>
import Drawner from "~/components/Drawner.vue";
export default {
  name: 'MainPage',
  components:{
    Drawner
  }
}
</script>
