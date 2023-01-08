<template>
  <form @submit.prevent="createItem">
    <div class="form-row">
      <label class="form-label">Produkt:</label>
      <input class="form-input" type="text" v-model="newItem.produkt" />
    </div>
    <div class="form-row">
      <label class="form-label">Menge:</label>
      <input class="form-input" type="number" v-model="newItem.menge" />
    </div>
    <button type="submit">Create</button>
    <button type="button" @click="$emit('close')">Cancel</button>
  </form>
</template>

<script>
export default {
  name: 'CreateItemForm',
  data () {
    return {
      newItem: {
        produkt: '',
        menge: ''
      }
    }
  },
  methods: {
    createItem () {
      const requestOptions = {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(this.newItem),
        redirect: 'follow'
      }

      fetch('http://localhost:8080/api/v1/item', requestOptions)
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
