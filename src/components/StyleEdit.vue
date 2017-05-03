<template>
    <div class="styleEditor" ref="container">
        <div class="code" v-html="introduceStyle"></div>
        <pre v-html="hlStyle">
        </pre>

    </div>
</template>

<script>
    import  hljs from 'highlight.js'

    export default {
        props: ['code','curCode'],
        data () {
            return {}
        },
        mounted () {
        },
        methods: {
            goBottom () {
                this.$refs.container.scrollTop = 100000
            }
        },
        computed: {
            hlStyle () {
                return hljs.highlightAuto(this.code).value;
            },
            introduceStyle () {
                return `<style>${this.curCode}</style>`
            }
        }
    }
</script>

<style scoped>
</style>