<template>
    <v-flex class="pr-3 pb-2" xs12 md6 lg4>
        <v-card class="blue darken-3 white--text">
            <v-card-title class="headline">
                <strong>
                    {{ stock.name }} 
                    <small>
                        (Preço: {{ stock.price | currency }} | Qtde: {{ stock.quantity }})
                    </small>
                </strong>
            </v-card-title>
        </v-card>
        <v-card>
            <v-container fill-height>
                <v-text-field 
                    label="Quantidade" 
                    type="number" 
                    :error="insufficientQuantity || !Number.isInteger(quantity)"
                    v-model.number="quantity" />
                <v-btn class="blue darken-3 white--text ml-4"
                    :disabled="insufficientQuantity || quantity <= 0 || !Number.isInteger(quantity)"
                    @click="sellStock">{{ insufficientQuantity ? 'Insuficiente' : 'Vender' }}</v-btn>
            </v-container>
        </v-card>
    </v-flex>
</template>

<script>
export default {
    props: {
        stock: {
            type: Object,
            required: true
        }
    },
    computed: {
        insufficientQuantity() {
            return this.quantity > this.stock.quantity
        }
    },
    data() {
        return {
            quantity: 0
        }
    },
    methods: {
        sellStock() {
            const order = {
                stockId: this.stock.id,
                stockPrice: this.stock.price,
                quantity: this.quantity
            }
            this.$store.dispatch('sellStock', order)
            this.quantity = 0
        }
    }
}
</script>

<style>

</style>