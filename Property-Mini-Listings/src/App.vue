<script setup>
import { ref, computed } from 'vue';
import PropertyCard from "./components/PropertyCard.vue";
import {propertiesData}from "./data/properties.js";

const search = ref('')
const sortOrder = ref('low')

const properties = ref(propertiesData)

const filteredProperties = computed(() => {

  let results = properties.value.filter(property =>
    property.title.toLowerCase().includes(search.value.toLowerCase()) ||
    property.location.toLowerCase().includes(search.value.toLowerCase())
  )

  results.sort((a,b)=>{
    return sortOrder.value === 'low'
      ? a.price - b.price
      : b.price - a.price
  })

  return results
})

const toggleFavourite = (id) => {
  const property = properties.value.find(p => p.id === id)

  if(property){
    property.favourite = !property.favourite
  }
}

const totalProperties = computed(() => properties.value.length)
</script>

<template>
  <div class="container">

    <header>
      <h1>Homes & Beyond</h1>
      <p>{{ totalProperties }} Properties Available</p>
    </header>

    <section class="controls">

      <input
        v-model="search"
        placeholder="Search property..."
      >

      <select v-model="sortOrder">
        <option value="low">
          Price Low to High
        </option>

        <option value="high">
          Price High to Low
        </option>
      </select>

    </section>

    <section class="grid">

      <PropertyCard
        v-for="property in filteredProperties"
        :key="property.id"
        :property="property"
        @toggleFavourite="toggleFavourite"
      />

    </section>

  </div>
</template>

<style>
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
}

body{
  font-family:Arial,sans-serif;
  background:#f5f5f5;
}

.container{
  max-width:1200px;
  margin:auto;
  padding:2rem;
}

header{
  text-align:center;
  margin-bottom:2rem;
}

.controls{
  display:flex;
  gap:1rem;
  margin-bottom:2rem;
}

.controls input,
.controls select{
  padding:10px;
  flex:1;
}

.grid{
  display:grid;
  grid-template-columns:
  repeat(auto-fit,minmax(280px,1fr));
  gap:1.5rem;
}
</style>
