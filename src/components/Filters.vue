<template>
    <div class="filter-container">
        <div
            class="filter" 
            v-for="(filter, index) in filters" 
            :key="index"
        >
            {{ Object.keys(filter)[0] }}: {{ filter[Object.keys(filter)[0]] }}
            <button @click="$emit('remove-filter', filter)">
                <img 
                    src="@/assets/icons/close.svg" 
                    alt="Buscar" 
                />
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props: ["filters"]
}
</script>

<style>

.filter-container {
    display: flex;
    flex-wrap: wrap;
}

.filter {
    background: #F1F1F1;
    border-radius: 8px;
    padding: 3px 10px;
    margin: 10px 10px 0 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

button {
    border: none;
    outline: none;
    display: flex;
    align-items: center;
    cursor: pointer;
}
</style>