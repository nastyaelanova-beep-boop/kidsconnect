<template>
  <div id="integrator">
    <template v-if="!showSection">
      <Header />
      
      <div class="main">
        <div class="top-nav">
          <div class="nav-left">
            <div class="search-wrapper">
              <svg class="search-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M15.5 14H14.71L14.43 13.73C15.41 12.59 16 11.11 16 9.5C16 5.91 13.09 3 9.5 3C5.91 3 3 5.91 3 9.5C3 13.09 5.91 16 9.5 16C11.11 16 12.59 15.41 13.73 14.43L14 14.71V15.5L19 20.49L20.49 19L15.5 14ZM9.5 14C7.01 14 5 11.99 5 9.5C5 7.01 7.01 5 9.5 5C11.99 5 14 7.01 14 9.5C14 11.99 11.99 14 9.5 14Z" fill="currentColor"/>
              </svg>
              <input 
                type="text" 
                class="search-input" 
                placeholder="Поиск"
                v-model="searchQuery"
                @input="applyFilters"
              />
            </div>
            
            <button 
              class="filter-btn" 
              :class="{ active: selectedFilter === 'all' }"
              @click="setFilter('all')"
            >
              Все
            </button>
            <button 
              class="filter-btn" 
              :class="{ active: selectedFilter === 'free' }"
              @click="setFilter('free')"
            >
              Бесплатные
            </button>
            <button 
              class="filter-btn" 
              :class="{ active: selectedFilter === 'paid' }"
              @click="setFilter('paid')"
            >
              Платные
            </button>
          </div>
          
          <button class="map-btn" @click="openMap">
            <svg class="map-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M12 2C8.13 2 5 5.13 5 9C5 13.17 9.42 18.92 11.24 21.11C11.64 21.59 12.37 21.59 12.77 21.11C14.58 18.92 19 13.17 19 9C19 5.13 15.87 2 12 2ZM12 11.5C10.62 11.5 9.5 10.38 9.5 9C9.5 7.62 10.62 6.5 12 6.5C13.38 6.5 14.5 7.62 14.5 9C14.5 10.38 13.38 11.5 12 11.5Z" fill="currentColor"/>
            </svg>
            На карте
          </button>
        </div>
        
        <div class="content-wrapper">
          <div class="main-content">
            <div class="section">
              <div class="section-title">Силовой спорт</div>
              <CourseCard 
                v-for="card in filteredCards" 
                :key="card.id" 
                :card="card"
                @detail="goToSection"
              />
            </div>
          </div>
          <FilterSidebar 
            v-model:age="selectedAge"
            v-model:gender="selectedGender"
            v-model:category="selectedCategory"
          />
        </div>
      </div>
    </template>
   
    <SectionView v-else :id="selectedSectionId" />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import Header from './components/Header.vue'
import CourseCard from './components/CourseCard.vue'
import FilterSidebar from './components/FilterSidebar.vue'
import SectionView from './views/SectionView.vue'
import { courses } from './data/courses.js'

const cards = ref(courses)

const currentPage = ref('home')
const selectedSectionId = ref(null)
const showSection = computed(() => currentPage.value === 'section')

const searchQuery = ref('')
const selectedFilter = ref('all')
const selectedAge = ref('any')
const selectedGender = ref([])
const selectedCategory = ref('')

const setFilter = (filter) => {
  selectedFilter.value = filter
}

const applyFilters = () => {}

const goToSection = (id) => {
  selectedSectionId.value = id
  currentPage.value = 'section'
  window.scrollTo(0, 0)
}

const openMap = () => {
  window.open('https://yandex.ru/maps/63/irkutsk/', '_blank')
}
const getCardsByCategory = (category) => {
  return filteredCards.value.filter(card => card.category === category)
}
const filteredCards = computed(() => {
  return cards.value.filter(card => {
    const searchText = searchQuery.value.toLowerCase().trim()
    if (searchText) {
      const title = card.title.toLowerCase()
      const address = card.address.toLowerCase()
      const tag = card.tag.toLowerCase()
      if (!title.includes(searchText) && !address.includes(searchText) && !tag.includes(searchText)) {
        return false
      }
    }
    
    if (selectedFilter.value === 'free' && !card.isFree) return false
    if (selectedFilter.value === 'paid' && card.isFree) return false
    
    if (selectedCategory.value && card.category !== selectedCategory.value) return false
    
    if (selectedAge.value !== 'any') {
      const ageRange = selectedAge.value.split('-')
      const minAge = parseInt(ageRange[0])
      const maxAge = parseInt(ageRange[1])
      const ageMatch = card.age.match(/(\d+)-(\d+)/)
      if (ageMatch) {
        const cardMinAge = parseInt(ageMatch[1])
        const cardMaxAge = parseInt(ageMatch[2])
        if (cardMaxAge < minAge || cardMinAge > maxAge) return false
      }
    }
    
    return true
  })
})
</script>
