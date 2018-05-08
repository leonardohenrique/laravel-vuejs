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
                        console.info(data);
                        this.albums = data.data;
                        this.pageCount = data.meta.last_page;
                    });
            },
        }        
    }
</script>
