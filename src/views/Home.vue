<template>
    <div class="home">
        <Inputs />
        <ItemCountry :pagination="pagination" />
        <PagePagination
            @paginate="spliceArrayCountries"
            :pagination-start="paginationStart"
            :itens-in-page="itensInPage"
            :number-of-itens="numberOfItens"
        />
    </div>
</template>

<script>
// @ is an alias to /src
import { api } from '../services.js';
import Inputs from '@/components/Inputs.vue';
import ItemCountry from '@/components/ItemCountry.vue';
import PagePagination from '@/components/PagePagination.vue';

export default {
    name: 'Home',
    components: {
        Inputs,
        ItemCountry,
        PagePagination
    },
    data() {
        return {
            contries: [],
            paginationStart: 0,
            pagination: [],
            itensInPage: 8
        };
    },
    computed: {
        url() {
            return this.$route.query;
        },
        numberOfItens() {
            return this.contries.length;
        }
    },
    watch: {
        async url() {
            this.url.endpoint ? this.filterCountries(this.url) : this.getAllCountry();
        }
    },
    methods: {
        async filterCountries(query) {
            this.contries = [];
            const { endpoint } = query;
            const countryFiltered = await api.getCountry(`${endpoint}`);
            !countryFiltered.status ? (this.contries = countryFiltered) : console.log('page not found');
            this.spliceArrayCountries((this.paginationStart = 0));
        },
        async getAllCountry() {
            this.contries = await api.getCountry(`/all?fields=flags,name,capital,population,region`);
            this.spliceArrayCountries(this.paginationStart);
        },
        spliceArrayCountries(start) {
            this.paginationStart = start;
            return (this.pagination = this.contries.slice(start, start + this.itensInPage));
        }
    },
    created() {
        this.getAllCountry();
    }
};
</script>

<style scoped>
.home {
    max-width: 1200px;
    padding: 40px 100px;
    margin: auto;
}
</style>
