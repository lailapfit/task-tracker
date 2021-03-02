<template>
    <div class="customers">
        <Customer />
    </div>
</template>

<script>
import Customer from "@/components/Customer.vue"

export default {
    name: "Customers",
    components: {
        Customer
    }
}
</script>