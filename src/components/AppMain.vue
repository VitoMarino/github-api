<script>
import axios from 'axios';

export default {
    data() {
        return {
            username: '',
            user: null,
            error: null
        };
    },
    methods: {
        // async è un metodo asincrono. Queste funzioni asincrone permettono di eseguire operazioni che possono richiedere tempo
        // senza bloccare l'interfaccia utente
        async getUserData() {
            // try viene utilizzato per eseguire codice che potrebbe fallire. Se va in errore catch cattura e lo gestisce
            try {
                const token = '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';
                // await fa si che la funzione aspetti fino a quando la richiesta non ottiene risposta.
                const response = await axios.get(`https://api.github.com/users/${this.username}`, {
                    // Con headers passo il token per autenticarmi alla richiesta API
                    headers: {
                        'Authorization': `token ${token}`
                    }
                });
                // Se la richiesta viene eseguita con successo viene memorizzato il tutto nella variabile user
                this.user = console.log(response.data);
                this.user = response.data;  // Salva i dati dell'utente
                this.error = null;  // Reset degli errori
            } catch (err) {
                this.error = 'Utente non trovato o errore nella richiesta';
                this.user = null;
            }
        }
    }
};
</script>

<template>
    <div class="my_div">
        <div>
            <h1>
                Cerca utente in GitHub
            </h1>
        </div>
        <hr>

        <div class="d-flex justify-content-center">
            <input class="input-group" style="width: 13rem;" @keyup.enter="getUserData" v-model="username" placeholder="Scrivi username">
            <button class="btn btn-primary ms-3" @click="getUserData">
                Cerca
            </button>
        </div>

        <div v-if="user" class="card" style="width: 18rem;">
            <img :src="user.avatar_url" :alt="user.login">
            <div class="card-body">
                <h5 class="card-title">
                    {{ user.login }}
                </h5>
                <p class="card-text">
                    {{ user.bio }}
                </p>
                <a :href="user.html_url">
                    Vai al profilo
                </a>
            </div>
        </div>
        <p v-if="error" class="text-danger mt-4">{{ error }}</p>
    </div>
</template>

<style scoped>
div.my_div {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    text-align: center;
    margin-top: 30px;
}
</style>