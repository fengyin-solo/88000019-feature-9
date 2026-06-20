<script setup>
import { ref } from 'vue'
import { formatTime, priorityMeta } from '../../utils/formatters'

const props = defineProps({
  items: {
    type: Array,
    required: true,
  },
})

const expandedKeys = ref(new Set())

const toggleExpand = (key) => {
  const next = new Set(expandedKeys.value)
  if (next.has(key)) {
    next.delete(key)
  } else {
    next.add(key)
  }
  expandedKeys.value = next
}

const isExpanded = (key) => expandedKeys.value.has(key)
</script>

<template>
  <div class="window-list">
    <article
      v-for="item in items"
      :key="`${item.shipName}-${item.startAt}`"
      class="window-card"
      :class="{ 'window-card--expanded': isExpanded(`${item.shipName}-${item.startAt}`) }"
    >
      <button
        type="button"
        class="window-card__header"
        @click="toggleExpand(`${item.shipName}-${item.startAt}`)"
      >
        <div class="window-card__summary">
          <div class="window-topline">
            <h4>{{ item.shipName }}</h4>
            <span :class="['tone-pill', `tone-pill--${priorityMeta(item.priority).tone}`]">
              {{ priorityMeta(item.priority).label }}
            </span>
          </div>
        </div>
        <div class="window-card__time">
          <strong>{{ formatTime(item.startAt) }}</strong>
          <svg
            class="window-card__chevron"
            :class="{ 'window-card__chevron--rotated': isExpanded(`${item.shipName}-${item.startAt}`) }"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <polyline points="6 9 12 15 18 9" />
          </svg>
        </div>
      </button>

      <Transition name="collapse">
        <div
          v-if="isExpanded(`${item.shipName}-${item.startAt}`)"
          class="window-card__details"
        >
          <div class="detail-row">
            <span class="detail-label">泊位</span>
            <span class="detail-value">{{ item.berth }}</span>
          </div>
          <div class="detail-row">
            <span class="detail-label">舱位</span>
            <span class="detail-value">{{ item.tanks.join('、') }}</span>
          </div>
          <div class="detail-row">
            <span class="detail-label">执行人</span>
            <span class="detail-value">{{ item.inspector }}</span>
          </div>
          <div v-if="item.note" class="detail-row detail-row--note">
            <span class="detail-label">备注</span>
            <small class="detail-value">{{ item.note }}</small>
          </div>
        </div>
      </Transition>
    </article>
  </div>
</template>

<style scoped>
.window-list {
  display: grid;
  gap: 14px;
}

.window-card {
  border-radius: 20px;
  background: #f4f7f4;
  overflow: hidden;
}

.window-card__header {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 14px;
  align-items: center;
  width: 100%;
  padding: 16px 18px;
  border: none;
  background: transparent;
  cursor: pointer;
  text-align: left;
  transition: background-color 0.2s ease;
}

.window-card__header:hover {
  background: rgba(255, 255, 255, 0.5);
}

.window-card__summary {
  min-width: 0;
}

.window-card__time {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-shrink: 0;
}

.window-topline {
  display: flex;
  justify-content: space-between;
  gap: 12px;
  align-items: center;
}

h4 {
  margin: 0;
  font-size: 1rem;
}

.window-card__time strong {
  font-size: 1.1rem;
  color: #1d2b2a;
}

.window-card__chevron {
  width: 18px;
  height: 18px;
  color: #5f7670;
  flex-shrink: 0;
  transition: transform 0.3s ease;
}

.window-card__chevron--rotated {
  transform: rotate(180deg);
}

.window-card__details {
  padding: 0 18px 16px;
}

.detail-row {
  display: grid;
  grid-template-columns: 60px 1fr;
  gap: 10px;
  align-items: start;
  padding: 8px 0;
  border-top: 1px solid rgba(95, 118, 112, 0.12);
}

.detail-row:first-child {
  border-top: none;
  padding-top: 12px;
}

.detail-label {
  font-size: 0.82rem;
  color: #8a9e98;
  flex-shrink: 0;
}

.detail-value {
  color: #1d2b2a;
  word-break: break-all;
}

small.detail-value {
  color: #5f7670;
}

.tone-pill {
  padding: 6px 10px;
  border-radius: 999px;
  font-size: 0.76rem;
}

.tone-pill--danger {
  background: #f7ddd7;
}

.tone-pill--warn {
  background: #fff1d2;
}

.tone-pill--neutral {
  background: #dce8f7;
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
  padding-top: 0;
  padding-bottom: 0;
}

.collapse-enter-to,
.collapse-leave-from {
  opacity: 1;
  max-height: 300px;
}

@media (max-width: 768px) {
  .window-list {
    gap: 10px;
  }

  .window-card__header {
    padding: 12px 14px;
    gap: 10px;
  }

  .window-card__details {
    padding: 0 14px 12px;
  }

  .detail-row {
    grid-template-columns: 52px 1fr;
    gap: 8px;
    padding: 6px 0;
  }

  .detail-row:first-child {
    padding-top: 10px;
  }

  .window-topline {
    gap: 8px;
    flex-wrap: wrap;
  }

  h4 {
    font-size: 0.95rem;
  }

  .window-card__time strong {
    font-size: 1rem;
  }

  .tone-pill {
    padding: 4px 8px;
    font-size: 0.7rem;
  }
}
</style>
