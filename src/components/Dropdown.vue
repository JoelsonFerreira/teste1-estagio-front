<template>
    <div class="dropdown">
        <button v-on:click="showDropdown">
            colunas visíveis
            <img src="@/assets/icons/expand_more.svg" alt="Expandir" />
        </button>
        <div class="columns" v-bind:style="dropdownStyle">
            <label>
                Marcar todos
                <input type="checkbox" v-bind:value="col" v-on:change="addAll($event)">
            </label>
            <label v-for="col in cols" :key="col">
                {{ col }}
                <input 
                    type="checkbox"
                    v-bind:value="col"
                    v-on:change="addCol($event)"
                >
            </label>
        </div>
    </div>
</template>

<script>
export default {
    props: ["cols"],
    data() {
        return {
            dropdownStyle: {
                "display": "none"
            }
        }
    },
    methods: {
        showDropdown() {
            this.dropdownStyle.display = this.dropdownStyle.display == "none" ? "block" : "none";
        },
        addCol(event) {
            this.$emit("add-col", event.target.value);
        },
        addAll(event) {
            if(event.target.checked) {
                this.cols.forEach(col => {
                    const input = this.$el.querySelector(`input[value='${col}']`);
                    if(!input.checked) {
                        this.$el.querySelector(`input[value='${col}']`).checked = true;
                        this.$emit("add-col", col);
                    }
                });
            } else {
                this.cols.forEach(col => {
                    const input = this.$el.querySelector(`input[value='${col}']`);
                    if(input.checked) {
                        this.$el.querySelector(`input[value='${col}']`).checked = false;
                        this.$emit("add-col", col);
                    }
                });
            }
        }
    }
}
</script>

<style scoped>
.dropdown {
    position: relative;
}

.columns {
    position: absolute;
    background: white;
    height: 500px;
    overflow-x: hidden;
    overflow-y: scroll;

    border: 1px solid #ccc;
    border-radius: 8px;
}

.columns::-webkit-scrollbar {
  width: 8px;
}

.columns::-webkit-scrollbar-track {
  background: transparent;
}

.columns::-webkit-scrollbar-thumb {
  background-color: #ccc;
  border-radius: 20px;

  transition: background 100ms;
}

.columns::-webkit-scrollbar-thumb:hover {
    background-color: #bbb;
}

.columns label {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 200px;
    padding: 15px 20px;
}

button {
    width: 150px;

    padding: 5px 10px;

    border: 1px solid #eaeaea;
    border-radius: 20px;

    cursor: pointer;

    display: flex;
    justify-content: space-between;
    align-items: center;

    background: transparent;
    outline: none;

    transition: background 100ms;
}

button:hover {
    background: #eee;
}

input:checked ~ .checkmark {
  background-color: #2196F3;
}
</style>