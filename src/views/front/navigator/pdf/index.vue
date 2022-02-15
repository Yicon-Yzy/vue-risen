<template>
    <div class="pdf_wrap">
        <iframe :src="url" frameborder="0"></iframe>
    </div>
</template>

<script>
    // import { GetQueryString } from '../../../src/public/public'

    export default {
        name: "index",
        data() {
            return {
                url: '',
            }
        },
        watch: {
            '$route'(newValue, oldValue) {
                console.log(this.$route.query.name)
                let name = this.$route.query.name
                //let origin = location.origin
                //this.url = `/pdf/web/viewer.html?file=${origin}/src/${name}.pdf&${Math.random()}`
                // this.url = `/pdf/web/viewer.html?file=./template.pdf`
                let origin = `https://object.risen.com/navigator`
                this.url = `${origin}/pdf/web/viewer.html?file=${origin}/src/${name}.pdf&${Math.random()}`
            },
        },
        mounted() {
          let name = this.$route.query.name
          //let origin = location.origin
          //this.url = `/pdf/web/viewer.html?file=${origin}/src/${name}.pdf&${Math.random()}`
          let origin = `https://object.risen.com/navigator`
          this.url = `${origin}/pdf/web/viewer.html?file=${origin}/src/${name}.pdf&${Math.random()}`
        },
    }
</script>

<style scoped lang="scss">
    .pdf_wrap{
        width: 100%;
        height: 100%;
        background-image: url("../../../../assets/img/bg@2x.jpg");
        background-repeat: no-repeat;
        background-size: cover;
    }
    iframe{
        width: 50%;
        height: 100%;
        padding: 0;
        margin: 0 25%;
    }
</style>
