<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

defineProps({
  eyebrow: {
    type: String,
    required: true,
  },
  title: {
    type: String,
    required: true,
  },
  description: {
    type: String,
    required: true,
  },
})

const isMobile = ref(false)
const isExpanded = ref(false)

let mediaQuery = null

const checkMobile = () => {
  if (mediaQuery) {
    isMobile.value = mediaQuery.matches
    if (!isMobile.value) {
      isExpanded.value = false
    }
  }
}

const toggleDescription = () => {
  if (isMobile.value) {
    isExpanded.value = !isExpanded.value
  }
}

onMounted(() => {
  mediaQuery = window.matchMedia('(max-width: 768px)')
  checkMobile()
  mediaQuery.addEventListener('change', checkMobile)
})

onUnmounted(() => {
  if (mediaQuery) {
    mediaQuery.removeEventListener('change', checkMobile)
  }
})
</script>

<template>
  <header class="page-header">
    <p class="eyebrow">{{ eyebrow }}</p>
    <h2 @click="toggleDescription" :class="{ 'clickable': isMobile }">
      {{ title }}
      <span v-if="isMobile" class="toggle-icon">{{ isExpanded ? '−' : '+' }}</span>
    </h2>
    <p class="description" :class="{ 'expanded': isExpanded }">{{ description }}</p>
  </header>
</template>

<style scoped>
.page-header {
  margin-bottom: 24px;
}

.eyebrow {
  margin: 0 0 10px;
  text-transform: uppercase;
  letter-spacing: 0.22em;
  font-size: 0.78rem;
  color: #4a6a5f;
}

h2 {
  margin: 0;
  font-size: clamp(2rem, 4vw, 3.4rem);
  line-height: 0.98;
}

h2.clickable {
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
}

.toggle-icon {
  font-size: 1.2rem;
  font-weight: normal;
  color: #4a6a5f;
}

.description {
  margin: 14px 0 0;
  max-width: 52rem;
  color: #46605a;
}

@media (max-width: 768px) {
  .page-header {
    margin-bottom: 16px;
  }

  .eyebrow {
    display: none;
  }

  h2 {
    font-size: clamp(1.4rem, 6vw, 2rem);
    line-height: 1.1;
  }

  .description {
    max-height: 0;
    overflow: hidden;
    opacity: 0;
    margin: 0;
    transition: max-height 0.3s ease, opacity 0.3s ease, margin 0.3s ease;
  }

  .description.expanded {
    max-height: 500px;
    opacity: 1;
    margin-top: 10px;
  }
}
</style>
