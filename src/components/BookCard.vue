<template>
  <div class="card h-100 shadow-sm book-card" :class="{ 
    'border-success': book.completed,
    'border-warning': book.favorite && !book.completed,
    'completed': book.completed
  }">
    <!-- Обложка книги -->
    <img
      v-if="book.imageUrl"
      :src="book.imageUrl"
      class="card-img-top book-cover"
      :alt="book.title"
      @error="handleImageError"
    />
    <div v-else class="card-img-top book-cover-placeholder">
      <i class="bi bi-book display-1"></i>
    </div>

    <div class="card-body">
      <!-- Заголовок и избранное -->
      <div class="d-flex justify-content-between align-items-start mb-2">
        <h5 class="card-title mb-0">{{ book.title }}</h5>
        <button
          @click="$emit('toggle-favorite')"
          class="btn btn-link p-0"
          :class="book.favorite ? 'text-warning' : 'text-muted'"
        >
          <i :class="book.favorite ? 'bi bi-heart-fill' : 'bi bi-heart'"></i>
        </button>
      </div>

      <!-- Автор и жанр -->
      <p class="text-muted mb-1">
        <i class="bi bi-person"></i> {{ book.author }}
      </p>
      <span v-if="book.genre" class="badge bg-secondary mb-2">
        {{ book.genre }}
      </span>

      <!-- Описание -->
      <p v-if="book.description" class="card-text small text-muted">
        {{ book.description }}
      </p>

      <!-- Рейтинг -->
      <div v-if="book.completed" class="mb-3">
        <div class="rating">
          <span
            v-for="star in 5"
            :key="star"
            @click="$emit('rate', star)"
            class="star"
            :class="{ rated: star <= book.rating }"
          >
            <i :class="star <= book.rating ? 'bi bi-star-fill' : 'bi bi-star'"></i>
          </span>
          <span class="ms-2 text-muted small">({{ book.rating }}/5)</span>
        </div>
      </div>

      <!-- Статус -->
      <div class="mb-3">
        <span :class="book.completed ? 'badge bg-success' : 'badge bg-secondary'">
          <i :class="book.completed ? 'bi bi-check-circle' : 'bi bi-clock'"></i>
          {{ book.completed ? 'Прочитано' : 'Не прочитано' }}
        </span>
      </div>
    </div>

    <div class="card-footer bg-transparent">
      <div class="btn-group w-100">
        <button
          @click="$emit('toggle')"
          :class="book.completed ? 'btn btn-outline-secondary' : 'btn btn-success'"
        >
          <i :class="book.completed ? 'bi bi-arrow-counterclockwise' : 'bi bi-check-lg'"></i>
          {{ book.completed ? 'Не прочитано' : 'Прочитано' }}
        </button>
        <button
          @click="$emit('delete')"
          class="btn btn-outline-danger"
        >
          <i class="bi bi-trash"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate', 'toggle-favorite'])

const handleImageError = (e) => {
  e.target.style.display = 'none'
}
</script>

<style scoped>
.book-card {
  transition: all 0.3s ease;
  border: 2px solid transparent;
}

.book-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15) !important;
}

.book-card.completed {
  opacity: 0.9;
}

.book-cover {
  height: 250px;
  object-fit: cover;
  background: #f8f9fa;
}

.book-cover-placeholder {
  height: 250px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.star {
  font-size: 1.5rem;
  cursor: pointer;
  color: #dee2e6;
  transition: all 0.2s;
}

.star.rated {
  color: #076aff;
}

.star:hover {
  transform: scale(1.2);
}
</style>