<template>
  <form @submit.prevent="saveItem">
    <label for="produkt">Produkt:</label>
    <input type="text" id="produkt" v-model="editingItem.produkt" />
    <br />
    <label for="menge">Menge:</label>
    <input type="text" id="menge" v-model="editingItem.menge" />
    <br />
    <button type="submit">Save</button>
    <button type="button" @click="$emit('close')">Cancel</button>
  </form>
</template>

<script>
export default {
  name: 'EditItemForm',
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      editingItem: { ...this.item }
    }
  },
  methods: {
    saveItem () {
      const requestOptions = {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(this.editingItem),
        redirect: 'follow'
      }

      fetch(`http://localhost:8080/api/v1/item/${this.item.id}`, requestOptions)
        .then(response => response.json())
        .then(result => {
          console.log(result)
          this.$emit('close')
          window.location.reload()
        })
        .catch(error => console.log('error', error))
    }
  }
}
</script>

<style scoped>
</style>
