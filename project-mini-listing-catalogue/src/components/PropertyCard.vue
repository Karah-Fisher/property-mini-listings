<template>
  <div class="card" :class="{ 'card-unavailable': !property.available }">
    
    <!-- Ribbon Flag Element -->
    <div v-if="!property.available" class="badge-ribbon">
      Not Available
    </div>

    <!-- Image Wrapper Window -->
    <div class="image-window">
      <img :src="property.image" :alt="property.title" class="property-image" />
      
      <!-- Heart Vector Path Button Box -->
      <button 
        @click="$emit('toggle-bookmark', property.id)"
        class="bookmark-btn"
        :class="{ 'bookmark-active': property.isBookmarked }"
        aria-label="Toggle favourite"
      >
      <svg xmlns-xlink="http://w3.org" width="18" height="18" viewBox="0 0 24 24" fill="currentColor" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"/></svg>
      </button>
    </div>

    <!-- Info Descriptive Core Segment Block -->
    <div class="content-block">
      <div>
        <div class="meta-row">
          <span class="type-tag">{{ property.type }}</span>
          <span class="location-tag">📍 {{ property.location.split(',')[0] }}</span>
        </div>
        <h3 class="title-text">{{ property.title }}</h3>
      </div>
      
      <div class="footer-row">
        <div class="price-box">
          <span class="amount">R{{ property.price }}</span>
          <span class="label"> / night</span>
        </div>
        <button :disabled="!property.available" class="action-btn">
          Book Now
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PropertyCard',
  props: {
    property: { type: Object, required: true }
  }
};
</script>
