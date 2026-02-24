<script setup>
const props = defineProps({
  brand: { type: String, default: "NeoBrutality" },
  brandLink: { type: String, default: "/" },
  links: {
    type: Array,
    default: () => [],
  },
});
//Выучи и напиши что это
const emit = defineEmits(["link-click"]);
//Выучи и напиши что это
const handleLinkClick = (link) => {
  if (link.href.startsWith("#")) {
    const element = document.querySelector(link.href);
    if (element) {
      element.scrollIntoView({ behavior: "smooth" });
    }
  }
  //Выучи и напиши что это
  emit("link-click", {
    text: link.text,
    href: link.href,
    timestamp: Date.now(),
  });
};
</script>

<template>
  <header class="nb-header nb-header-fixed">
    <div class="nb-container nb-header-container">
      <!-- Логотип -->
      <a :href="brandLink" class="nb-navbar-brand">
        {{ brand }}
      </a>

      <!-- Навигация (ТОЛЬКО динамическая) -->
      <nav class="nb-nav-links">
        <a
          v-for="(link, index) in links"
          :key="index"
          :href="link.href"
          class="nb-nav-link"
          @click.prevent="handleLinkClick(link)"
        >
          {{ link.text }}
        </a>
      </nav>
    </div>
  </header>
</template>

<style scoped>
/*--Хедер--*/
.nb-header {
  margin: 0;
  padding: 15px 0;
  border-bottom: var(--nb-border-width) solid var(--nb-border);
  background: var(--nb-surface);

  margin-bottom: 30px;
}
/* Фиксированная позиция хедера */
.nb-header-fixed {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background: var(--nb-surface);
  border-bottom: var(--nb-border-width) solid var(--nb-border);
}
/*---Контейнер---*/
.nb-container {
  width: 100%;
  max-width: 1140px;
  margin: 0 auto;
  padding: 0 15px;
}

/*---Контейнер внутри хедера---*/
.nb-header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 0;
}
.nb-navbar-brand {
  font-family: var(--nb-font-head);
  font-size: 1.5rem;
  text-transform: uppercase;
  border: var(--nb-border-width) solid var(--nb-border);
  padding: 5px 10px;
  background: var(--nb-warning);
  box-shadow: 3px 3px 0 var(--nb-border);
  display: inline-block;
}

/* Навигация - сбрасываем float из глобальных стилей */
.nb-nav-links {
  display: flex;
  gap: 20px;
  float: none;
}

/* Ссылки навигации */
.nb-nav-link {
  margin-left: 0;
  font-weight: bold;
  text-transform: uppercase;
  cursor: pointer;
  position: relative;
}

.nb-nav-link:hover {
  text-decoration: underline;
  text-decoration-thickness: 2px;
}
</style>
