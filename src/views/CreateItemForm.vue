<template>
  <form @submit.prevent="createItem">
    <label for="produkt">Produkt:</label>
    <input type="text" id="produkt" v-model="newItem.produkt" />
    <br />
    <label for="menge">Menge:</label>
    <input type="text" id="menge" v-model="newItem.menge" />
    <br />
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

<style scoped>
</style>
