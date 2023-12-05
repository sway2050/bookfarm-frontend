<template>
    <user-common-header />
    <div class="container">
        <div class="row">
            <div class="col-md-12 mt-2">
                <button @click="toggleFilters" class="btn btn-outline-dark  mt-2" v-if="showFilters">
                    Hide Filters
                </button>
                <button @click="toggleFilters" class="btn btn-outline-dark btn-hover-custom mt-2" v-else>
                    Show Filters
                </button>
            </div>
        </div>
    </div>
    <div class="container">
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
                                                <input type="date" class="border-none-text-box published_on_box form-control"
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
                                    :to="{ name: 'books.view', params: { id: item.id } }">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                                        class="bi bi-eye" viewBox="0 0 18 20">
                                        <path
                                            d="M8 1.5a6.5 6.5 0 0 1 6.354 8A6.5 6.5 0 0 1 8 14.5a6.5 6.5 0 0 1-6.354-8A6.5 6.5 0 0 1 8 1.5zm0 1a5.5 5.5 0 0 0-5.5 5.5A5.5 5.5 0 0 0 8 13.5a5.5 5.5 0 0 0 5.5-5.5A5.5 5.5 0 0 0 8 2.5zM8 4a3 3 0 1 1 0 6 3 3 0 0 1 0-6zM7 6a1 1 0 1 0 0 2 1 1 0 0 0 0-2z" />
                                    </svg>View
                                </router-link>

                            </template>
                        </EasyDataTable>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import UserCommonHeaderVue from '../layout/UserCommonHeader.vue';
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
        toggleFilters() {
            this.showFilters = !this.showFilters;
        },
        resetServerOptions() {
            this.serverOptions.page = 1;
            this.serverOptions.sortBy = "id";
            this.serverOptions.sortType = "asc";
        }
    },
    components: {
        'user-common-header': UserCommonHeaderVue
    }
}
</script>