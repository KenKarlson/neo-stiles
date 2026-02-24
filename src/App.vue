//App
<script setup>
import { ref } from "vue";
import HeaderComponent from "./components/header/HeaderComponent.vue";
import NbWelcome from "./components/welcome/NbWelcome.vue";

const trackClickToBackend = async (link) => {
  try {
    await fetch();
  } catch (error) {
    console.error(error, "Error");
  }
};

const onLinkClick = (link) => {
  console.log("Клик по меню:", {
    текст: link.text,
    ссылка: link.href,
    время: new Date(link.timestamp).toLocaleTimeString(),
  });
  if (typeof ym === "function") {
    ym(12345678, "reachGoal", "menu_click", {
      menu_item: link.text,
      menu_href: link.href,
    });
  }

  if (typeof gtag === "function") {
    gtag("event", "menu_click", {
      event_category: "navigation",
      event_label: link.text,
      value: link.href,
    });
  }
  trackClickToBackend(link);
};
// Обработчик клика по кнопке в секции Welcome (от NbWelcome)
const onWelcomeClick = (data) => {
  console.log("Welcome click:", {
    кнопка: data.label,
    секция: data.section,
    ссылка: data.downloadUrl,
    время: new Date(data.timestamp).toLocaleTimeString(),
  });

  // Аналитика (если есть)
  if (typeof ym === "function") {
    ym(12345678, "reachGoal", "welcome_download", {
      button: data.label,
      url: data.downloadUrl,
    });
  }

  // Отправка на бэкенд (функция уже объявлена выше)
  trackClickToBackend(data);
};

const navLinks = ref([
  { text: "Установка", href: "#install" },
  { text: "Кнопки", href: "#buttons" },
  { text: "Карточки", href: "#cards" },
  { text: "Формы", href: "#forms" },
  { text: "Контакты", href: "#contacts" },
  { text: "О нас", href: "#about" },
]);
const welcomeData = ref({
  title: "NeoBrutality UI",
  description:
    "Ретро-фреймворк в стиле комиксов. Минимализм, чёткие формы, ретро-эстетика.",
  buttonText: "Скачать neobrutality.css",
  downloadUrl: "/assets/neobrutality.css",
  bgColor: "warning",
});
</script>
<template>
  <HeaderComponent
    :links="navLinks"
    brand="NEOBRUTALITY.ru ;)"
    @link-click="onLinkClick"
  />
  <div class="nb-header-spacer"></div>
  <div class="nb-header-spacer"></div>
  <div class="nb-header-spacer"></div>
  <NbWelcome
    :title="welcomeData.title"
    :description="welcomeData.description"
    :button-text="welcomeData.buttonText"
    :download-url="welcomeData.downloadUrl"
    :bg-color="welcomeData.bgColor"
    @welcome-click="onWelcomeClick"
  />
</template>

<style scoped></style>
