<template>
  <span>
    <span
      v-for="(item, i) in p"
      :key="i"
      :class="{
        'font-bold': item.annotations.bold && !item.annotations.code,
        italic: item.annotations.italic,
        'line-through': item.annotations.strikethrough,
        underline: item.annotations.underline,
        'text-red-500': item.annotations.code,
        'dark:text-red-300': item.annotations.code,
        'p-1.5': item.annotations.code,
        'bg-gray-200': item.annotations.code,
        'dark:bg-gray-900': item.annotations.code,
        'font-light': item.annotations.code,
        'text-base': item.annotations.code,
        'font-serif': item.type == 'equation',
        'text-gray-100':
          item.annotations.color != 'default' &&
          item.annotations.color.split('_')[1],
        'p-1':
          item.annotations.color != 'default' &&
          item.annotations.color.split('_')[1],
      }"
      :style="{
        color:
          item.annotations.color != 'default' &&
          !item.annotations.color.split('_')[1]
            ? item.annotations.color
            : null,
        'background-color':
          item.annotations.color != 'default' &&
          item.annotations.color.split('_')[1]
            ? item.annotations.color.split('_')[0]
            : 'none',
      }"
    >
      <a
        v-if="item.href && i > 0 && p[--i].plain_text !== '#img#'"
        :href="item.href"
        target="_blank"
        class="text-blue-600 underline"
        >{{ item.plain_text }}</a
      >
      <span v-else-if="!item.href && item.plain_text != '#img#'">{{
        item.plain_text
      }}</span></span
    >
  </span>
</template>
<script>
export default {
  props: {
    p: Array,
  },
  mounted() {
    this.p.forEach((item, i) => {
      if (i > 0 && i <= this.p.length && this.p[--i].plain_text === '#img#') {
        this.$emit('imgLink', item.plain_text)
      }
    })
  },
  data() {
    return {
      regex: /^\[([\w\s\d]+)\]\(((?:\/|https?:\/\/)[\w\d./?=#]+)\)$/,
    }
  },
}
</script>
