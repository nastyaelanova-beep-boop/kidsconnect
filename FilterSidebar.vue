<template>
  <div class="filter-wrapper">
    <h2 class="filters-title">Фильтры</h2>
    <div class="filters-sidebar">
      
      <div class="filter-group">
        <h4 class="filter-group-title">Возраст</h4>
        <div class="age-selector" @click="toggleAgeDropdown">
          <div class="age-selected">
            {{ getAgeLabel(localAge) }}
            <span class="age-arrow">▼</span>
          </div>
          <div class="age-dropdown" v-if="ageDropdownOpen">
            <div 
              class="age-option" 
              v-for="age in ageOptions" 
              :key="age.value"
              :class="{ active: localAge === age.value }"
              @click.stop="selectAge(age.value)"
            >
              {{ age.label }}
            </div>
          </div>
        </div>
      </div>

      <div class="filter-divider"></div>

      <div class="filter-group">
        <h4 class="filter-group-title">Пол</h4>
        <label class="filter-option checkbox">
          <input type="checkbox" value="male" v-model="localGender" />
          <span class="checkmark"></span>
          Мужской
        </label>
        <label class="filter-option checkbox">
          <input type="checkbox" value="female" v-model="localGender" />
          <span class="checkmark"></span>
          Женский
        </label>
      </div>

      <div class="filter-divider"></div>

      <div class="filter-group">
        <h4 class="filter-group-title">Каталог</h4>
        <div 
          class="filter-catalog-item" 
          v-for="cat in categories" 
          :key="cat.name"
          :class="{ active: localCategory === cat.name }"
          @click="selectCategory(cat.name)"
        >
          <span>{{ cat.name }}</span>
          <span class="count">{{ cat.count || 0 }}</span>
        </div>
        <div class="filter-divider"></div>
        <div class="filter-catalog-item reset-filter" @click="resetFilters">
          <span style="color: #681D29; font-weight: 600;">✕ Сбросить фильтр</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  age: { type: String, default: 'any' },
  gender: { type: Array, default: () => [] },
  category: { type: String, default: '' }
})

const emit = defineEmits(['update:age', 'update:gender', 'update:category'])

const localAge = ref(props.age)
const ageDropdownOpen = ref(false)

const ageOptions = [
  { value: 'any', label: 'Любой' },
  { value: '6-10', label: '6-10 лет' },
  { value: '11-14', label: '11-14 лет' },
  { value: '15-18', label: '15-18 лет' },
  { value: '19-25', label: '19-25 лет' },
  { value: '26-35', label: '26-35 лет' },
  { value: '36-45', label: '36-45 лет' },
  { value: '46-55', label: '46-55 лет' },
  { value: '56-65', label: '56-65 лет' }
]

const getAgeLabel = (value) => {
  const found = ageOptions.find(opt => opt.value === value)
  return found ? found.label : 'Любой'
}

const toggleAgeDropdown = () => {
  ageDropdownOpen.value = !ageDropdownOpen.value
}

const selectAge = (value) => {
  localAge.value = value
  ageDropdownOpen.value = false
}

const localGender = ref([...props.gender])

const localCategory = ref(props.category)

const categories = ref([
  { name: 'Силовой спорт', count: 4 },
  { name: 'Единоборства', count: 2 },
  { name: 'ДПИ и ремесла', count: 0 },
  { name: 'Техническое конструирование', count: 0 },
  { name: 'Словесность', count: 0 },
  { name: 'Иностранные языки', count: 0 },
  { name: 'Развитие интеллекта', count: 0 },
  { name: 'Информационные технологии', count: 0 },
  { name: 'История и традиции', count: 0 },
  { name: 'Педагогика', count: 0 },
  { name: 'Музыка и звук', count: 0 },
  { name: 'Пение', count: 0 },
  { name: 'Хореография (танцы)', count: 0 },
  { name: 'Зрелищные искусства', count: 0 },
  { name: 'Мода и стиль', count: 0 },
  { name: 'Познавательные развлечения', count: 0 },
  { name: 'Туризм', count: 0 },
  { name: 'Естественные науки', count: 0 },
  { name: 'Люди и животные', count: 0 },
  { name: 'Эстетические виды спорта', count: 0 },
  { name: 'Технические виды спорта', count: 0 },
  { name: 'Командно-игровой спорт', count: 0 },
  { name: 'Индивидуально-игровой спорт', count: 0 },
  { name: 'Водные виды спорта', count: 0 },
  { name: 'Легкая атлетика и гимнастика', count: 0 },
  { name: 'Физкультура', count: 0 }
])

watch(localAge, (val) => emit('update:age', val))
watch(localGender, (val) => emit('update:gender', val))
watch(localCategory, (val) => emit('update:category', val))

const selectCategory = (name) => {
  localCategory.value = localCategory.value === name ? '' : name
}

const resetFilters = () => {
  localAge.value = 'any'
  localGender.value = []
  localCategory.value = ''
}
</script>