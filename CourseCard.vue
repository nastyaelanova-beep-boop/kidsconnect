<template>
  <div class="card">
    <div class="card-inner">
      <div class="card-image">
        <img :src="card.image || '/src/assets/Group-330.png'" :alt="card.title" class="card-img" />
      </div>
      <div class="card-info-wrapper">
        <div class="card-header">
          <h3 class="card-title">{{ card.title }}</h3>
          <div class="card-price-tag" :class="card.isFree ? 'free' : 'paid'">
            {{ card.isFree ? 'Бесплатно' : card.price }}
          </div>
        </div>
        <div class="card-detail">
          <span class="detail-text-hashtag">#{{ card.tag }}</span>
        </div>
        <div class="card-detail">
          <img src="/src/assets/Frame 397.png" alt="👤" class="detail-img" />
          <span class="detail-text">{{ card.age }}</span>
        </div>
        <div class="card-detail">
          <img src="/src/assets/layer1.png" alt="📍" class="detail-img" />
          <span class="detail-text">{{ card.address }}</span>
        </div>
        <div class="card-detail">
          <img src="/src/assets/locationpointer_83774 1.png" alt="🏟️" class="detail-img" />
          <span class="detail-text">ФОК "Юбилейный"</span>
        </div>
        <div class="card-detail schedule-row">
          <img src="/src/assets/clock_time_icon_142903 1.png" alt="📅" class="detail-img" />
          <span class="detail-text">{{ card.schedule }}</span>
          <span class="schedule-time">{{ card.time || '09:00 - 19:30' }}</span>
          <button class="card-details-btn" @click="$emit('detail', card.id)">
            Подробнее
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  card: {
    type: Object,
    required: true
  }
})

defineEmits(['detail'])
</script>