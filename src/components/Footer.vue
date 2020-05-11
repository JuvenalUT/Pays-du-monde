<template>
    <footer>
        <p>Développé par Jordan Lebrethon 👋</p>
    </footer>
</template>

<script>
export default {
    name: 'Footer'
}
</script>

<style lang="scss">
footer {
    max-width: 280px;
    margin: 0 auto;
    padding: 20px 0;
    text-align: center;
    p {
        max-width: 275px;
        border-bottom: dashed 1px rgba(0,0,0,0.4);
        margin: 0 !important;
        &:hover {
            border-bottom: solid 1px rgba(0,0,0,0.4);
        }
    }
}
</style>