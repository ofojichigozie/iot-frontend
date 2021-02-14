<template>
    <div class="main">
        <div class="top">
            <div class="container">
                <div class="top-inner">
                    <span style="color: #000000; font-style: italic;">{{ email }}</span>
                    <router-link to="/home"><button class="back-btn">Back</button></router-link>
                    <button class="logout-btn" v-on:click="logout()">Logout</button>
                </div>
            </div>
        </div>

        <div class="main">
            <div class="main-head">
                <h3>HELP DESK</h3>
            </div>

            <div class="status-table-div">
                <h4 class="heading">CATTLE STATUS</h4>
                <table>
                    <!-- <thead>
                        <th></th>
                        <th>DESCRIPTION</th>
                    </thead> --> 
                    <tbody>
                        <tr v-bind:key="cattleStatus.key" v-for="cattleStatus in cattleStatuses">
                            <td> {{ cattleStatus.status }}</td>
                            <td> {{ cattleStatus.details }} </td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <div class="note-table-div">
                <h4 class="heading">N/B</h4>
                <table>
                    <!-- <thead>
                        <th></th>
                        <th>DESCRIPTION</th>
                    </thead> -->
                    <tbody>
                        <tr v-bind:key="note.key" v-for="note in notes">
                            <td> {{ note.status }}</td>
                            <td> {{ note.details }} </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Help',

        data(){
            return {
                email: 'user@gmail.com',
                cattleStatuses: [
                    {status: 'Temperature', details: 'The normal body temperature of a cattle is the range of 38 - 42 degree celcius. When the temperature is below or above (<38 degree, >42 degree) this range; seek the veterinarian\'s attention.'},
                    {status: 'Humidity', details: 'At stable or seemingly stable condition, the cattle has a humidity range of 1 - 90%. At any percentage above this value indicates severe stress; seek the attention of the veterinarian.'},
                    {status: 'Heartbeat', details: 'The cattle have heartbeat in the range of 48 - 84 beats per minutes (bpm). Note: Beat per minute indicates the number of times the cattle\'s heart is contracting or expanding in a minute. If the heartbeat is beyond the value, place the animal in the resting position or you can call the veterinarian doctor.'},
                    {status: 'Location', details: 'The GPS helps to track the location of the cattle by providing satellite signal to its receiver to compute animal position, time, latitude, longitude etc. and as well display their location on a Google map.'}
                ],
                notes: [
                    {status: 'Get Instant Data', details: 'On clicking this button, the real time data is retrieved from various sensors regarding the cattle at that instance'},
                    {status: 'Log', details: 'Displays the readings from various sensors at a particular time and date.'},
                    {status: 'Update Interval', details: 'The livestock data are captured from the remote hardware system every 12 hours.'}
                ]
            }
        },

        mounted(){
            try{
                let loggedInUser = localStorage.getItem("iot-admin");

                if(loggedInUser != null){
                    this.email = loggedInUser;
                }else{
                    window.location.href = '/';
                }
            }catch(e){
                console.log("Error gettimg from local storage");
            }
        },

        methods: {
            logout: function(){
                try{
                    localStorage.removeItem("iot-user");
                }catch(e){
                    console.log("Error removing from local storage");
                }

                window.location.href = '/';
            }
        }
    }
</script>

<style scoped>
    *{
        box-sizing: border-box;
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

    .back-btn{
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

    .main{
        width: 100%;
        /* height: 100vh; */
        background: linear-gradient(180deg, rgba(0, 0, 0, 0.9) 25.62%, rgba(0, 0, 0, 0.74) 100%), url('~@/assets/img/back01.jpg');
        background-repeat: no-repeat;
        background-position: center;
        background-size: cover;
        padding-bottom: 10px
    }

    .main-head{
        text-align: center;
    }

    .status-table-div{
        width: 80%;
        margin: auto;
        background-color: #FFFFFF;
        padding: 20px;
        border-radius: 15px;
    }

    .note-table-div{
        width: 80%;
        margin: 20px auto;
        background-color: #FFFFFF;
        padding: 20px;
        border-radius: 15px;
    }

    table{
        width: 100%;
    }

    table, tr, td{
        border: 1px solid #444444;
        border-collapse: collapse;
    }

    th, td{
        padding: 8px;
    }

    .main-head{
        color: #FFFFFF;
    }

    .heading{
        width: 20%;
        float: right;
        text-align: center;
        padding: 5px;
        border-radius: 5px;
        background-color: #444444;
        color: #FFFFFF;
    }

    @media screen and (max-width: 450px){
        .heading{
            width: 40%;
        }

        table{
            font-size: 0.8em;
        }
    }

</style>