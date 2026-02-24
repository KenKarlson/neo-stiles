<template>
  <div class="nb-container">
    <section :id="sectionId" class="download-box">
      <!-- Заголовок -->
      <h1>{{ title }}</h1>

      <!-- Описание -->
      <p>{{ description }}</p>

      <br />

      <NbButton
        :label="buttonText"
        variant="primary"
        :href="downloadUrl"
        :download="downloadFilename"
        @click="onButtonClick"
      />

      <!-- Блок инструкций (опциональный, можно скрыть через prop) -->
      <div v-if="showInstructions" class="instructions">
        <h3>{{ instructionsTitle }}</h3>

        <p v-for="(step, index) in instructions" :key="index">
          {{ step }}
        </p>

        <!-- Слот для кастомного контента (например, <pre> с кодом) -->
        <slot name="instructions-extra"></slot>
      </div>
    </section>
  </div>
</template>

<script setup>
import NbButton from "../button/NbButton.vue";
const props = defineProps({
  // ID секции (для якорных ссылок в меню)
  sectionId: {
    type: String,
    default: "install",
  },

  // Заголовок секции
  title: {
    type: String,
    default: "Добро пожаловать в NeoBrutality UI",
  },

  // Краткое описание
  description: {
    type: String,
    default: "Безопасный ретро-фреймворк с префиксом nb-.",
  },

  // Текст на кнопке
  buttonText: {
    type: String,
    default: "Скачать neobrutality.css",
  },

  // Ссылка на файл для скачивания
  downloadUrl: {
    type: String,
    default: "assets/css/neobrutality.css",
  },

  // Имя файла при скачивании (атрибут download)
  downloadFilename: {
    type: String,
    default: "neobrutality.css",
  },

  // Показывать ли блок инструкций
  showInstructions: {
    type: Boolean,
    default: true,
  },

  // Заголовок блока инструкций
  instructionsTitle: {
    type: String,
    default: "Как подключить?",
  },

  // Шаги инструкций (массив строк)
  instructions: {
    type: Array,
    default: () => [
      "1. Скачайте файл neobrutality.css.",
      "2. Положите его в папку вашего проекта.",
      "3. Добавьте ссылку в <head>:",
    ],
  },
});

// 2. 🔼 ЭМИТЫ (в App.vue)
const emit = defineEmits(["welcome-click"]);

// 3. 🖱️ ОБРАБОТЧИК КЛИКА
const onButtonClick = (data) => {
  // Локальный лог
  console.log("[NbWelcome] Скачивание:", data.label);

  // Пробрасываем событие в App.vue с контекстом
  emit("welcome-click", {
    ...data,
    section: props.sectionId,
    downloadUrl: props.downloadUrl,
  });

  // 📌 Примечание:
  // NbButton рендерит <a download>, поэтому браузер
  // сам начнёт скачивание. Мы только треким событие.
};
</script>

<style scoped>
/* 
  Дополнительные стили, если нужны.
  Основные стили .download-box — в глобальном CSS.
*/

/* Адаптивность для мобильных */
@media (max-width: 768px) {
  .download-box {
    padding: 20px;
    box-shadow: 5px 5px 0 var(--nb-border);
  }
}
</style>
