
<template>
    <div class="container">
        <p v-if="showButton">
            <button class="btn primary" @click="downloadComents">Загрузить комментарии</button>
        </p>
        
        <div v-else>
            <div class="loader" v-if="load"></div>

            <div class="card" v-else>
                <h2>Комментарии</h2>
                <ul class="list" v-for="coment in coments" :key="coment.name">
                    <li class="list-item">
                    <div>
                        <p><strong>{{coment.email}}</strong></p>
                        <small>{{coment.body}}</small>
                    </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
    

    data() {
        return{
            showButton: true,
            load: false,
            coments: []
        }
    },
    methods: {
        
        downloadComents() {
            this.showButton = false
            this.load = true

            this.getDataBase()
        },
        async getDataBase() {
        const {data} = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')

        this.coments = data

        console.log(data)
        this.load = false
        }
    }
    
}
</script>
