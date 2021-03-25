<template>
    <select class="content-center text-white form-select mt-1 bg-gray-700 border-b border-gray-600">
        <option class="selector" v-for="list in lists" :value="list.id" v-bind:key="list.id" selected="selected"> {{list.name}} </option>
        <option value="" disabled selected hidden>Choisissez une liste</option>
    </select>
</template>

<script>
export default {
  name: 'sidebar',
  props : ['lists'],
}
</script>