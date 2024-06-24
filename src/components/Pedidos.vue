<template>
    <div>
        <div class="contenido">
            <form @submit.prevent="submitForm">

                <label for="numPedido">Codigo de pedido:</label>
                <br />
                <input type="text" id="numPedido" v-model="formData.numPedido" required />
                <br />
                <label for="nomCliente">Nombre de Cliente:</label>
                <br />
                <input type="text" id="nomCliente" v-model="formData.nomCliente" required />
                <br />
                <label for="descripcion">Descripcion:</label>
                <br />
                <input type="text" id="descripcion" v-model="formData.descripcion" required />
                <br />
                <label for="total">Total:</label>
                <br />
                <input type="text" id="total" v-model="formData.total" required />
                <br />
                <button type="submit">Agregar</button>
            </form>

            <br />
            <br />
            <table>
                <thead>
                    <tr>
                        <th>Pedido</th>
                        <th>Nombre</th>
                        <th>Descripcion</th>
                        <th>Total</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in data" :key="item.numPedido">
                        <td>{{ item.numPedido }}</td>
                        <td>{{ item.nomCliente }}</td>
                        <td>{{ item.descripcion }}</td>
                        <td>{{ item.total }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    data() {
        return {
            formData: {
                numPedido: '',
                nomCliente: '',
                descripcion: '',
                total: '',
            },
            data: [],
        };
    },
    methods: {
        async submitForm() {
            try {
                const response = await axios.post('http://aws-bff2-env.eba-jvmmdhu4.us-east-1.elasticbeanstalk.com/bff/pedidos', this.formData);
                console.log('Order submitted successfully:', response.data);
                alert('Order submitted succesfully')
                this.fetchData(); //Fetch the updated data
            } catch (error) {
                console.log('Error submitting order:', error);
                alert('Error submitting order')
            }
        },
        async fetchData() {
            try {
                const response = await axios.get('http://aws-bff2-env.eba-jvmmdhu4.us-east-1.elasticbeanstalk.com/bff/pedidos');
                this.data = response.data;
            } catch (error) {
                console.error('Error fetching data:', error);
            }
        },
    },
    mounted() {
        this.fetchData(); // Fetch data when the component is mounted
    },
};
</script>

<style scoped>
div {
    margin-bottom: 10px;
}

label {
    display: inline-block;
    width: 70px;
}

input {
    padding: 5px;
    margin-bottom: 10px;
    border-radius: 10px;
}

button {
    padding: 5px 10px;
    border-radius: 10px;
    background-color: green
}

.contenido {
    max-width: fit-content;
    margin-left: auto;
    margin-right: auto;
}
</style>