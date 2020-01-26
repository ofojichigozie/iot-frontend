<template>
    <div class="main">
        <div class="top">
            <div class="container">
                <div class="top-inner">
                    <span style="color: #000000; font-style: italic;">{{ username }}</span>
                    <router-link to="/help"><button class="help-btn">Help</button></router-link>
                    <button class="logout-btn" v-on:click="logout()">Logout</button>
                </div>
            </div>
        </div>

        <div class="landing">
            <h1>REMOTE LIVESTOCK MONITORING SYSTEM</h1>
            <p>Using NFC cum internet-of-things (IoT)</p>
            <button>GET STARTED</button>
        </div>

        <div class="main">
            <h4 class="">This web application enables you to monitor your livestocks which are located at a remote environment. Enter the ID of a livestock to view the recently captured information. Remote data are captured every 12 hours...</h4>

            <div class="main-inner">
                <div class="form-div">
                    <input class="id-field" type="number" v-model="nfc_uuid" placeholder="Enter livestock ID">
                    <button class="get-data-btn" v-on:click="getOneLivestockData()">Get Data</button>
                    <div>
                        <button class="get-all-data-btn" v-on:click="getAllLivestockData()">Get all livestock data</button>
                    </div>
                </div>
            </div>

            <div class="container">
                <div v-if="retrievedOneData == true">
                    <div>
                        <h3 style="text-align: center; margin-top: 10px;">Below are the data of the livestock with the specified UUID</h3>
                    </div>

                    <div style="overflow-x: auto;">
                        <table>
                            <thead>
                                <th>S/N</th>
                                <th>UUID</th>
                                <th>Temperature</th>
                                <th>Humidity</th>
                                <th>Pulse Rate</th>
                                <th>Location</th>
                                <th>Capture Time</th>
                            </thead>
                            <tbody>
                                <tr v-bind:key="oneLivestockDatum.key" v-for="oneLivestockDatum in oneLivestockData">
                                    <td> {{ oneLivestockDatum.id }} </td>
                                    <td> {{ oneLivestockDatum.nfc_uuid }} </td>
                                    <td> {{ oneLivestockDatum.temperature }} </td>
                                    <td> {{ oneLivestockDatum.humidity }} </td>
                                    <td> {{ oneLivestockDatum.pulse_rate }} </td>
                                    <td> Lat.({{ oneLivestockDatum.loc_latitude }}), Long.({{ oneLivestockDatum.loc_longitude }})</td>
                                    <td> {{ oneLivestockDatum.created_at }} </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>

                <div v-else-if="retrievedOneData == false">
                    <div>
                        <h3 style="text-align: center; margin-top: 10px;">Below are the data of all available livestock</h3>
                    </div>

                    <div style="overflow-x: auto;">
                        <table>
                            <thead>
                                <th>S/N</th>
                                <th>UUID</th>
                                <th>Temperature</th>
                                <th>Humidity</th>
                                <th>Pulse Rate</th>
                                <th>Location</th>
                                <th>Capture Time</th>
                            </thead>
                            <tbody>
                                <tr v-bind:key="allLivestockDatum.key" v-for="allLivestockDatum in allLivestockData">
                                    <td> {{ allLivestockDatum.id }} </td>
                                    <td> {{ allLivestockDatum.nfc_uuid }} </td>
                                    <td> {{ allLivestockDatum.temperature }} </td>
                                    <td> {{ allLivestockDatum.humidity }} </td>
                                    <td> {{ allLivestockDatum.pulse_rate }} </td>
                                    <td> Lat.({{ allLivestockDatum.loc_latitude }}), Long.({{ allLivestockDatum.loc_longitude }})</td>
                                    <td> {{ allLivestockDatum.created_at }} </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'Home',

        data(){
            return {
                username: 'user@gmail.com',
                nfc_uuid: '',
                oneLivestockData: [],
                allLivestockData: [],
                counter: 1,
                retrievedOneData: null
            }
        },

        methods: {
            getOneLivestockData: function(){
                //Reset this variable to null
                this.retrievedOneData = null;

                if(this.nfc_uuid.trim().length > 0){
                    axios.get('https://remotelivestockmonitor.000webhostapp.com/api/livestock_data/' + this.nfc_uuid, {
                            headers: {
                                //No headers
                            }
                        })
                        .then(response => {
                            if(response.data.status == 'LIVESTOCK_FOUND'){
                                this.retrievedOneData = true;
                                this.oneLivestockData = response.data.data;
                            }else{
                                alert(response.data.message);
                            }
                        })
                        .catch(e => {
                            alert(e);
                        })
                }else{
                    alert('Please, enter a livestock UUID to get its data');
                }
            },

            getAllLivestockData: function(){
                //Reset this variable to null
                this.retrievedOneData = null;

                axios.get('https://remotelivestockmonitor.000webhostapp.com/api/livestock_data', {
                        headers: {
                            //No headers
                        }
                    })
                    .then(response => {
                        if(response.data.status == 'LIVESTOCKS_FOUND'){
                            this.retrievedOneData = false;
                            this.allLivestockData = response.data.data;
                        }else{
                            alert(response.data.message);
                        }
                    })
                    .catch(e => {
                        alert(e)
                    })
            },

            logout: function(){
                window.location.href = '/';
            }
        }
    }
</script>

<style scoped>
    *{
        box-sizing: border-box;
    }

    .main{
        /* background: #bfcfc3; */
    }

    .container{
        width: 90%;
        height: 100%;
        margin: auto;
    }

    .top{
        width: 100%;
        height: 80px;
        background-color: #EEEEEE;
    }

    .top-inner{
        height: 100%;
        display: flex;
        justify-content: flex-end;
        align-items: center;
    }

    .help-btn{
        background-color: rgb(35, 158, 97);
        color: #FFFFFF;
        padding: 8px 23px;
        margin-left:10px;
        border-radius: 10px;
        border: 1px solid #FFFFFF;
    }

    .logout-btn{
        background-color: rgb(158, 35, 35);
        color: #FFFFFF;
        padding: 8px 18px;
        margin-left:10px;
        border-radius: 10px;
        border: 1px solid #FFFFFF;
    }

    .landing{
        display: flex;
        justify-content: center;
        align-items: center;
        color: #FFFFFF;
        flex-flow: column;
        line-height: 0.2em;

        width: 100%;
        height: 350px;
        background: linear-gradient(180deg, rgba(0, 0, 0, 0.75) 25.62%, rgba(0, 0, 0, 0.75) 100%), url('~@/assets/img/back01.jpg');
        background-repeat: no-repeat;
        background-position: center;
        background-size: cover
    }

    .landing p{
        letter-spacing: 0.2em;
    }

    .landing button{
        margin-top: 25px;
        border-radius: 10px;
        padding: 15px 35px;
        background-color: #1b4125;
        color: #FFFFFF;
        border: 0.5px solid #FFFFFF;
    }

    .main h4{
        padding: 15px;
        color: #555555;
        width: 70%;
        margin: auto;
        text-align: center;
    }

    .form-div{
        width: 50%;
        margin: 20px auto;
        padding: 20px;
        border: 1px solid #CCCCCC;
        border-radius: 10px;
    }

    .form-div input,  .form-div button{
        height: 40px;
        margin-top: 8px;
    }

    .form-div button{
        background-color: #1b4125;
        color: #FFFFFF;
        border: none;
    }

    .form-div .id-field{
        width: 69%;
        float: left;
        padding: 10px;
    }

    .form-div .get-data-btn{
        width: 30%;
        float: right;
    }

    .form-div .get-all-data-btn{
        width: 100%;
    }

    table{
        width: 90%;
        margin: 20px auto;
    }

    table, th, tr, td{
        border: 1px solid #444444;
        border-collapse: collapse;
    }

    table thead th{
        background: #555555;
        color: #FFFFFF;
        border: 1px solid #FFFFFF;
    }

    th, td{
        padding: 8px;
    }

    @media screen and (max-width: 450px){
        .landing h1, .landing p{
            font-size: 1em;
        }

        .landing p{
            letter-spacing: normal;
        }
        .main h4{
            width: 90%;
        }

        .form-div{
            width: 90%;
        }
    }

</style>