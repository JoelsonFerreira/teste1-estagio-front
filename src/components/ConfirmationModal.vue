<template>
    <div 
        class="modal-container" 
        id="modal"
        :style="styles"
        @click.stop="outOfModal($event)"
    >
        <div class="conf-container">
            <header>
                <h3>Tem certeza que deseja deletar o registro?</h3>
            </header>
                       
            <footer>
                <button @click="hide">
                    Cancelar
                </button>
                <button @click="confirm">
                    Confirmar 
                </button>
            </footer>
        </div>
    </div>
</template>

<script>
export default {
    props: ["showed"],
    data() {
        return {
            styles: {
                display: "none",
            }
        }
    },
    watch: {
        showed(isShowed) {
            this.styles.display = isShowed ? "flex" : "none"
        }
    },
    methods: {
        outOfModal(event) {
            if(event.target.id == "modal") {
                this.hide()
            }
        },
        hide() {
            this.$emit("hide-modal");  
        },
        confirm() {
            this.$emit('delete-row');
            this.hide();
        }
    }
}
</script>

<style scoped>
.modal-container {
    height: 100vh;
    width: 100vw;
    position: absolute;
    top: 0;
    left: 0;
    background: rgba(0, 0, 0, .3);
    justify-content: center;
    align-items: center;
}

.conf-container {
    background: #fff;
    padding: 30px 60px;
    border-radius: 4px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, .2);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 20px 0;
}

header #close-button {
    background: #f3f3f3;
    display: flex;
    justify-content: center;
    align-items: center;
    border: none;
    outline: none;
    margin-left: auto;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    cursor: pointer;
    transition: background 100ms;
}

header #close-button:hover {
    background: #ddd;
}

footer {
    margin: 20px 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

footer button {
    width: 130px;
    height: 40px;
    cursor: pointer;
    border: none;
    background: #ddd;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: background 100ms, color 100ms;
}

footer button:hover {
    color: white;
    background: #aaa;
}

</style>