<style>
body {
    background-image: url("https://cdn.discordapp.com/attachments/841250203985248276/956639141456662528/5128e948e3171cedb328e5f5aa892a8a.jpg");
}
</style>
<template>
    <v-app style="opacity: 0.9;">
        <div class="container">
            <v-row style="margin:5px" align="center" justify="space-around">
                <v-btn depressed @click="redirect('/product')">
                    จัดการสินค้า
                </v-btn>
            </v-row>
        </div>
    </v-app>
</template>

<script>
export default {
    data() {
        return {};
    },
    mounted() {
        this.initialize();
        console.log("Initialized");
    },
    methods: {
        redirect(url) {
            window.location.href = url;
        },
        initialize() {},
    },
};
</script>
