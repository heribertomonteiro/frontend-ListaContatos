<template>
    <div class="container">
        <table class="table table-hover mt-4">
            <thead>
                <tr>
                    <th scope="col">Nome</th>
                    <th scope="col">Email</th>
                    <th scope="col">Celular</th>
                    <th scope="col">Ações</th>
                </tr>
            </thead>
            <tbody v-for="contact in contacts" :key="contact.id">
                <tr class="class-secondary">
                    <th scope="row">{{ contact.nome }}</th>
                    <th scope="row">{{ contact.email }}</th>
                    <th scope="row">{{ contact.telefone }}</th>
                    <th scope="row">
                        <router-link :to="{ name: 'EditContact', params: {id: contact.id}}" class="btn btn-primary btn-sm">Editar</router-link>
                        <button class="btn btn-danger btn-sm" @click.prevent="deleteContact(contact.id)">Deletar</button>
                    </th>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    import axios from 'axios';
    export default {
        name: 'ContactList',
        data() {
            return {
                contacts: Array
            }
        },
        created() {
            this.getContacts();
        },
        methods: {
            async getContacts() {
                let url = 'http://127.0.0.1:8000/api/contacts';
                await axios.get(url).then(response => {
                    this.contacts = response.data.contacts;
                    console.log(this.contacts);
                }).catch(error => {
                    console.log(error);
                });
            },

            async deleteContact(id){
                let url = `http://127.0.0.1:8000/api/contacts/${id}`
                await axios.delete(url).then(response => {
                    console.log(response);
                    this.getContacts();
                }).catch(error => {
                    console.log(error);
                });
            }
        },
        
        mounted() {
            console.log('Componente montado');
        }
    }
</script>