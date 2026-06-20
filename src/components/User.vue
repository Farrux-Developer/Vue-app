<template>
  <div class="user-module-tile">
    <div class="tile-header">
      <div class="tile-id-badge">ID-{{ index + 1 }}</div>
      <h2 class="tile-name-panel">{{ user.name }}</h2>
    </div>
    
    <div class="tile-data-fields">
      <div class="data-field access-email">
        <span class="field-label">ACCESS</span>
        <span class="field-value">{{ user.email }}</span>
      </div>
      <div class="data-field registry-key">
        <span class="field-label">PASSWORD</span>
        <span class="field-value masked-value">{{ user.password.replace(/./g, 'X') }}</span>
      </div>
    </div>

    <div class="tile-tactile-controls">
      <button class="bezel-btn btn-actionable tile-control edit-control" @click="$emit('edit', index)">
        RECONFIGURE
      </button>
      <button class="bezel-btn btn-critical tile-control delete-control" @click="deleteUser(index)">
        TERMINATE
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    user: { type: Object, required: true },
    index: { type: Number, required: true },
    deleteUser: {
      type: Function,
      required: true,
    },
  },
};
</script>

<style scoped>
.user-module-tile {
  background: radial-gradient(circle at center, #14141a 0%, #0a0a0f 100%);
  border: 1px solid #1a1a20;
  border-radius: 12px;
  padding: 25px;
  transition: all 0.3s cubic-bezier(0.17, 0.88, 0.32, 1.28);
  position: relative;
  overflow: hidden;
  box-shadow: 
    8px 8px 16px rgba(0, 0, 0, 0.6),
    -8px -8px 16px rgba(255, 255, 255, 0.02),
    inset 1px 1px 2px rgba(255, 255, 255, 0.05),
    inset -1px -1px 2px rgba(0, 0, 0, 0.3);
}

.user-module-tile:hover {
  transform: translateY(-8px) scale(1.01);
  box-shadow: 
    12px 12px 24px rgba(0, 0, 0, 0.7),
    -12px -12px 24px rgba(255, 255, 255, 0.03),
    0 0 15px rgba(92, 132, 216, 0.15);
  border-color: #282830;
}

.tile-header {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
  border-bottom: 1px solid #1a1a20;
  padding-bottom: 15px;
}

.tile-id-badge {
  background: linear-gradient(180deg, #d8d8e0 0%, #a0a0b0 100%);
  color: #050508;
  font-weight: bold;
  padding: 5px 10px;
  border-radius: 4px;
  font-size: 0.8rem;
  margin-right: 15px;
  letter-spacing: 1px;
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  border: 1px solid #505060;
}

.tile-name-panel {
  margin: 0;
  font-size: 1.3rem;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: #f0f0f5;
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.7);
}

.tile-data-fields {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-bottom: 25px;
}

.data-field {
  display: flex;
  align-items: center;
  background-color: #060609;
  border: 1px solid #1a1a20;
  padding: 12px 15px;
  border-radius: 6px;
  box-shadow: 
    inset 2px 2px 5px rgba(0, 0, 0, 0.8),
    inset -2px -2px 5px rgba(255, 255, 255, 0.01);
}

.field-label {
  font-size: 0.75rem;
  color: #505060;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-right: 15px;
  width: 95px; /* Увеличено с 50px, чтобы слово PASSWORD не переносилось */
  flex-shrink: 0;
}

.field-value {
  color: #d1d1e0;
  font-size: 0.9rem;
  word-break: break-all;
  letter-spacing: 1px;
}

.masked-value {
  letter-spacing: 3px;
  color: #808090;
}

.tile-tactile-controls {
  display: flex;
  gap: 10px;
}

.bezel-btn.tile-control {
  padding: 10px 15px;
  font-size: 0.8rem;
  letter-spacing: 1.5px;
  border-radius: 4px;
}

.edit-control {
  background: linear-gradient(135deg, #1c1c2b 0%, #0e0e15 100%);
  border-color: #3c548c;
  color: #8fb1f3;
}

.delete-control {
  background: linear-gradient(135deg, #2b1c1c 0%, #150e0e 100%);
  border-color: #8c3c3c;
  color: #f38f8f;
}
</style>