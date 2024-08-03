<template>
    <div class="container">
        <div class="row">
            <div class="">

                <div class="alert alert-danger mt-4" v-if="errors.length">
                    <ul class="mb-0">
                        <li v-for="(error, index ) in errors" :key="index">{{ error }}</li>
                    </ul>
                </div>

                <form @submit.prevent="updateContact" novalidate>
                    <fieldset>
                        <div class="form-group">
                            <label class="form-label mt-4">Nome</label>
                            <input type="text" class="form-control" placeholder="Insira o nome do contato" v-model="contact.nome">
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label mt-4">Email</label>
                            <input type="email" class="form-control" placeholder="Insira o email do contato" v-model="contact.email">
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label mt-4">Celular</label>
                            <input type="text" class="form-control" placeholder="Insira o número de celular -- XXXXXXXXXXX" v-model="contact.telefone">
                        </div>

                        <router-link to="/" class="btn btn-primary mt-4 col-md-12" >Salvar</router-link>
                    </fieldset>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
    export default {
        name: 'EditContact',
        data(){
            return {
                contact: {},
                nome: '',
                email: '',
                telefone: '',
                errors: []
            }
        },

        created(){
            this.getContactById();
        },

        methods: {
            async getContactById(){
                let url = `http://127.0.0.1:8000/api/contacts/${this.$route.params.id}`
                await axios.get(url).then(response => {
                    console.log(this.contact);
                    this.contact = response.data;
                })
            },

            async updateContact(){
                this.errors = [];
                if(!this.contact.nome){
                    this.errors.push('O nome é obrigatório');
                }
                if(!this.contact.email){
                    this.errors.push('O email é obrigatório');
                }
                if(!this.contact.telefone){
                    this.errors.push('O telefone é obrigatório');
                }
                
                if(!this.errors.length){
                    let formData = new FormData();

                    formData.append('nome', this.contact.nome);
                    formData.append('email', this.contact.email);
                    formData.append('telefone', this.contact.telefone);
                    let url = `http://127.0.0.1:8000/api/contacts/update/${this.$route.params.id}`
                    await axios.put(url, formData, {headers:{'Content-Type': 'application/json'}}).then(response => {
                        
                        console.log(response);
                            this.contact = {};
                            this.nome = '';
                            this.email = '';
                            this.telefone = '';
                    }).catch(error => {
                        console.log(error);
                    });
                }
            },

        },

        mounted: function(){
            console.log('Edit component mounted.');
        }
    }
</script>