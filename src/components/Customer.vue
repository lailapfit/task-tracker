<template>
    <div class="customers-container">
        <b-button class="float-right" size="lg" variant="success" @click="addNewCustomer">+ NEW CUSTOMER</b-button>
        <div class="shadow table">
            <b-table hover :items="customers" :fields="fields">
                <template #cell(actions)="data">
                    <b-button size="sm" class="mr-2" variant="primary" @click="updateCustomer(data.item.id)" :ref="data.id">EDIT</b-button>
                    <b-button size="sm" class="mr-2" variant="danger" @click="deleteCustomer(data.item)"  :ref="data.id">DELETE</b-button>
                </template>
            </b-table>
        </div>
        <div class="form-container">
            <label for="name">Customer Name</label>
            <br>
            <input id="customer-name" v-model="name">
            <br>
            <b-button class="main-button" type="button" @click="addNewCustomer">CREATE CUSTOMER</b-button>
        </div>
    </div>
</template>

<script>
export default {
    name: "Customer",
    data() {
        return {
            customers: [],
            fields: [
                'id', 
                'name', 
                { key: 'actions', label: 'Actions' }
            ],
            name: ''
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
            console.log('Deleted customer: ' + JSON.stringify(customer));
            console.log('Deleted customer id: ' + customer.id);
            this.$http.delete('http://localhost:8000/customer/id/' + customer.id)
            .then(deletedCustomer => {
                console.log(deletedCustomer);
                let index = this.customers.findIndex(c => { return c.id === customer.id; })
                console.log('index: ' + index);
                this.$delete(this.customers, index);
            })
            .catch(error => console.log(error))
        },
        addNewCustomer: function() {
            this.$http.post('http://localhost:8000/customer/', { name:this.name })
            .then(customer => {
                let newCustomer = customer.data[0];
                newCustomer['actions'] = {};
                console.log('addNewCustomer: ' + JSON.stringify(newCustomer))
                this.customers.push(newCustomer);
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