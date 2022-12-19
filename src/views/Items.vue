<template>
  <h1>Welcome to your Shopping List</h1>
  <div class="container-fluid">
    <div class="row row-cols-1 row-cols-md-4 g-4">
      <div class="col" v-for="item in items" :key="item.id">
          <div class="card-body">
            <h5 class="card-title">{{ item.produkt }}</h5>
            <p class="card-text">
              Menge: {{ item.menge }}
            </p>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'Items',
  data () {
    return {
      items: []
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
</style>
