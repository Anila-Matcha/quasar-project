<template>
  <q-page class="flex flex-center">
    <!--<q-input v-model="text" :dense="true"/>-->
    <ItemCart />
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import ItemCart from "../components/ItemCart.vue";

export default defineComponent({
  name: 'IndexPage',
  methods: {
    save:function(){
      console.log("Testing QUasar app");
    }
  },
  components:{
    ItemCart
  }
})
</script>
