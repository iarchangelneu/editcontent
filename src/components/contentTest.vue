<template>
    <p v-for="(item, i) in content" :key="i">{{ item.Text }}</p>
</template>

<script>
export default {
    emits: ["load"],
    props: ["content"],
}
</script>

<style>
</style>