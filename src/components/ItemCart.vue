<template>
  <div class="q-pa-md" style="max-width: 350px">
    <q-list bordered separator v-for="item in items" :key=item>
      <q-item>
        <q-item-section>
          <q-item-label>{{ item.name }}</q-item-label>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ item.price }}</q-item-label>
        </q-item-section>
        <q-item-section>
          <q-btn
            class="glossy"
            round
            color="deep-orange"
            icon="local_activity"
          />
        </q-item-section>
      </q-item>
    </q-list>
  </div>
</template>

<script>
export default{
  name:"ItemCart",
  data(){
    return {
      items:[]
    };
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
