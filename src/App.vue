<template>
  <div class="container mt-5">
    <header class="text-center mb-5">
      <h1 class="display-4 text-primary">
        <i class="bi bi-bookshelf"></i> Менеджер книг
      </h1>
      <p class="lead text-muted">Управляй своей библиотекой</p>
    </header>

    <main>
      <!-- Форма добавления книги -->
      <AddBookForm @add-book="addBook" />

      <!-- Фильтры, поиск и сортировка -->
      <BookFilters
        v-model:searchQuery="searchQuery"
        v-model:filter="currentFilter"
        v-model:sortBy="sortBy"
        :books="books"
      />

      <!-- Статистика -->
      <div class="alert alert-info mb-4">
        <i class="bi bi-graph-up"></i>
        <strong>Статистика:</strong> 
        Всего: {{ books.length }} | 
        Прочитано: {{ completedCount }} | 
        В избранном: {{ favoritesCount }} |
        Осталось: {{ books.length - completedCount }}
      </div>

      <!-- Сообщение, если книг нет -->
      <div v-if="filteredAndSortedBooks.length === 0" class="text-center py-5">
        <i class="bi bi-inbox display-1 text-muted"></i>
        <p class="lead mt-3">Книги не найдены</p>
        <p class="text-muted">Добавьте первую книгу или измените параметры поиска</p>
      </div>

      <!-- Список книг -->
      <div v-else class="row">
        <div
          v-for="book in filteredAndSortedBooks"
          :key="book.id"
          class="col-md-6 col-lg-4 mb-4"
        >
          <BookCard
            :book="book"
            @toggle="toggleBook(book.id)"
            @delete="deleteBook(book.id)"
            @rate="rateBook(book.id, $event)"
            @toggle-favorite="toggleFavorite(book.id)"
          />
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import AddBookForm from './components/AddBookForm.vue'
import BookFilters from './components/BookFilters.vue'
import BookCard from './components/BookCard.vue'

// Состояние книг
const books = ref([])

// Загрузка из localStorage
const savedBooks = localStorage.getItem('books')
if (savedBooks) {
  books.value = JSON.parse(savedBooks)
}

// Фильтры и сортировка
const currentFilter = ref('all')
const searchQuery = ref('')
const sortBy = ref('date') // 'date', 'title', 'rating'

// Сохранение в localStorage
watch(books, (newBooks) => {
  localStorage.setItem('books', JSON.stringify(newBooks))
}, { deep: true })

// Добавление книги
const addBook = (bookData) => {
  const newBook = {
    id: Date.now(),
    ...bookData,
    completed: false,
    rating: 0,
    favorite: false,
    createdAt: new Date().toISOString()
  }
  books.value.push(newBook)
}

// Переключение статуса прочтения
const toggleBook = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) {
    book.completed = !book.completed
    if (!book.completed) {
      book.rating = 0
    }
  }
}

// Оценка книги
const rateBook = (id, rating) => {
  const book = books.value.find(b => b.id === id)
  if (book && book.completed) {
    book.rating = rating
  }
}

// Удаление книги
const deleteBook = (id) => {
  if (confirm('Удалить книгу?')) {
    books.value = books.value.filter(b => b.id !== id)
  }
}

// Избранное
const toggleFavorite = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) {
    book.favorite = !book.favorite
  }
}

// Статистика
const completedCount = computed(() => 
  books.value.filter(b => b.completed).length
)

const favoritesCount = computed(() => 
  books.value.filter(b => b.favorite).length
)

// Фильтрация и сортировка
const filteredAndSortedBooks = computed(() => {
  let result = [...books.value]

  // Фильтр по статусу
  if (currentFilter.value === 'unread') {
    result = result.filter(book => !book.completed)
  } else if (currentFilter.value === 'read') {
    result = result.filter(book => book.completed)
  } else if (currentFilter.value === 'favorites') {
    result = result.filter(book => book.favorite)
  }

  // Поиск
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    result = result.filter(book => 
      book.title.toLowerCase().includes(query) ||
      book.author.toLowerCase().includes(query) ||
      (book.description && book.description.toLowerCase().includes(query))
    )
  }

  // Сортировка
  result.sort((a, b) => {
    if (sortBy.value === 'title') {
      return a.title.localeCompare(b.title)
    } else if (sortBy.value === 'rating') {
      return b.rating - a.rating
    } else if (sortBy.value === 'date') {
      return new Date(b.createdAt) - new Date(a.createdAt)
    }
    return 0
  })

  return result
})
</script>

<style>
body {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  padding-bottom: 50px;
}

.container {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20px;
  padding: 40px;
  margin-top: 30px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
}
</style>