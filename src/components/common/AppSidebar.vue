<script setup>
defineProps({
  title: {
    type: String,
    required: true,
  },
  subtitle: {
    type: String,
    required: true,
  },
  items: {
    type: Array,
    required: true,
  },
})

const IconOverview = {
  template: `
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <rect x="3" y="3" width="7" height="9" rx="1" />
      <rect x="14" y="3" width="7" height="5" rx="1" />
      <rect x="14" y="12" width="7" height="9" rx="1" />
      <rect x="3" y="16" width="7" height="5" rx="1" />
    </svg>
  `,
}

const IconWindow = {
  template: `
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <rect x="3" y="4" width="18" height="16" rx="2" />
      <path d="M3 9h18" />
      <path d="M8 4v5" />
      <circle cx="6" cy="6.5" r="0.5" fill="currentColor" />
      <circle cx="10" cy="6.5" r="0.5" fill="currentColor" />
    </svg>
  `,
}

const IconRecord = {
  template: `
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z" />
      <polyline points="14 2 14 8 20 8" />
      <line x1="8" y1="13" x2="16" y2="13" />
      <line x1="8" y1="17" x2="16" y2="17" />
      <line x1="8" y1="9" x2="10" y2="9" />
    </svg>
  `,
}

const iconMap = {
  overview: IconOverview,
  window: IconWindow,
  record: IconRecord,
}
</script>

<template>
  <aside class="sidebar">
    <div class="brand">
      <p class="brand-kicker">{{ title }}</p>
      <h1>{{ subtitle }}</h1>
    </div>
    <nav class="nav">
      <RouterLink
        v-for="item in items"
        :key="item.to"
        :to="item.to"
        class="nav-link"
      >
        <component :is="iconMap[item.icon]" class="nav-icon" v-if="iconMap[item.icon]" />
        <span>{{ item.label }}</span>
      </RouterLink>
    </nav>
  </aside>

  <nav class="tab-bar">
    <RouterLink
      v-for="item in items"
      :key="item.to"
      :to="item.to"
      class="tab-link"
    >
      <component :is="iconMap[item.icon]" class="tab-icon" v-if="iconMap[item.icon]" />
      <span class="tab-label">{{ item.label }}</span>
    </RouterLink>
  </nav>
</template>

<style scoped>
.sidebar {
  width: 240px;
  padding: 28px 20px;
  border-right: 1px solid rgba(29, 43, 42, 0.12);
  background: rgba(244, 249, 246, 0.9);
  position: sticky;
  top: 0;
  height: 100vh;
}

.brand-kicker {
  margin: 0 0 8px;
  text-transform: uppercase;
  letter-spacing: 0.16em;
  color: #4a6a5f;
  font-size: 0.76rem;
}

.brand h1 {
  margin: 0;
  font-size: 1.5rem;
  line-height: 1.1;
}

.nav {
  display: grid;
  gap: 10px;
  margin-top: 28px;
}

.nav-link {
  padding: 12px 14px;
  border-radius: 14px;
  color: #35524c;
  text-decoration: none;
  background: rgba(255, 255, 255, 0.72);
  display: flex;
  align-items: center;
  gap: 12px;
}

.nav-icon {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}

.nav-link.router-link-active {
  background: #22372f;
  color: #eff6f1;
}

.tab-bar {
  display: none;
}

@media (max-width: 980px) {
  .sidebar {
    width: auto;
    height: auto;
    position: static;
    border-right: none;
    border-bottom: 1px solid rgba(29, 43, 42, 0.12);
  }

  .nav {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }
}

@media (max-width: 768px) {
  .sidebar {
    display: none;
  }

  .tab-bar {
    display: flex;
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(255, 255, 255, 0.98);
    border-top: 1px solid rgba(29, 43, 42, 0.12);
    backdrop-filter: blur(12px);
    z-index: 100;
    padding-bottom: env(safe-area-inset-bottom, 0);
  }

  .tab-link {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 8px 0 10px;
    color: #6b8a7f;
    text-decoration: none;
    gap: 4px;
  }

  .tab-icon {
    width: 24px;
    height: 24px;
  }

  .tab-label {
    font-size: 0.72rem;
    line-height: 1;
  }

  .tab-link.router-link-active {
    color: #22372f;
  }

  .tab-link.router-link-active .tab-icon {
    color: #22372f;
  }
}

@media (max-width: 680px) {
  .nav {
    grid-template-columns: 1fr;
  }
}
</style>
