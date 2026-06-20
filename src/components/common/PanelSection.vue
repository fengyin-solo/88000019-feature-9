<script setup>
import { ref } from 'vue'

defineProps({
  title: {
    type: String,
    required: true,
  },
  badge: {
    type: String,
    default: '',
  },
})

const isExpanded = ref(true)

const toggle = () => {
  isExpanded.value = !isExpanded.value
}
</script>

<template>
  <section class="panel-section">
    <header class="panel-head" @click="toggle">
      <div class="panel-head-left">
        <h3>{{ title }}</h3>
        <span v-if="badge" class="badge">{{ badge }}</span>
      </div>
      <svg
        class="chevron"
        :class="{ 'chevron--expanded': isExpanded }"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      >
        <polyline points="6 9 12 15 18 9" />
      </svg>
    </header>
    <Transition name="collapse">
      <div v-show="isExpanded" class="panel-content">
        <slot />
      </div>
    </Transition>
  </section>
</template>

<style scoped>
.panel-section {
  border: 1px solid rgba(29, 43, 42, 0.1);
  border-radius: 24px;
  background: rgba(255, 253, 248, 0.88);
  padding: 22px;
  box-shadow: 0 16px 40px rgba(53, 74, 64, 0.08);
}

.panel-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  margin-bottom: 18px;
  cursor: pointer;
  user-select: none;
}

.panel-head-left {
  display: flex;
  align-items: center;
  gap: 12px;
  flex: 1;
  flex-wrap: wrap;
}

h3 {
  margin: 0;
  font-size: 1.1rem;
}

.badge {
  padding: 7px 11px;
  border-radius: 999px;
  background: #edf3ef;
  color: #4a6259;
  font-size: 0.78rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

.chevron {
  width: 20px;
  height: 20px;
  color: #4a6259;
  transition: transform 0.3s ease;
  flex-shrink: 0;
}

.chevron--expanded {
  transform: rotate(180deg);
}

.collapse-enter-active,
.collapse-leave-active {
  transition: all 0.3s ease;
  overflow: hidden;
}

.collapse-enter-from,
.collapse-leave-to {
  opacity: 0;
  max-height: 0;
}

.collapse-enter-to,
.collapse-leave-from {
  opacity: 1;
  max-height: 1000px;
}

@media (max-width: 768px) {
  .panel-section {
    padding: 16px;
    border-radius: 18px;
  }

  .panel-head {
    margin-bottom: 14px;
  }

  h3 {
    font-size: 1rem;
  }
}
</style>
