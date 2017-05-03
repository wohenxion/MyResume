<template>
    <div class="resumeEditor" ref="container" :class="{htmlMode:enableHtml}">
        <div v-if="enableHtml" v-html="markdowns"></div>
        <pre v-else>{{markdowns}}</pre>
    </div>
</template>

<script>
    import marked from 'marked'
    export default {
        props: ['markdown', 'enableHtml'],
        data () {
            return {}
        },
        computed: {
            markdowns () {
                return this.enableHtml ? marked(this.markdown) : this.markdown
            }
        }
    }
</script>

<style scoped>
    .htmlMode {
        animation: flip 2s;
    }

    @keyframes flip {
        from {
            opacity: 0;
        }
        to {
            opacity: 1;
        }
    }
</style>