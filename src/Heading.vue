<template>
    <component
        v-bind:is="`h${level}`">
        {{ text }}</component>
</template>

<script>
export default {
    props: {
        level: {
            type: Number,
            required: true
        },
        text: String
    }
}
</script>

<style>

</style>