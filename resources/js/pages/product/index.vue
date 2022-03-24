<style>
body {
    background-image: url("https://cdn.discordapp.com/attachments/841250203985248276/956639141456662528/5128e948e3171cedb328e5f5aa892a8a.jpg");
}
</style>
<template>
    <v-app style="opacity: 0.9;">
        <v-data-table
            :headers="headers"
            :items="products"
            sort-by="calories"
            class="elevation-1"
        >
            <template v-slot:top>
                <v-toolbar flat>
                    <v-toolbar-title>รายการสินค้า</v-toolbar-title>
                    <v-divider class="mx-4" inset vertical></v-divider>
                    <v-spacer></v-spacer>
                    <v-dialog v-model="dialog" max-width="500px">
                        <template v-slot:activator="{ on, attrs }">
                            <v-btn
                                color="primary"
                                dark
                                class="mb-2"
                                v-bind="attrs"
                                v-on="on"
                            >
                                เพิ่มสินค้า
                            </v-btn>
                        </template>
                        <v-card>
                            <v-card-title>
                                <span class="text-h5">เพิ่มสินค้า</span>
                            </v-card-title>

                            <v-card-text>
                                <v-container>
                                    <v-row>
                                        <v-col cols="12" sm="6" md="6">
                                            <v-text-field
                                                v-model="editedItem.name"
                                                label="ชื่อสินค้า"
                                            ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" sm="6" md="6">
                                            <v-text-field
                                                v-model="editedItem.price"
                                                label="ราคาสินค้า"
                                            ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" sm="6" md="6">
                                            <v-text-field
                                                v-model="editedItem.description"
                                                label="รายละเอียด"
                                            ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" sm="6" md="6">
                                            <v-text-field
                                                v-model="editedItem.image"
                                                label="รูปภาพสินค้า"
                                            ></v-text-field>
                                        </v-col>
                                    </v-row>
                                </v-container>
                            </v-card-text>

                            <v-card-actions>
                                <v-spacer></v-spacer>
                                <v-btn
                                    color="blue darken-1"
                                    text
                                    @click="close"
                                >
                                    Cancel
                                </v-btn>
                                <v-btn color="blue darken-1" text @click="save">
                                    Save
                                </v-btn>
                            </v-card-actions>
                        </v-card>
                    </v-dialog>
                    <v-dialog v-model="dialogDelete" max-width="500px">
                        <v-card>
                            <v-card-title class="text-h5"
                                >Are you sure you want to delete this
                                item?</v-card-title
                            >
                            <v-card-actions>
                                <v-spacer></v-spacer>
                                <v-btn
                                    color="blue darken-1"
                                    text
                                    @click="closeDelete"
                                    >Cancel</v-btn
                                >
                                <v-btn
                                    color="blue darken-1"
                                    text
                                    @click="deleteItemConfirm"
                                    >OK</v-btn
                                >
                                <v-spacer></v-spacer>
                            </v-card-actions>
                        </v-card>
                    </v-dialog>
                </v-toolbar>
            </template>
            <template v-slot:item.actions="{ item }">
                <v-icon small class="mr-2" @click="editItem(item)">
                    mdi-pencil
                </v-icon>
                <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
            </template>
            <template v-slot:no-data>
                <v-btn color="primary" @click="initialize"> Reset </v-btn>
            </template>
        </v-data-table>
    </v-app>
</template>

<script>
export default {
    data() {
        return {
            dialog: false,
      dialogDelete: false,
      headers: [
        {
            text:'ID',
            value:'id'
        },
        {
          text: 'ชื่อสินค้า (100g serving)',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'ราคา', value: 'price' },
        { text: 'จัดการ', value: 'actions', sortable: false },
      ],
      products: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        price: 0,
        description: '',
        image: '',
      },
      defaultItem: {
        name: '',
        price: 0,
        description: '',
        image: '',
      },
        };
    },
    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },
    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },
    mounted() {
        this.initialize();
        console.log("Initialized");
    },
    methods: {
        redirect(url) {
            window.location.href = url;
        },
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
        editItem (item) {
        this.editedIndex = this.products.indexOf(item)
        this.editedItem = Object.assign({}, item)
        //API UPDATE
        this.dialog = true
      },

      deleteItem (item) {
        this.editedIndex = this.products.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },

      deleteItemConfirm () {
        //API DELETE
        axios
                .delete("api/product/destroy/"+this.editedItem.id)
                .then((response) => {
                    this.products.splice(this.editedIndex, 1)
                })
                .catch((error) => {
                    console.log("error");
                });
        this.closeDelete()
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {
          
        if (this.editedIndex > -1) {
            //Update
          Object.assign(this.products[this.editedIndex], this.editedItem)
        } else {
            //Add new
            console.log("add new")
            axios
                .post("api/product/store",{
                    name : this.editedItem.name,
                    description : this.editedItem.description,
                    price : this.editedItem.price,
                    image : this.editedItem.image,
                })
                .then((response) => {
                    if (response.data.success == true) {
                        this.products.push(this.editedItem)
                    } else {
                        console.log("error");
                    }
                })
                .catch((error) => {
                    console.log("error");
                });
        }
        this.close()
        
      },
    },
};
</script>
