<template>
    <div>
        <h2 class="text-danger text-center text-capitalize">Captura tus ideas</h2>
        
        <div class="card text-white bg-secondary" style="padding: 4%;">
            <h4 class="text-light text-capitalize">En que estás pensando?</h4>
            <form action="" v-on:submit.prevent="createIdea">
                <div class="input-group">
                    <input type="text" id="" v-model="newIdea" class="form-control input-sm" maxlength="255">
                    <span class="input-group-btn">
                        <button class="btn btn-primary">Agregar</button>
                    </span>
                </div>
            </form>
            <hr>
            <ul class="list-unstyled">
                <li v-for="idea in ideas">
                    <p>
                        <small class="text-warning">
                            <em>{{ since(idea.created_at) }}</em>
                        </small>
                        {{ idea.description }}
                    </p>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import toastr from 'toastr'
    import moment from 'moment'

    moment.locale('es');

    export default {
        data(){
            return{
                ideas:[],
                newIdea: ''
            }
        },
        created: function () {
          this.getIdeas();
        },
        methods:{
            getIdeas: function () {
                var urlIdeas = 'mis-ideas';
                var vm = this;

                axios.get(urlIdeas).then(function(response){
                    vm.ideas = response.data
                });
            },
            since: function(d){
                return moment(d).fromNow();
            },
            createIdea: function(){
                var url = 'guardar-idea';
                var vm = this;

                axios.post(url, {
                    description: vm.newIdea
                }).then(function(response){
                    vm.getIdeas();
                    vm.newIdea = '';
                    toastr.success('Idea Registrada');
                }).catch(error => {
                    toastr.error('Error');
                });  
            }
        }
    }
</script>