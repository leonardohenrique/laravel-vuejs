<template>
    <div class="container">
        <h1>Album</h1>

        <div class="alert alert-danger" role="alert" v-if="error">
            {{ error }}
        </div>

        <form v-on:submit.prevent="onSubmit" >
            <div class="form-group">
                <label for="artist">Artist</label>
                <input type="text" class="form-control" :class="{'is-invalid': errors.artist}" id="artist" v-model="album.artist">
                <div v-if="errors.artist" class="invalid-feedback">{{ errors.artist[0] }}</div>                
            </div>   
            <div class="form-group">
                <label for="title">Title</label>
                <input type="text" class="form-control" :class="{'is-invalid': errors.title}" id="title" v-model="album.title">
                <div v-if="errors.title" class="invalid-feedback">{{ errors.title[0] }}</div>
            </div>     
            <button type="submit" class="btn btn-primary">Submit</button>            
        </form>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                album: {},
                error: null,
                errors: {},
                endpoint: '/api/albums'
            };
        },

        methods: {
            onSubmit() {
                this.error = null;
                this.errors = [];

                axios.post(this.endpoint, this.album)
                    .then(({data}) => this.onSubmitComplete())
                    .catch(({response}) => this.onSubmitFailed(response));
            },

            onSubmitComplete() {
                window.location.href = '/albums';
            },      

            onSubmitFailed(response) {
                if (response.data.message) {
                    this.error = response.data.message;
                }                

                if (response.data.errors) {
                    this.errors = response.data.errors;
                }                
            },
      
        }        
    }
</script>
