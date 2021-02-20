<template>
    <div class="content-area">
        <h3>Database View</h3>
        <div class="table-wrapper">
        <vuetable ref="vuetable"
        api-url="https://pb-dash-api.herokuapp.com/api/v1/read_books"
        pagination-path="pagination"
        data-path="books"
        :fields="['title', 'author', 'genre', 'category', 'status']"
        @vuetable:pagination-data="onPaginationData"
        :css="{tableClass: 'table table-striped table-dark', loadingClass: 'Please wait...'  }"
        />
        </div>
        <vuetable-pagination ref="pagination" @vuetable-pagination:change-page="onChangePage" />
    
    </div>
</template>
<script>
import Vuetable from 'vuetable-2';
import VuetablePagination from '../../node_modules/vuetable-2/src/components/VuetablePagination.vue';

export default {
    components: {
        Vuetable, 
        VuetablePagination
    },
    methods: {
        // when the pagination data is available, set it to pagination component
        onPaginationData (paginationData) {
            this.$refs.pagination.setPaginationData(paginationData)
        },
        onChangePage (page) {
            this.$refs.vuetable.changePage(page)
        }
    }
}
</script>