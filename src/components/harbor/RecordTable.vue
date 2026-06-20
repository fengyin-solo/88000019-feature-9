<script setup>
import { statusMeta } from '../../utils/formatters'

defineProps({
  rows: {
    type: Array,
    required: true,
  },
})
</script>

<template>
  <div class="record-table">
    <div class="record-head">
      <span>船名</span>
      <span>样本标签</span>
      <span>状态</span>
      <span>执行人</span>
      <span>备注</span>
    </div>
    <div
      v-for="row in rows"
      :key="row.sampleCode"
      class="record-row"
    >
      <span class="cell cell--ship">{{ row.shipName }}</span>
      <span class="cell cell--sample">{{ row.sampleCode }}</span>
      <span class="cell cell--status">
        <span :class="['status-chip', `status-chip--${statusMeta(row.status).tone}`]">
          {{ statusMeta(row.status).label }}
        </span>
      </span>
      <span class="cell cell--assignee">{{ row.assignee }}</span>
      <span class="cell cell--note">{{ row.note }}</span>
    </div>
  </div>
</template>

<style scoped>
.record-table {
  overflow: hidden;
  border-radius: 18px;
  border: 1px solid rgba(29, 43, 42, 0.1);
}

.record-head {
  display: grid;
  grid-template-columns: 1.1fr 1fr 0.8fr 0.8fr 1.5fr;
  gap: 12px;
  padding: 14px 16px;
  background: #e6efe8;
  color: #49655d;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-size: 0.76rem;
}

.record-row {
  display: grid;
  grid-template-columns: 1.1fr 1fr 0.8fr 0.8fr 1.5fr;
  gap: 12px;
  padding: 14px 16px;
  background: rgba(255, 255, 255, 0.72);
}

.record-row + .record-row {
  border-top: 1px solid rgba(29, 43, 42, 0.08);
}

.cell {
  display: flex;
  align-items: center;
}

.status-chip {
  display: inline-flex;
  justify-content: center;
  width: fit-content;
  padding: 6px 10px;
  border-radius: 999px;
}

.status-chip--success {
  background: #d6efe0;
  color: #226040;
}

.status-chip--warn {
  background: #ffe9bf;
  color: #8b5e08;
}

.status-chip--neutral {
  background: #dde6f6;
  color: #35528b;
}

@media (max-width: 768px) {
  .record-table {
    display: flex;
    flex-direction: column;
    gap: 12px;
    background: transparent;
    border: none;
    border-radius: 0;
    overflow: visible;
  }

  .record-head {
    display: none;
  }

  .record-row {
    display: grid;
    grid-template-columns: auto 1fr;
    grid-template-areas:
      "ship sample"
      "status status"
      "assignee assignee"
      "note note";
    gap: 10px;
    padding: 16px;
    background: rgba(255, 255, 255, 0.9);
    border-radius: 16px;
    border: 1px solid rgba(29, 43, 42, 0.1);
  }

  .record-row + .record-row {
    border-top: none;
  }

  .cell--ship {
    grid-area: ship;
    font-weight: 600;
    font-size: 1rem;
    color: #1d2b2a;
  }

  .cell--sample {
    grid-area: sample;
    justify-content: flex-end;
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
    font-size: 0.88rem;
    color: #49655d;
  }

  .cell--status {
    grid-area: status;
    justify-content: flex-start;
  }

  .cell--status .status-chip {
    font-weight: 600;
    font-size: 0.88rem;
    padding: 8px 14px;
  }

  .cell--assignee {
    grid-area: assignee;
    font-size: 0.9rem;
    color: #49655d;
  }

  .cell--assignee::before {
    content: '执行人：';
    color: #7a8f88;
  }

  .cell--note {
    grid-area: note;
    font-size: 0.88rem;
    color: #5a6f68;
    line-height: 1.5;
    padding-top: 8px;
    border-top: 1px dashed rgba(29, 43, 42, 0.1);
  }
}
</style>
