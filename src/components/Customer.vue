<template>
    <div class="customers-container">
        <b-button class="float-right" size="lg" variant="success" @click="openNewForm">+ NEW CUSTOMER</b-button>
        <div class="shadow table">
            <b-table hover :items="customers" :fields="fields">
                <template #cell(actions)="data">
                    <b-button size="sm" class="mr-2" variant="primary" @click="openEditForm(data.item.id)" :ref="data.id">EDIT</b-button>
                    <b-button size="sm" class="mr-2" variant="danger" @click="deleteCustomer(data.item)"  :ref="data.id">DELETE</b-button>
                </template>
            </b-table>
        </div>
        <div class="form-wrapper">
            <div class="form-container shadow" v-if="isHiddenAdd">
                <label for="name">Customer Name</label><br>
                <input id="customer-name" v-model="newCustomerName"><br>
                <b-button class="main-button" type="button" @click="addNewCustomer">CREATE CUSTOMER</b-button>
            </div>
            <div class="form-container shadow" v-if="isHiddenUpdate">
                <label for="name">Customer Id</label><br>
                <input v-model="updatedCustomerId">
                <br>
                <label for="name">Customer Name</label><br>
                <input v-model="updatedCustomerName"><br>
                <b-button class="main-button" type="button" @click="updateCustomer">UPDATE CUSTOMER</b-button>
            </div>
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
            newCustomerName: '',
            updatedCustomerName: '',
            updatedCustomerId: 0,
            isHiddenAdd: false,
            isHiddenUpdate: false
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
        updateCustomer: function() {
            console.log('Updated customer: ' + JSON.stringify(this.updatedCustomerName));
            this.$http.put('http://localhost:8000/customer/id/' + this.updatedCustomerId, { id: this.updatedCustomerId , name: this.updatedCustomerName })
            .then(updatedCustomer => {
                console.log('updatedCustomer: ' + JSON.stringify(updatedCustomer));
                this.customers.forEach(customer => {
                    if (customer.id === this.updatedCustomerId) {
                        customer.name = this.updatedCustomerName;
                    }
                })
            })
            .catch(error => console.log(error))
        },
        deleteCustomer: function(customer) {
            console.log('Deleted customer: ' + JSON.stringify(customer));
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
            this.$http.post('http://localhost:8000/customer/', { name:this.newCustomerName })
            .then(customer => {
                let newCustomer = customer.data[0];
                newCustomer['actions'] = {};

                console.log('addNewCustomer: ' + JSON.stringify(newCustomer))
                this.customers.push(newCustomer);
            })
            .catch(error => console.log(error))
        },
        openEditForm: function(customerId) {
            this.isHiddenUpdate = true;
            this.updatedCustomerId = customerId;
        },
        openNewForm: function() {
            this.isHiddenAdd = true;
        }
    },
    mounted() {
        this.getAllCustomers();
    }

}
</script>

<style scoped>
.form-wrapper {
    display: flex;
    margin-top: 40px;
}
.form-container {
    width: 30%;
    height: 225px;
    background: #7593FF;
    border-radius: 10px;
    border-color: #7593FF;
    color: white;
    margin: auto;
    padding: 15px;
}
</style>