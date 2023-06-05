<template>

    <AddItemPopUp 
        v-if="isPopUpVisible"
        @addProduct="addProduct"
        @showPopUp="showPopUp"
    />
    
    
    <table class="order-table" cellpadding="20px">
        <thead>
            <tr>
                <th class="product-name">Название товара</th>
                <th class="product-quantity">Кол-во</th>
                <th class="product-cost">Стоимость</th>
            </tr>
        </thead>
        <tbody>
            <CartItem 
                v-for="product in addedProducts"
                :key="product.id"
                :product_data="product"
             />
        </tbody>
    </table>
    <button 
        class="add-product-btn"
        @click="showPopUp"
    ></button>
    <h3 class="total-cost"
        v-if="addedProducts.length"
    >Итого: {{ TotalCost }} руб.</h3>
    <button
        v-if="addedProducts.length"
        class="save-btn"
        @click="saveOrder"
    >Сохранить</button>
</template>

<script>
    import CartItem from "@/components/CartItem.vue";
    import AddItemPopUp from '@/components/AddItemPopUp.vue'
    import axios from "axios"

    export default {
        data() {
            return {
                isPopUpVisible: false,
                addedProducts: []
            }
        },
        components: {
            CartItem,
            AddItemPopUp,
        },
        computed: {
            TotalCost() {
                let result = []
                this.addedProducts.map((elem) => {result.push(elem.price * elem.quantity)})
                if (result.length) {
                    result = result.reduce((sum, elem) => { return sum + elem})
                } else {
                    result = 0
                }
                return result.toFixed(2)
            }
        },
        methods: {
            showPopUp() {
                this.isPopUpVisible = this.isPopUpVisible ? false : true
            },
            addProduct(product) {
                this.isPopUpVisible = false
                let isFoundElem = false
                this.addedProducts.find((elem) => {
                    if (elem.id === product.id) {
                        elem.quantity += product.quantity
                        isFoundElem = true
                    }
                })
                if (!isFoundElem) {
                    this.addedProducts.push(product)
                }
            },
            async saveOrder() {
                let products = this.addedProducts.map((elem) => { 
                    return { 
                        product_id: elem.id,
                        quantity: elem.quantity
                     }
                })
                const response = await axios.post("https://dev-su.eda1.ru/test_task/save.php", products).then(res => res.data)
                if (response.success) {
                    alert(`Номер вашего заказа ${parseInt(response.code)}.`)
                    this.addedProducts = []
                } else {
                    alert('Произошла ошибка. Пожалуйста, повторите запрос.')
                }
            }
        }
    }
</script>

<style scoped>
.order-table {
    width: 50%;
}

.order-table th {
    font-size: 28px;
    text-align: center;
}

.order-table th:first-child {
    text-align: left;
}

.add-product-btn {
    width: 50%;
    height: 48px;
    background-image: url('@/assets/add-plus-icon.svg');
    background-repeat: no-repeat;
    background-position: center;
}

.total-cost {
    margin-top: 30px;
    margin-bottom: 0;
}

.save-btn {
    font-size: 26px;
}

</style>