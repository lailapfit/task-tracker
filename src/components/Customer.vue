<template>
    <div class="customers-container">
        <b-button class="float-right" size="lg" variant="success">+ NEW CUSTOMER</b-button>
        <div class="shadow table">
            <b-table hover :items="customers" :fields="fields">
                <template #cell(actions)="data">
                    <b-button size="sm" class="mr-2" variant="primary" @click="updateCustomer(data.item.id)" :ref="data.id">EDIT</b-button>
                    <b-button size="sm" class="mr-2" variant="danger" @click="deleteCustomer(data.item.id)" :ref="data.id">DELETE</b-button>
                </template>
            </b-table>
        </div>
    </div>
</template>

<script>
export default {
    name: "Customer",
    data() {
        return {
            customers: [],
            fields: ['id', 'name', 
            { key: 'actions', label: 'Actions' }]
        }
    },
    methods: {
        getAllCustomers: function() {
            this.$http.get('http://localhost:8000/customer/')
            .then(customers => {
                this.customers = customers.data;
                console.log('customers: ' + JSON.stringify(this.customers))
            })
            .catch(error => console.log(error))
        },
        updateCustomer: function(customer) {
            console.log('Updated customer: ' + customer);
        },
        deleteCustomer: function(customer) {
            console.log('Deleted customer: ' + customer.name);
        }
    },
    mounted() {
        this.getAllCustomers();
    }

}
</script>

<style scoped>

</style>