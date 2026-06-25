<template>
  <div class="app-layout">
    <!-- Header tracking active arrays -->
    <AppHeader :totalProperties="filteredProperties.length" />

    <main class="container">
      <!-- Search Input Filter Module Layout -->
      <section class="controls-bar">
        <div class="search-wrapper">
          <input 
            v-model="searchQuery" 
            type="text" 
            placeholder="Search by title or location (e.g., Sea Point)..." 
            class="search-input"
          />
        </div>
        
        <div class="sort-wrapper">
          <label for="sort" class="sort-label">Sort By Price:</label>
          <select id="sort" v-model="sortBy" class="sort-select">
            <option value="default">Default</option>
            <option value="low-high">Low to High</option>
            <option value="high-low">High to Low</option>
          </select>
        </div>
      </section>

      <!-- Search Query Failure State Empty Feedback Block -->
      <div v-if="filteredProperties.length === 0" class="empty-state">
        <p>No properties found matching your criteria.</p>
      </div>

      <!-- Core Display Multi-Grid -->
      <section v-else class="property-grid">
        <PropertyCard 
          v-for="property in filteredProperties" 
          :key="property.id" 
          :property="property"
          @toggle-bookmark="toggleBookmark"
        />
      </section>
    </main>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue';
import PropertyCard from './components/PropertyCard.vue';

export default {
  name: 'App',
  components: { AppHeader, PropertyCard },
  data() {
    return {
      searchQuery: '',
      sortBy: 'default',
      properties: [
        { id: 1, title: 'Luxury Sea Point Penthouse', location: 'Sea Point, Cape Town', price: 4500, type: 'Apartment', available: true, image: 'https://unsplash.com', isBookmarked: false },
        { id: 2, title: 'Cozy Camps Beach Cottage', location: 'Camps Bay, Cape Town', price: 6200, type: 'House', available: false, image: 'https://unsplash.com', isBookmarked: false },
        { id: 3, title: 'Modern Studio in Central CBD', location: 'City Bowl, Cape Town', price: 1800, type: 'Studio', available: true, image: 'https://unsplash.com', isBookmarked: false },
        { id: 4, title: 'Quaint Green Point Loft', location: 'Green Point, Cape Town', price: 2900, type: 'Apartment', available: true, image: 'https://unsplash.com', isBookmarked: false },
        { id: 5, title: 'Vibrant Woodstock Art Villa', location: 'Woodstock, Cape Town', price: 2100, type: 'Villa', available: false, image: 'https://unsplash.com', isBookmarked: false },
        { id: 6, title: 'Exclusive Constantia Estate', location: 'Constantia, Cape Town', price: 9500, type: 'House', available: true, image: 'https://unsplash.com', isBookmarked: false }
      ]
    };
  },
  computed: {
    filteredProperties() {
      let result = [...this.properties];
      if (this.searchQuery.trim() !== '') {
        const query = this.searchQuery.toLowerCase();
        result = result.filter(prop => 
          prop.title.toLowerCase().includes(query) || prop.location.toLowerCase().includes(query)
        );
      }
      if (this.sortBy === 'low-high') result.sort((a, b) => a.price - b.price);
      else if (this.sortBy === 'high-low') result.sort((a, b) => b.price - a.price);
      return result;
    }
  },
  created() {
    const saved = localStorage.getItem('homes-beyond-bookmarks');
    if (saved) {
      const bookmarkedIds = JSON.parse(saved);
      this.properties.forEach(p => { if (bookmarkedIds.includes(p.id)) p.isBookmarked = true; });
    }
  },
  methods: {
    toggleBookmark(id) {
      const target = this.properties.find(p => p.id === id);
      if (target) {
        target.isBookmarked = !target.isBookmarked;
        const active = this.properties.filter(p => p.isBookmarked).map(p => p.id);
        localStorage.setItem('homes-beyond-bookmarks', JSON.stringify(active));
      }
    }
  }
};
</script>
