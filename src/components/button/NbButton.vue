<template>
  <!-- 
    Кнопка может быть ссылкой (<a>) или кнопкой (<button>)
    Если есть href — рендерим <a>, иначе <button>
  -->
  <component 
    :is="href ? 'a' : 'button'"
    :href="href"
    :type="href ? undefined : 'button'"
    :class="[
      'nb-btn',
      `nb-btn-${variant}`,
      { 'nb-btn-block': block, 'nb-btn-loading': loading }
    ]"
    @click="handleClick"
  >
    <!-- Слот для иконки (опционально) -->
    <span v-if="$slots.icon" class="nb-btn-icon">
      <slot name="icon"></slot>
    </span>
    
    <!-- Текст кнопки -->
    <span class="nb-btn-label">
      <slot>{{ label }}</slot>
    </span>
    
    <!-- Индикатор загрузки (опционально) -->
    <span v-if="loading" class="nb-btn-loader">⏳</span>
  </component>
</template>

<script setup>
// ==========================================
// NbButton.vue — Универсальная ретро-кнопка
// ==========================================
// Отвечает за: отображение, стили, базовую логику клика
// НЕ знает: где используется, что делать после клика

// 1. 🔽 ПРОПСЫ (данные СВЕРХУ)
const props = defineProps({
  // Текст кнопки (если не передан слот)
  label: {
    type: String,
    default: 'Кнопка'
  },
  
  // Вариант стиля: primary | success | danger | warning | outline
  variant: {
    type: String,
    default: 'default',
    validator: (value) => ['default', 'primary', 'success', 'danger', 'warning', 'outline'].includes(value)
  },
  
  // Кнопка на всю ширину
  block: {
    type: Boolean,
    default: false
  },
  
  // Если есть href — кнопка становится ссылкой
  href: {
    type: String,
    default: null
  },
  
  // Состояние загрузки
  loading: {
    type: Boolean,
    default: false
  },
  
  // Отключить кнопку
  disabled: {
    type: Boolean,
    default: false
  }
})

// 2. 🔼 ЭМИТЫ (события ВВЕРХ)
const emit = defineEmits([
  'click'      // Пользователь кликнул
  // Можно добавить: 'hover', 'focus', 'blur' при необходимости
])

// 3. 🖱️ ОБРАБОТЧИК КЛИКА
const handleClick = (event) => {
  // Если кнопка отключена или загружается — ничего не делаем
  if (props.disabled || props.loading) {
    event.preventDefault()
    return
  }
  
  // ✅ ОТПРАВЛЯЕМ событие родителю (NbWelcome.vue)
  // Передаём полезные данные + оригинальное событие
  emit('click', {
    originalEvent: event,
    label: props.label,
    variant: props.variant,
    href: props.href,
    timestamp: Date.now()
  })
  
  // 📌 Примечание:
  // Мы НЕ делаем preventDefault() — если это ссылка (<a>), 
  // браузер перейдёт по href. Если нужна кастомная логика —
  // родитель перехватит событие и вызовет event.preventDefault()
}
</script>

<style scoped>
/* 
  Стили кнопки — только визуал.
  Логика (что делать при клике) — в родителе.
*/

.nb-btn {
  /* Базовые стили из neobrutality.css */
  font-family: var(--nb-font-main);
  font-weight: bold;
  text-transform: uppercase;
  padding: 10px 20px;
  font-size: 1rem;
  border: var(--nb-border-width) solid var(--nb-border);
  border-radius: var(--nb-radius);
  background-color: var(--nb-surface);
  color: var(--nb-text);
  box-shadow: var(--nb-shadow-offset) var(--nb-shadow-offset) 0 var(--nb-border);
  cursor: pointer;
  transition: all 0.1s;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  text-decoration: none;
}

/* Hover-эффект */
.nb-btn:hover:not(.nb-btn-loading):not([disabled]) {
  transform: translate(-2px, -2px);
  box-shadow: calc(var(--nb-shadow-offset) + 2px) calc(var(--nb-shadow-offset) + 2px) 0 var(--nb-border);
}

/* Active-эффект (нажатие) */
.nb-btn:active:not(.nb-btn-loading):not([disabled]) {
  transform: translate(var(--nb-shadow-offset), var(--nb-shadow-offset));
  box-shadow: 0 0 0 var(--nb-border);
}

/* Варианты цветов */
.nb-btn-primary { background-color: var(--nb-primary); color: var(--nb-primary-text); }
.nb-btn-success { background-color: var(--nb-success); color: var(--nb-text); }
.nb-btn-danger  { background-color: var(--nb-danger);  color: var(--nb-text); }
.nb-btn-warning { background-color: var(--nb-warning); color: var(--nb-text); }
.nb-btn-outline { background-color: transparent; }

/* Кнопка на всю ширину */
.nb-btn-block {
  width: 100%;
  justify-content: center;
}

/* Состояние загрузки */
.nb-btn-loading {
  opacity: 0.7;
  cursor: wait;
}

/* Отключённая кнопка */
.nb-btn[disabled] {
  opacity: 0.5;
  cursor: not-allowed;
}

/* Иконка внутри кнопки */
.nb-btn-icon {
  display: flex;
  align-items: center;
}
</style>
