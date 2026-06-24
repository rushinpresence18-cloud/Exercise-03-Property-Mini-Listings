<script setup>
defineProps({
  property: Object
})

const emit = defineEmits(['toggleFavourite'])
</script>

<template>
  <div class="card">
    <div class="image-container">
      <img :src="property.image" :alt="property.title">

      <span
        v-if="!property.available"
        class="badge"
      >
        Not Available
      </span>
    </div>

    <div class="card-content">
      <h3>{{ property.title }}</h3>

      <p>{{ property.location }}</p>

      <p>{{ property.type }}</p>

      <h4>R{{ property.price }} / month</h4>

      <button @click="emit('toggleFavourite', property.id)">
        {{ property.favourite ? '★ Saved' : '☆ Save' }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.card{
  background:white;
  border-radius:12px;
  overflow:hidden;
  box-shadow:0 0 10px rgba(0,0,0,.1);
}

.card img{
  width:100%;
  height:200px;
  object-fit:cover;
}

.card-content{
  padding:1rem;
}

.badge{
  position:absolute;
  top:10px;
  right:10px;
  background:red;
  color:white;
  padding:5px 10px;
  border-radius:5px;
}

.image-container{
  position:relative;
}
</style>