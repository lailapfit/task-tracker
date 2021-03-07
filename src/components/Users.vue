<template>
    <div class="user">
        <div class="login-wrapper shadow">
            <div class="banner">
                <h1 id="application-title">TASK TRACKER</h1>
                <img class="shadow" src="../assets/clipboard.png"/>
            </div>
            <h2 id="login-title">User Login</h2>
            <div class="form-container">
                <div class="email-container">
                    <label for="email">Email Address</label>
                    <br>
                    <input id="email" v-model="email">
                </div>
                <div class="password-container">
                    <label for="password">Password</label>
                    <br>
                    <input id="password" v-model="password">
                </div>
            </div>
            <button class="main-button" type="button" @click="login">LOGIN</button>
        </div>
    </div>
</template>

<script>
export default {
    name: "Users",
    data() {
        return {
            email: '',
            password: '',
            userId: ''
        }
    },
    methods: {
        login: function() {
            this.$http.post('http://localhost:8000/user/login', { "email": this.email, "password": this.password})
            .then(user => {
                console.log('user login: ' + JSON.stringify(user));
                user.data.updatedUser.forEach(u => {
                    this.userId = u.id;
                    console.log('userid: ' + u.id);
                    this.email = u.email;
                })
            })
        }
    },
    watch: {
        userId: function(val) {
            console.log('watch userid: ' + val)
            this.$userId = val;
        },
        email: function(val) {
            this.$email = val;
        },
    }
}
</script>

<style scoped>
#application-title {
    font-size: 2.5em;
    margin-top: 0;
    padding-top: 15%;
    color: white;
    font-weight: 900;
    opacity: 0.8;
}
.user {
    margin-bottom: 5%;
}
.banner {
    height: 300px;
    background-image: url('~@/assets/mask-group.png');
    background-repeat: no-repeat;
}
#login-title {
    color: #7593FF;
    font-weight: 900;
}
.login-wrapper {
    width: 365px;
    margin: auto;
    border: 1px solid #E9F2FF;
    border-radius: 17px;
    height: 725px;
    background-color: #E9F2FF;
}
.form-container {
    display: block;
    text-align: center;
    margin: 15% 0 10% 0;
}
.form-container input {
    border-top: none;
    border-right: none;
    border-left: none;
    border-bottom: 1px solid #7593FF;
    margin-top: 2%;
    background-color: #E9F2FF;
    width: 50%;
}
.form-container input::placeholder {
    text-align: center;
    opacity: 0.5; 
}
.form-container label {
    opacity: 0.5;
}
.email-container {
    margin: 2% 0 10% 0;
}
.main-button {
    width: 137px;
    height: 36px;
    background: #7593FF;
    border-radius: 31px;
    border-color: #7593FF;
    box-shadow: none;
    color: white;
    font-weight: 600;
}
</style>