<template>
    <div class="container">
        <div class="row">
            
            <div class="col-md-12 mt-2">
                <router-link class="admin-add-new-book-btn btn btn-outline-dark btn-hover-custom " :to="{ name: 'admin.books.create' }">Add New
                    Book</router-link>
                    <button @click="toggleFilters" class="btn btn-outline-dark  mt-2" v-if="showFilters">
            Hide Filters
        </button>
        <button @click="toggleFilters" class="btn btn-outline-dark btn-hover-custom mt-2" v-else>
            Show Filters
        </button>
            </div>
        </div>

        <div class="row">
            <div class="col-md-2">
                
                <div v-show="showFilters">
                    <div class="grid search">
                        <div class="grid-body">
                            <div class="row">
                                <div>
                                    <h2 class="mt-2 grid-title"><i class="fa fa-filter"></i> <button
                                            @click="clearFilter()" class="btn btn-outline-dark">Reset all</button></h2>
                                    <hr>
                                    <div class="row">
                                        <label> Title:
                                            <div class="input-group input-group-date-filter mb-3">
                                                <input type="text" class="form-control" placeholder="" v-model="TempTitle">
                                                <div class="input-group-append">
                                                    <button @click="clearTitle()" class="btn btn-custom btn-hover-custom-dark" type="button">x</button>
                                                </div>
                                            </div>

                                        </label>
                                    </div>

                                    <div class="row mt-2">
                                        <label> Author:
                                            <div class="input-group input-group-date-filter mb-3">
                                                <input type="text" class="form-control" placeholder="" v-model="TempAuthor">
                                                <div class="input-group-append">
                                                    <button @click="clearAuthor()" class="btn btn-custom btn-hover-custom-dark" type="button">x</button>
                                                </div>
                                            </div>
                                        </label>
                                    </div>
                                    <div class="row mt-2">
                                        <label> Genre:
                                            <div class="input-group input-group-date-filter mb-3">
                                                <input type="text" class="form-control" v-model="TempGenre" />
                                                <div class="input-group-append">
                                                    <button @click="clearGenre()" class="btn btn-custom btn-hover-custom-dark" type="button">x</button>
                                                </div>
                                            </div>
                                        </label>
                                    </div>
                                    <div class="row mt-2">
                                        <label> ISBN:
                                            <div class="input-group input-group-date-filter mb-3">
                                                <input type="text" class="form-control" placeholder="" v-model="TempIsbn">
                                                <div class="input-group-append">
                                                    <button @click="clearIsbn()" class="btn btn-custom btn-hover-custom-dark" type="button">x</button>
                                                </div>
                                            </div>
                                        </label>
                                    </div>
                                    <div class="row mt-2">
                                        <label> Published On:
                                            <div class="input-group input-group-date-filter mb-3">
                                                <input type="date"
                                                    class="border-none-text-box published_on_box form-control"
                                                    placeholder="" v-model="TempPublished_on">

                                                <div class="input-group-append">
                                                    <button @click="clearPublishedOn()" class="btn btn-custom btn-hover-custom-dark" type="button">x</button>
                                                </div>
                                            </div>
                                        </label>
                                    </div>
                                </div>
                            </div>
                            <div class="row mt-2">
                                <button @click="applyFilter()" class="btn btn-block btn-outline-dark"> Apply </button>
                            </div>

                        </div>
                    </div>
                </div>

            </div>
            <div class="col-md-10">
                <div class="row">
                    <div class="col-md-12 mt-2">
                        <EasyDataTable theme-color="#000" table-class-name="book-table" header-text-direction="center"
                            body-text-direction="center" :headers="headers" :items="items" v-model:payload="payload"
                            v-model:server-options="serverOptions" :server-items-length="serverItemsLength"
                            :loading="loading" :rowsItems="rowsItemsList" buttons-pagination>
                            <template #item-action="item">
                                
                                <router-link class="btn btn-outline-dark btn-sm btn-hover-custom"
                                    :to="{ name: 'admin.books.edit', params: { id: item.id } }"
                                    style="margin-left: 10px;"><svg xmlns="http://www.w3.org/2000/svg" width="16"
                                        height="16" fill="currentColor" class="bi bi-pen" viewBox="0 0 16 16">
                                        <path
                                            d="m13.498.795.149-.149a1.207 1.207 0 1 1 1.707 1.708l-.149.148a1.5 1.5 0 0 1-.059 2.059L4.854 14.854a.5.5 0 0 1-.233.131l-4 1a.5.5 0 0 1-.606-.606l1-4a.5.5 0 0 1 .131-.232l9.642-9.642a.5.5 0 0 0-.642.056L6.854 4.854a.5.5 0 1 1-.708-.708L9.44.854A1.5 1.5 0 0 1 11.5.796a1.5 1.5 0 0 1 1.998-.001m-.644.766a.5.5 0 0 0-.707 0L1.95 11.756l-.764 3.057 3.057-.764L14.44 3.854a.5.5 0 0 0 0-.708l-1.585-1.585z" />
                                    </svg></router-link>

                                <button @click="deleteBook(item.id)" class="btn btn-outline-danger btn-sm"
                                    style="margin-left: 10px;"><svg xmlns="http://www.w3.org/2000/svg" width="16"
                                        height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
                                        <path
                                            d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0z" />
                                        <path
                                            d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4zM2.5 3h11V2h-11z" />
                                    </svg></button>
                            </template>
                        </EasyDataTable>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue';
export default {
    setup() {
        const headers = [
            { text: "ID", value: "id", sortable: true },
            { text: "Book Title", value: "title", sortable: true },
            { text: "Author", value: "author", sortable: true },
            { text: "Genre", value: "genre", sortable: true },
            { text: "ISBN", value: "isbn", sortable: true },
            { text: "Published On", value: "published_on", sortable: true },
            { text: "Action", value: "action", class: 'dummy' },
        ];
        const showFilters = ref(false);

        return {
            headers,
            items: [],
            showFilters,
        };
    },
    data() {
        return {
            serverOptions: {
                page: 1,
                rowsPerPage: 5,
                sortBy: "id",
                sortType: "desc",
            },
            loading: false,
            serverItemsLength: null,
            payload: [],
            genres: [],
            TempTitle: "",
            TempAuthor: "",
            TempIsbn: "",
            TempGenre: "",
            TempPublished_on: "",
            rowsItemsList: [5, 10]
        };
    },
    mounted() {
        this.getBooksList();
    },
    watch: {
        serverOptions: {
            handler() {
                this.getBooksList();
            },
            deep: true,
        }
    },
    methods: {
        async getBooksList() {
            this.loading = true;
            this.payload = { ...this.serverOptions };
            this.payload.search = {
                title: this.title,
                author: this.author,
                isbn: this.isbn,
                genre: this.genre,
                published_on: this.published_on
            };
            var response = await this.frontendApi("POST", "books", this.payload);
            if (response) {
                this.items = response.data.records;
                this.serverItemsLength = response.data.recordsTotal;
                this.loading = false;
            }
        },
        applyFilter() {
            this.title = this.TempTitle,
                this.author = this.TempAuthor;
            this.isbn = this.TempIsbn;
            this.genre = this.TempGenre;
            this.published_on = this.TempPublished_on;
            this.resetServerOptions();
            this.getBooksList();
        },
        clearTitle() {
            this.title = "";
            this.TempTitle = "";
        },
        clearAuthor() {
            this.author = "";
            this.TempAuthor = "";
        },
        clearGenre() {

            this.genre = "";
            this.TempGenre = "";
        },
        clearIsbn() {
            this.isbn = "";
            this.TempIsbn = "";
        },
        clearPublishedOn() {
            this.published_on = "";
            this.TempPublished_on = "";
        },
        clearFilter() {
            this.resetServerOptions();
            this.title = "";
            this.author = "";
            this.isbn = "";
            this.genre = "";
            this.published_on = "";
            this.TempTitle = "";
            this.TempAuthor = "";
            this.TempIsbn = "";
            this.TempGenre = "";
            this.TempPublished_on = "";
            this.getBooksList();
        },
        resetServerOptions() {
            this.serverOptions.page = 1;
            this.serverOptions.sortBy = "id";
            this.serverOptions.sortType = "desc";
        },
        toggleFilters() {
            this.showFilters = !this.showFilters;
        },
        async deleteBook(id) {
            if (confirm('Are you sure?')) {
                this.store.dispatch('pageLoader', { show: true })
                var response = await this.adminApi('DELETE', 'books/' + id)
                if (response) {
                    this.store.dispatch('pageLoader', { show: false })
                    this.store.dispatch('alert', {
                        messages: response.message,
                        alertClass: 'success'
                    })
                }
                this.getBooksList();
            }
        }
    }
}
</script>