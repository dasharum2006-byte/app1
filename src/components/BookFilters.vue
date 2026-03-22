<template>
  <div class="card mb-4 shadow-sm">
    <div class="card-body">
      <div class="row g-3">
        <!-- Поиск -->
        <div class="col-md-6">
          <div class="input-group">
            <span class="input-group-text">
              <i class="bi bi-search"></i>
            </span>
            <input
              v-model="searchQuery"
              type="text"
              class="form-control"
              placeholder="Поиск по названию, автору или описанию..."
            />
          </div>
        </div>

        <!-- Сортировка -->
        <div class="col-md-3">
          <select v-model="sortBy" class="form-select">
            <option value="date">По дате добавления</option>
            <option value="title">По названию (А-Я)</option>
            <option value="rating">По рейтингу</option>
          </select>
        </div>

        <!-- Фильтр -->
        <div class="col-md-3">
          <select v-model="filter" class="form-select">
            <option value="all">Все книги</option>
            <option value="read">Прочитанные</option>
            <option value="unread">Непрочитанные</option>
            <option value="favorites">Избранные</option>
          </select>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const filter = defineModel('filter')
const searchQuery = defineModel('searchQuery')
const sortBy = defineModel('sortBy')
</script>