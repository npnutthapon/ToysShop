<style>
body {
    background-image: url("https://cdn.discordapp.com/attachments/841250203985248276/956639141456662528/5128e948e3171cedb328e5f5aa892a8a.jpg");
}
</style>
<template>
    <v-app style="opacity: 0.9;">
        <div class="container">
            <div class="row justify-content-center">
                <div class="col-md-8">
                    <v-container fluid>
                        <v-row dense>
                            <v-col
                                v-for="product in products"
                                :key="product.id"
                                :cols="3"
                            >
                                <v-card>
                                    <v-img
                                        :src= "product.image"
                                        class="white--text align-end justify-center"
                                        gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
                                        height="500px"
                                    >
                                        <v-card-title
                                            v-text="product.name"
                                        ></v-card-title>
                                    </v-img>

                                    <v-card-actions>
                                        <v-card-title
                                            v-text="product.price"
                                        ></v-card-title>
                                        <span class="text-h5">บาท</span>
                                        <v-spacer></v-spacer>

                                        <v-btn icon>
                                            <v-icon>mdi-heart</v-icon>
                                        </v-btn>

                                        <v-btn icon>
                                            <v-icon>mdi-bookmark</v-icon>
                                        </v-btn>

                                        <v-btn icon>
                                            <v-icon>mdi-share-variant</v-icon>
                                        </v-btn>
                                    </v-card-actions>
                                </v-card>
                            </v-col>
                        </v-row>
                    </v-container>
                </div>
            </div>
        </div>
    </v-app>
</template>

<script>
export default {
    data() {
        return {
            products: [],
            product: null,
        };
    },
    mounted() {
        this.initialize();
        console.log("Initialized");
    },
    methods: {
        initialize() {
            axios
                .get("api/product")
                .then((response) => {
                    if (response.data.success == true) {
                        this.products = response.data.products;
                    } else {
                        console.log("error");
                    }
                })
                .catch((error) => {
                    console.log("error");
                });
        },
    },
};
</script>
