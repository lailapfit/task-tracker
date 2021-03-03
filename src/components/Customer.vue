<template>
    <div class="customers-container">
        <div class="section shadow">
            <ul v-for="customer in customers" :key="customer.id">
                <li>
                    <h3>{{ customer.name }}</h3>
                    <button class="secondary-button">UPDATE</button>
                    <button class="secondary-button">DELETE</button>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: "Customer",
    data() {
        return {
            customers: []
        }
    },
    methods: {
        getAllCustomers: function() {
            this.$http.get('http://localhost:8000/customer/')
            .then(customers => {
                console.log('customers: ' + JSON.stringify(customers))
                this.customers = customers.data;
            })
            .catch(error => console.log(error))
        }
    },
    mounted() {
        this.getAllCustomers();
    }

}
</script>

<style scoped>

</style>