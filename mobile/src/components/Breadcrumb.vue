<template>
  <div>
    {{ weekDay }}

    <span>
      {{ date }}
    </span>
  </div>
</template>

<script setup>
const props = defineProps({
  weekDay: {
    type: String,
    default: '',
    required: true
  },
  date: {
    type: String,
    default: '',
    required: true
  }
});
</script>

<style scoped>
div {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;

  color: #9ca3af;
  font-weight: 600;
  font-size: 1rem;
  text-transform: lowercase; 
}

div span{
  color: var(--font);
  font-size: 1.875rem;
  line-height: 2.25rem;
  font-weight: 800;
}
</style>