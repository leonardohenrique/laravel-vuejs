<template>
    <div class="container">
        <h1>Albums</h1>

        <table class="table table-striped table-bordered">
            <thead>
                <tr>
                    <th>Id</th>
                    <th>Artist</th>
                    <th>Title</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="album in albums">
                    <td>{{ album.id }}</td>
                    <td>{{ album.artist }}</td>
                    <td>{{ album.title }}</td>
                </tr>
            </tbody>
        </table>
    
        <paginate
            :page-count="pageCount"
            :click-handler="fetch"
            :prev-text="'Previous'"
            :next-text="'Next'"
            :container-class="'pagination'"
            :page-class="'page-item'"
            :page-link-class="'page-link'"
            :next-link-class="'page-link'"
            :prev-link-class="'page-link'">
        </paginate>

    </div>
</template>

<script>
    export default {
        data() {
            return {
                albums: [],
                pageCount: 1,
                endpoint: 'api/albums'
            };
        },

        created() {
            this.fetch();
        },

        methods: {
            fetch(page = 1) {
                axios.get(this.endpoint + '?page=' + page)
                    .then(({data}) => {
                        this.albums = data.data;
                        this.pageCount = data.last_page;
                    });
            },
        }        
    }
</script>
