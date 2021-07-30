<template>
    <div class="table-container">
        <table>
            <tr>
                <th v-for="col in showedCols" :key="col">{{ col }}</th>
            </tr>
            <tr v-for="row in rows" :key="row.CNPJ">
                <td v-for="col in showedCols" :key="col">{{ row[col] }}</td>
                <td class="config-row" v-if="showedCols.length > 0">
                    <button class="edit-btn" @click="editrow(row)/*;*/">
                        <img src="@/assets/icons/edit.svg" alt="Adicionar" />
                    </button>
                    <button class="delete-btn" @click="deleterow(row['Registro ANS'])">
                        <img src="@/assets/icons/delete.svg" alt="Adicionar"/>
                    </button>
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
export default {
    props: [
        "showedCols",
        "rows"
    ],
    methods: {
        deleterow(ans_record) {
            this.$emit("delete-row", ans_record);
        },
        editrow(row) {
            const rowToEdit = {}

            Object.keys(row).forEach(key => rowToEdit[key] = row[key]);

            this.$emit('edit-row', rowToEdit);
        }
    }
}
</script>

<style scoped>
.table-container {
    overflow: scroll;
    height: 100%;
    border: 1px solid #f3f3f3;
}

table {
    border-collapse: collapse;
    width: 100%;
}

td, th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 10px;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    color: #2c3e50;
}

.table-container::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

.table-container::-webkit-scrollbar-track {
  background: #f3f3f3;
}

.table-container::-webkit-scrollbar-thumb {
  background-color: #ccc;
  border-radius: 20px;

  transition: background 100ms;
}

.table-container::-webkit-scrollbar-thumb:hover {
    background-color: #bbb;
}

.config-row {
    min-width: 76px;
    width: 76px;
    padding: 0 5px;
    border: none;
}

.config-row button {
    display: inline;
    width: 30px;
    height: 30px;
    background: #ccc;
    border: none;
    cursor: pointer;
    transition: background 100ms;
}

.edit-btn {
    margin-right: 5px;
}

.edit-btn:hover {
    background: #ffc107;
}

.delete-btn:hover {
    background: #dc3545;
}

</style>