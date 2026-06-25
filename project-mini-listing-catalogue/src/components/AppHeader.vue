<template>
  <header class="app-header">
    <div class="header-container">
      <div class="brand">
        <div class="brand-icon">H</div>
        <span>Homes<span class="brand-amp">&</span>Beyond</span>
      </div>

      <div class="counter-badge">
        Properties Found: <span>{{ totalProperties }}</span>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'AppHeader',
  props: {
    totalProperties: { type: Number, required: true }
  }
};
</script>
