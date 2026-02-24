<script setup>
import { ref } from "vue";
import HeaderComponent from "./components/header/HeaderComponent.vue";
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

const navLinks = ref([
  { text: "Установка", href: "#install" },
  { text: "Кнопки", href: "#buttons" },
  { text: "Карточки", href: "#cards" },
  { text: "Формы", href: "#forms" },
  { text: "Контакты", href: "#contacts" },
  { text: "О нас", href: "#about" },
]);
</script>
<template>
  <HeaderComponent
    :links="navLinks"
    brand="NEOBRUTALITY.ru ;)"
    @link-click="onLinkClick"
  />
</template>

<style scoped></style>
