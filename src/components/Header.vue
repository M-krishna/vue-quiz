<template>
    <b-nav>
        <b-nav-item disabled>Quiz App</b-nav-item>
        <b-nav-item disabled>Counter: {{ correct }}/{{ total }}</b-nav-item>
    </b-nav>
</template>

<script>
export default {
    props: [
        "correct",
        "total"
    ]
}
</script>