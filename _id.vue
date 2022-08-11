<template>
    <div>

        <pre>{{ book }}</pre>

        <v-switch v-model="book.Available">

        </v-switch>
        <label for="title" class="font-weight-bold">TITLE</label>
        <v-text-field title="title" v-model="book.title" :rules="[v => !!v || 'Title is required']"></v-text-field>
        <label for="page" class="font-weight-bold">PAGE</label>
        <v-text-field title="page" v-model="book.page" type="int" :rules="[v => !!v || 'Page is required']"></v-text-field>
        <label for="author" class="font-weight-bold">AUTHOR</label>
        <v-text-field title="author" v-model="book.author" :rules="[v => !!v || 'Author is required']"></v-text-field>
        <label for="published" class="font-weight-bold">PUBLISHED</label>
        <v-text-field title="published" v-model="book.published" :rules="[v => !!v || 'Published is required']"></v-text-field>
        <v-btn @click="save">
            Save
        </v-btn>
        <v-btn @click="close">
            Close
        </v-btn>

    </div>
</template>
<script>
export default {
    name: 'Books-id',
    data() {
        return {
            bookId: this.$route.params.id,
            book: {},
            isNew: false,
        }
    },

    methods: {
        save() {
          const configHeaders = {
            "content-type": "application/json",
             "Accept": "application/json"
            };
            if (this.isNew) {
              const data={
                title: this.book.title,
                page: parseInt(this.book.page),
                author: this.book.author,
                published: this.book.published
              }
                this.$axios.$post('api/insert', data,configHeaders)
                    .then(response => {
                        this.$router.push('/Books')
                    })
            } else {
                this.$axios.$put('api/update?id=' + this.book.Id, this.book)
                    .then(response => {
                        this.$router.push({
                            path: '/Books',
                        })
                    })
            }

        },
        close() {
            this.$router.push({
                path: '/Books',
            })
        }
    },
    mounted() {
        // this.$axios.$get('/api/books/' + this.bookId)
        //     .then(response => {
        //         this.book = response
        //         console.log(response)
        //     })
        if (this.bookId !== 'add') {
            this.$axios.$get('/api/find?id=' + this.bookId)
                .then(response => {
                    if (response.length > 0) {
                        this.book = response[0]
                    } else {
                        this.$router.push('/Books')
                    }
                    // this.book = response
                    // console.log(response)
                })
        }
        else {
            this.isNew = true
        }
    },
}

</script>
