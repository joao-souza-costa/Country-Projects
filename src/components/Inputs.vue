<template>
    <div class="input">
        <div class="search-input">
            <input class="search" placeholder="Search for a country..." v-model="countryName" />
            <button class="search-button">
                <ion-icon name="search-outline"></ion-icon>
            </button>
        </div>
        <div class="filter-container">
            <button class="filter" @click.prevent="openModal">
                {{ label }}
                <ion-icon name="chevron-down-outline"></ion-icon>
            </button>
            <ul v-if="isOpen" class="options">
                <li class="o" v-for="(region, index) in regions" :key="index" @click="changeRegion(region)">
                    {{ region }}
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
/* import { api } from "../services.js";
 */ export default {
    name: 'inputs',
    data() {
        return {
            isOpen: false,
            regions: ['Africa', 'Americas', 'Asia', 'Europe', 'Oceania'],
            label: 'Filter by Region',
            countryName: ''
        };
    },
    methods: {
        openModal() {
            this.isOpen = !this.isOpen;
        },
        async changeRegion(region) {
            if (region !== this.label) {
                await this.$router.push({ query: { endpoint: `/continent/${region}` } });
                this.label = region;
            }
            this.isOpen = false;
        },
        async changeName(name) {
            await this.$router.push({ query: { endpoint: `/name/${name}` } });
        }
    },
    watch: {
        countryName(name) {
            if (name) {
                this.changeName(name);
            } else {
                this.$router.push({ path: '/' });
            }
        }
    }
};
</script>

<style scoped>
.input {
    display: flex;
    justify-content: space-between;
}

.search,
.filter,
ion-icon {
    background-color: var(--dark-blue);
    border: none;
    color: #fff;
}

.search-button {
    background: transparent;
    width: 40px;
    position: relative;
    right: 40px;
    border: none;
    cursor: pointer;
    border-left: 1px solid black;
}
.search-input {
    display: flex;
}

.search {
    width: 300px;
    padding: 17px;
    border-radius: 5px;
    box-shadow: 1px 3px 5px rgba(0, 0, 0, 0.24);
}
.filter-container {
    width: 200px;
    position: relative;
}
.filter,
.options {
    text-align: left;
    border-radius: 5px;
}
.filter {
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    padding: 15px;
    box-shadow: 1px 3px 5px rgba(0, 0, 0, 0.24);
    font-family: 'Nunito Sans', sans-serif;
    font-size: 14px;
}

.options {
    position: absolute;
    background-color: var(--dark-blue);
    margin-top: 3px;
    width: 100%;
    box-shadow: 1px 3px 5px rgba(0, 0, 0, 0.24);
}
.options li {
    color: #fff;
    padding: 0 15px;
    margin: 10px 0;
    cursor: pointer;
    font-size: 14px;
}
.options li:hover {
    color: #000;
}

ion-icon {
    font-size: 16px;
}
</style>
