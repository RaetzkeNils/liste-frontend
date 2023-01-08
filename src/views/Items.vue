<template>
  <h1>Welcome to your Shopping List</h1>
  <button @click="createItem">Create</button><br><br>
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
        </div>
      </div>
    </div>
  </div>
  <edit-item-form v-if="editingItem" :item="editingItem" @close="editingItem = null" />
  <create-item-form v-if="creatingItem" @close="creatingItem = null" />
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

<style scoped>

.container-fluid {
  width: 80%;
  margin: 0 auto;
}

.card-body {
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 20px;
  text-align: center;
}

.card-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
}

.card-text {
  font-size: 14px;
  margin-bottom: 20px;
}

button {
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

button:hover {
  background-color: #3e8e41;
}
</style>
