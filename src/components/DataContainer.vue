<template>
    <div class="container">
        <header>
            <nav>
                <Dropdown :cols="cols" @add-col="addCol" />
                <FilterInput :cols="cols" @add-filter="addFilter" @search-filters="search" />
            </nav>
            <Filters :filters="filters" @remove-filter="removeFilter" />
        </header>
        <DataTable :showedCols="showedCols" :rows="rows" @delete-row="deleteRow" @edit-row="showEditMenu" />
        <footer>
            <button class="button" @click="isVisibleForm = true">
                <img src="@/assets/icons/add.svg" alt="Adicionar" />
                <p>Adicionar linha</p>
            </button>
        </footer>
        <FormModal :showed="isVisibleForm" @hide-modal="isVisibleForm = false" />
        <EditModal :showed="isVisibleEdit" @hide-modal="isVisibleEdit = false" :currow="rowToEdit" />
        <ConfirmationModal :showed="isVisibleConf" @hide-modal="isVisibleConf = false" @delete-row="confirmDelete" />
    </div>
</template>

<script>
import axios from 'axios';
import Dropdown from './Dropdown.vue';
import DataTable from './DataTable.vue';
import FilterInput from './FilterInput.vue';
import Filters from './Filters.vue';
import FormModal from './FormModal.vue';
import ConfirmationModal from './ConfirmationModal.vue';
import EditModal from './EditModal.vue';

export default {
    components: {
        Dropdown,
        DataTable,
        FilterInput,
        Filters,
        FormModal,
        ConfirmationModal,
        EditModal
    },
    data() {
        return {
            rows: [],
            cols: [
                'Registro ANS', 'CNPJ', 'Razão Social', 'Nome Fantasia', 
                'Modalidade','Logradouro', 'Número', 'Complemento', 'Bairro', 
                'Cidade', 'UF', 'CEP','DDD', 'Telefone', 'Fax', 'Endereço eletrônico', 
                'Representante','Cargo Representante', 'Data Registro ANS'
            ],
            showedCols: [],
            filters: [],
            isVisibleForm: false,
            isVisibleConf: false,
            isVisibleEdit: false,
            ans_recordToDelete: "",
            rowToEdit: {}
        }
    },
    methods: {
        addCol(col) {
            const index = this.showedCols.findIndex(c => col == c);

            if(index === -1) this.showedCols.push(col);
            else             this.showedCols.splice(index, 1);
        },
        addFilter(filter) {
            this.filters.push(filter)
        },
        search() {
            axios.post("http://localhost:5000/search", this.filters)
            .then(res => this.rows = res.data);
        },
        removeFilter(filter) {
            const index = this.filters.findIndex(f => filter == f);
            if(index !== -1) this.filters.splice(index, 1);
        },
        deleteRow(ans_record) {
            this.ans_recordToDelete = ans_record;
            this.isVisibleConf = true;
        },
        confirmDelete() {
            axios.delete(`http://localhost:5000/deleterow/${this.ans_recordToDelete}`).then(res => {
                console.log(res.data);
                if(res.data["success"]) {
                    const index = this.rows.findIndex(row => row["Registro ANS"] == this.ans_recordToDelete);
                    if(index !== -1) this.rows.splice(index, 1);
                }
            });
        },
        showEditMenu(row) {
            this.rowToEdit = row;
            this.isVisibleEdit = true;
        }
    }
}
</script>

<style scoped>
.container {
    background: #fff;
    border-radius: 4px;
    width: 100%;
    height: 100%;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.05);
    padding: 0 50px;
    display: flex;
    flex-direction: column;
}

header {
    padding: 20px 0 10px;
    position: sticky;
    top: 0;
    background: #fff;
    border-bottom: 1px solid #f3f3f3;
}

nav {
    display: flex;
    align-items: center;
    justify-content: space-between;
}


footer {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    margin: 20px 0
}

.button {
    border: none;
    height: 40px;
    padding: 10px;
    border-radius: 4px;
    cursor: pointer;
    background: #F1F1F1;
    transition: background 100ms;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.button:hover {
    background: #ddd;
}

button p {
    margin: 0 10px;
}

</style>