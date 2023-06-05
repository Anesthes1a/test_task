<template>
    <div class="background-popup" @click="showPopUp" />
    <div class="pop-up">
        <h3>Добавить продукт</h3>
        <div class="choose-product">
            <span>Продукт: </span>
            <select v-model="curr_product">
                <option 
                    v-for="product in products"
                    :value="product"
                >
                    {{ product.title }}
                </option>
            </select>
        </div>
        <div class="input-quantity">
            <span>Кол-во: </span>
            <input 
                v-model="quantity"
                :maxlength="2"
                @input="isNumberString"
            >
        </div>
        <button class="confirm-btn"
            @click="addProduct"
        >Добавить</button>
    </div>
</template>

<script>
    import axios from "axios"

    export default {
            data() {
                return {
                    products: null,
                    curr_product: '',
                    quantity: '',
                }
            },
            methods: {
                addProduct() {
                    if (this.curr_product != '' && this.quantity != '') {
                        this.curr_product.quantity = parseInt(this.quantity)
                        this.$emit('addProduct', this.curr_product)
                    } else {
                        alert("Заполните все поля")
                    }
                    
                },
                isNumberString() {
                    let numberArray = []
                    this.quantity.split("").map((elem) => {numberArray.push(!isNaN(parseInt(elem)) ? parseInt(elem) : '')})
                    numberArray[0] = numberArray[0] == '0' ? '' : numberArray[0]
                    this.quantity = numberArray.join('')
                },
                showPopUp() {
                    this.$emit('showPopUp')
                }
            },
            async mounted() {
                const response = await axios.get('https://dev-su.eda1.ru/test_task/products.php').then(res => res.data)
                if (response.success) {
                    this.products = response.products
                } else {
                    alert('Произошла ошибка. Пожалуйста, повторите запрос.')
                    this.showPopUp()
                }
            }
        }
</script>

<style scoped>
    .background-popup {
        top: 0;
        left: 0;
        position: absolute;
        width: 100%;
        height: 100%;
        background-color: rgba(29, 30, 31, 0.4);;
    }

    .pop-up {
        position: absolute;
        margin-left: auto;
        margin-right: auto;
        margin-top: 200px;
        padding: 20px 40px;
        justify-content: space-between;
        width: 400px;
        height: 300px;
        left: 0;
        right: 0;
        display: flex;
        flex-direction: column;
        gap: 10px;
        border: #228C22 5px solid;
        border-radius: 20px;
        background-color: white;
        
    }

    .pop-up h3 {
        text-align: center;
    }

    .choose-product {
        display: flex;
    }

    .input-quantity {
        display: flex;
    }

    .choose-product select {
        margin-left: auto;
        margin-right: 30px;
        width: 200px;
        vertical-align: middle;
    }

    .choose-product span {
        vertical-align: middle;
        margin-left: 30px;
    }

    .input-quantity input {
        margin-left: auto;
        margin-right: 30px;
        width: 35px;
        vertical-align: middle;
    }

    .input-quantity span {
        vertical-align: middle;
        margin-left: 30px;
    }
</style>