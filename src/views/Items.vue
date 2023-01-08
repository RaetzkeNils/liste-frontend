<template>
  <h1>Welcome to your Shopping List</h1>
  <button @click="createItem">Create</button>
  <create-item-form v-if="creatingItem" @close="creatingItem = null" />
  <br><br>
  <div class="container-fluid">
    <div class="row row-cols-1 row-cols-md-4 g-4">
      <div class="col" v-for="item in items" :key="item.id">
        <div class="card-body">
          <h3 class="card-title">{{ item.produkt }}</h3>
          <p class="card-text">
            Menge: {{ item.menge }}
          </p>
          <button @click="deleteItem(item.id)">Delete</button>
          <button @click="editItem(item)">Edit</button>
          <edit-item-form v-if="editingItem === item" :item="editingItem" @close="editingItem = null" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import EditItemForm from './EditItemForm.vue'
import CreateItemForm from './CreateItemForm.vue'

export default {
  name: 'Items',
  components: {
    EditItemForm,
    CreateItemForm
  },
  data () {
    return {
      items: [],
      editingItem: null,
      creatingItem: false
    }
  },
  methods: {
    deleteItem (id) {
      const requestOptions = {
        method: 'DELETE',
        redirect: 'follow'
      }

      fetch(`http://localhost:8080/api/v1/item/${id}`, requestOptions)
        .then(response => {
          if (response.ok) {
            // remove the item from the items array
            this.items = this.items.filter(item => item.id !== id)
          }
        })
        .catch(error => console.log('error', error))
    },
    editItem (item) {
      // open the edit item form
      this.editingItem = item
    },
    createItem () {
      // open the create item form
      this.creatingItem = true
    }
  },
  mounted () {
    const requestOptions = {
      method: 'GET',
      redirect: 'follow'
    }

    fetch('http://localhost:8080/api/v1/item', requestOptions)
      .then(response => response.json())
      .then(result => result.forEach(item => {
        this.items.push(item)
      }))
      .catch(error => console.log('error', error))
  }
}
</script>

<style scoped src="./style.css">
</style>
