<template>
  <div class="main">
    <div class="head">
        <h2 class="">Remote <span style="color: rgb(247, 240, 120); font-style: italic;">Livestock</span> Monitoring System</h2>
    </div>

    <div class="container">
        <div class="form-div">
            <form @submit="login">
                <label for="username">Email</label><br>
                <input type="email" name="email" v-model="email" id="email"><br>
                <label for="password">Password</label><br>
                <input type="password" name="password" v-model="password" id="password"><br>
                <input type="submit" class="login-btn" value="LOGIN">
            </form>
        </div>
    </div> <div class="container" style="color: white; padding: 15px;">{{ authUser }}</div>
  </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'Login',
        
        data(){
            return {
                email: '',
                password: '',
                authUser: null
            }
        },

        methods: {
            login: function(e){
                e.preventDefault();

                if((this.email.trim().length > 0) && (this.password.trim().length > 0)){

                    let data = {
                        email: this.email,
                        password: this.password
                    };

                    let configHeaders = {
                        headers: {
                            'content-type':'application/json',
                            'Cache-Control' : 'no-cache'
                        }
                    };

                    axios.post('https://remotelivestockmonitor.000webhostapp.com/api/remote_livestock_monitoring/user_login', 
                        data,
                        configHeaders
                        )
                        .then(response => {
                            this.authUser = response.data.data;
                            if(response.data.status == "AUTH_SUCCEED"){
                                window.location.href = '/home';
                            }else{
                                alert('Unauthorized access denied');
                            }
                        })
                        .catch(e => {
                            alert(e);
                        })
                }else{
                    alert('Please, enter the login details...');
                }
            }
        },

        computed: {
            
        }
    }
</script>

<style scoped>
    *{
        box-sizing: border-box;
    }
    

    .main{
        width: 100%;
        height: 100vh;
        background: linear-gradient(180deg, rgba(0, 0, 0, 0.75) 25.62%, rgba(0, 0, 0, 0.75) 100%), url('~@/assets/img/mouau-gate.jpg');
        background-repeat: no-repeat;
        background-position: center;
        background-size: cover;
    }

    .head{
        text-align: center;
        padding: 25px;
        color: #FFFFFF;
    }

    .container{
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .form-div{
        margin-top: 5%;
        background-color: #FFFFFF;
        color: #000000 !important;
        border: 1px solid #CCCCCC;
        padding: 1.5em;
        border-radius: 10px;
    }

    input{
        width: 100%;
        height: 40px;
        padding: 10px;
        margin: 10px 0px;
    }

    .login-btn{
        background-color: #1b4125;
        color: #FFFFFF;
        border: none;
    }

    @media screen and (min-width: 760px){
        .form-div{
            width: 30% !important;
        }
    }

    @media screen and (max-width: 450px){
        .form-div{
            width: 90% !important;
        }
    }
</style>
