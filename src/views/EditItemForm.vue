<template>
  <form @submit.prevent="saveItem">
    <div class="form-row">
      <label class="form-label">Produkt:</label>
      <input class="form-input" type="text" v-model="editingItem.produkt" />
    </div>
    <div class="form-row">
      <label class="form-label">Menge:</label>
      <input class="form-input" type="number" v-model="editingItem.menge" />
    </div>
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

<style scoped src="./style.css">
</style>
