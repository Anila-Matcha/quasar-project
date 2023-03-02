<template>
  <div class="q-pa-md" style="max-width: 350px">

    <div class="q-pa-md" style="max-width: 400px">
    <q-form  @reset="onReset" class="q-gutter-md">
        <q-input filled v-model="item.name" label="Item name *" hint="Product Name" lazy-rules
          :rules="[val => val && val.length > 0 || 'Please type something']" />

        <q-input filled type="number" v-model="item.price" label="Item Price *" lazy-rules :rules="[
          val => val !== null && val !== '' || 'Please type the price',
          val => val > 0 && val < 1000000 || 'Please type a real price'
        ]" />

        <q-date v-model="item.date" />

        <!-- <q-toggle v-model="accept" label="I accept the license and terms" /> -->

        <div>
          <q-btn label="Submit" type="submit" @click='save' color="primary" />
          <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
        </div>
      </q-form>
  </div>
    <q-list bordered separator v-for="(item, index) in items" :key="item.id">
      <q-item>
        <q-item-section>
          <q-item-label>{{ item.name }}</q-item-label>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ item.price }}</q-item-label>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ item.date }}</q-item-label>
        </q-item-section>  
        <q-item-section avatar>
          <q-avatar color="red" text-color="white" icon="delete" @click="deleteItem(item.id, index)" />
        </q-item-section>
      </q-item>
    </q-list>
  </div>
</template>

<script>
import { ref } from 'vue';
export default{
  name:"ItemCart",
  data(){
    return {
      item:{
        name:'IPhone',
        price:30000,
        date: '2023/03/01',
      },
      items: [],
      date: ref('2023/03/01'),
    };
  },
  methods:{
    async save(){
      console.log("Saving Item");
    const saveItemResponse = await fetch("http://localhost:3000/items",{
      method:'post',
      body: JSON.stringify(Object.assign({}, this.item)),
      headers:{
        'content-type':'application/json'
      }
    });
      this.items.push(await saveItemResponse.json());
      console.log("Item Saved");
    },
    deleteItem(itemId, index) {
      const deleteResponse = fetch("http://localhost:3000/items/" + itemId, {
        method: "delete",
      });
      deleteResponse.then(() => {
        this.items.splice(index, 1);
      });
    },
  },
  mounted(){
      console.log("ENTERED MOUNTED");
      const itemsResponse = fetch("http://localhost:3000/items");
      itemsResponse.then((response) => {
        response.json().then((items) => {
          this.items = items;
        });
      });
    }
}
</script>
