<template>
  <div>
        <ul v-for="data in allData" :key="data.id" class="menu-list" >
            <li>
                {{data.name}} / {{data.age}}
            </li>
        </ul>

    <!-- {{allData}} -->

    <p>
        <input v-model="name" placeholder="edit me">
        <input v-model="age" placeholder="edit me">
        <button v-on:click='post'>送信</button>
    </p>
        <!-- The core Firebase JS SDK is always required and must be listed first -->
        <!-- <script src="https://www.gstatic.com/firebasejs/7.19.1/firebase-app.js"></script> -->

        <!-- TODO: Add SDKs for Firebase products that you want to use
            https://firebase.google.com/docs/web/setup#available-libraries -->

  </div>
</template>

<script>
import firebase from "firebase/app";
import "firebase/firestore";

export default {
    components: {},

    data(){
        return{
            db: {},
            allData: [],
            name: '',
            age: 0
        }
    },

    methods: {
        init: () => {
            const config = {
                apiKey: "AIzaSyDErAYqDCowJQgbTYd7qx5hRfOCPcaRkbQ",
                authDomain: "class20200523.firebaseapp.com",
                databaseURL: "https://class20200523.firebaseio.com",
                projectId: "class20200523",
                storageBucket: "class20200523.appspot.com",
                messagingSenderId: "8916758614",
                appId: "1:8916758614:web:5576fbb74c08ca144acbf7"
            };

            // Initialize Firebase
            firebase.initializeApp(config);
        },

        post: function(){
            const testId = firebase.firestore().collection('test1').doc().id; //ユニークなIDを生成
            const docRef = firebase.firestore().collection('test1').doc(testId);
            const setAda = docRef.set({
                name: this.name,
                age: this.age
            });
            this.get();
        },

        get: function(){
            this.allData = [];
            firebase.firestore().collection('test1').get().then(snapshot => {
                snapshot.forEach(doc => {
                    // console.log(doc);
                    this.allData.push(doc.data());
                })
            });
        }

    },

    mounted(){
        this.init();
        this.get();

        // this.post();
    },
}


</script>