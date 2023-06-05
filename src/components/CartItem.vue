<template>
    <tr>
        <td>{{product_data.title}}</td>
        <td>{{product_data.quantity}}</td>
        <td>{{ (product_data.price * product_data.quantity).toFixed(2) }} руб.</td>
    </tr>
</template>

<script>

    export default {
        props: {
            product_data: {
                type: Object,
                default: {}
            }
        }
    }
</script>

<style scoped>
    td {
        text-align: center; 
        vertical-align: middle;
        font-weight: 400;
    }

    td:first-child {
        padding-left: 30px;
        text-align: left;
        vertical-align: middle;
        font-weight: 400;
    }

    tr:nth-child(2n - 1){
        background-color: #ebf8eb;
    }
</style>