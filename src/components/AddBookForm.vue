<template>
  <div class="card mb-4 shadow-sm">
    <div class="card-header bg-primary text-white">
      <h5 class="mb-0">
        <i class="bi bi-plus-circle"></i> Добавить новую книгу
      </h5>
    </div>
    <div class="card-body">
      <form @submit.prevent="handleSubmit">
        <div class="row">
          <div class="col-md-6 mb-3">
            <label class="form-label">Название книги *</label>
            <input
              v-model="formData.title"
              type="text"
              class="form-control"
              placeholder="Введите название"
              required
            />
          </div>
          <div class="col-md-6 mb-3">
            <label class="form-label">Автор *</label>
            <input
              v-model="formData.author"
              type="text"
              class="form-control"
              placeholder="Введите автора"
              required
            />
          </div>
        </div>

        <div class="row">
          <div class="col-md-6 mb-3">
            <label class="form-label">Жанр</label>
            <select v-model="formData.genre" class="form-select">
              <option value="">Выберите жанр</option>
              <option value="Роман">Роман</option>
              <option value="Фантастика">Фантастика</option>
              <option value="Детектив">Детектив</option>
              <option value="Научная">Научная</option>
              <option value="Поэзия">Поэзия</option>
              <option value="Биография">Биография</option>
              <option value="Другое">Другое</option>
            </select>
          </div>
          <div class="col-md-6 mb-3">
            <label class="form-label">URL обложки</label>
            <input
              v-model="formData.imageUrl"
              type="url"
              class="form-control"
              <input v-model="formData.imageUrl" placeholder="harrypotter.jpg" />
            />
            <div class="form-text">Оставьте пустым, если нет фото</div>
          </div>
        </div>

        <div class="mb-3">
          <label class="form-label">Описание</label>
          <textarea
            v-model="formData.description"
            class="form-control"
            rows="3"
            placeholder="Краткое описание книги..."
          ></textarea>
        </div>

        <button type="submit" class="btn btn-primary w-100">
          <i class="bi bi-check-lg"></i> Добавить книгу
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['add-book'])

const formData = reactive({
  title: '',
  author: '',
  genre: '',
  description: '',
  imageUrl: ''
})

const handleSubmit = () => {
  emit('add-book', { ...formData })
  // Очистка формы
  formData.title = ''
  formData.author = ''
  formData.genre = ''
  formData.description = ''
  formData.imageUrl = ''
}
</script>