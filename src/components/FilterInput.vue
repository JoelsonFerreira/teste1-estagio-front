<template>
    <div class="search-container">
        <input 
            type="search" 
            placeholder="Digite o texto de busca" 
            v-model="searchText"
        />
        <select id="cols" v-model="searchCol">
            <option v-for="col in cols" :key="col" :value="col">{{ col }}</option>
        </select>
        <button v-on:click="addFilter">
            <img src="@/assets/icons/add.svg" alt="Adicionar" />
            <p>Adicionar filtro</p>
        </button>
        <button v-on:click="search">
            <img src="@/assets/icons/search.svg" alt="Buscar" />
        </button>
    </div>
</template>

<script>
export default {
    props: ["cols"],
    data() {
        return {
            searchText: "",
            searchCol: ""
        }
    },
    methods: {
        addFilter() {
            if(this.searchCol.trim() === "" || this.searchText.trim() === "") return;
            
            const filter = {}

            filter[this.searchCol] = this.searchText;

            this.$emit("add-filter", filter);

            this.searchText = "";
            this.searchCol = "";
        },
        search() {
            this.$emit("search-filters");
        }
    }
}
</script>

<style scoped>
.search-container {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    flex-wrap: wrap;
    width: 100%;
    max-width: 900px;
}

input,
select {
    height: 40px;
    padding: 10px 20px;
    width: 100%;
    max-width: 250px;
    border-radius: 4px;
    border: 1px solid #ddd;
    outline: none;
    margin: 0 10px;
}

button {
    border: none;
    height: 40px;
    padding: 10px;
    border-radius: 4px;
    margin: 0 10px;
    cursor: pointer;
    background: #F1F1F1;
    transition: background 100ms;
    display: flex;
    align-items: center;
}

button:hover {
    background: #ddd;
}

button p {
    margin: 0 10px;
}

option {
    font-size: 16px;
}

@media (max-width: 1260px) {
    .search-container {
        max-width: 100%;
        justify-content: center;
    }
}

</style>