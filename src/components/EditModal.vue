<template>
    <div 
        class="modal-container" 
        id="modal"
        :style="styles"
        @click.stop="outOfModal($event)"
    >
        <div class="form-container">
            <header>
                <h3>Editar registro</h3>
                <button id="close-button" @click="hide">
                    <img src="@/assets/icons/close.svg" alt="Fechar modal" />
                </button>
            </header>
            <form>
                <input name='Razão Social'  placeholder='Razão Social'  id="razsocial"      style="width: 100%;" v-model="row['Razão Social']"/>
                <input name='Nome Fantasia' placeholder='Nome Fantasia' id="nomefantasia"   style="width: 100%;" v-model="row['Nome Fantasia']"/>
                <input name='Modalidade'    placeholder='Modalidade'    id="modalidade" v-model="row['Modalidade']"/>
                <input name='CEP'           placeholder='CEP'           id="cep" @blur="getLocate($event)"  v-model="row['CEP']"/>
                <input name='UF'            placeholder='UF'            id="uf"          style="width: 90px;"  v-model="row['UF']"/>
                <input name='Cidade'        placeholder='Cidade'        id="cidade"      style="width: 185px;"  v-model="row['Cidade']"/>
                <input name='Bairro'        placeholder='Bairro'        id="bairro"      style="width: 185px;"  v-model="row['Bairro']"/>
                <input name='Logradouro'    placeholder='Logradouro'    id="logradouro"  style="width: 100%;"  v-model="row['Logradouro']"/>
                <input name='Número'        placeholder='nº'            id="numero"      style="width: 90px;"    type="number"  v-model="row['Número']" />
                <input name='Complemento'   placeholder='Complemento'   id="complemento" style="width: 380px;" v-model="row['Complemento']"/>
                <input name='DDD'           placeholder='DDD'           id="ddd"         style="width: 90px;" type="number"  v-model="row['DDD']" />
                <input name='Telefone'      placeholder='Telefone'      id="telefone"    style="width: 185px;"   type="tel"  v-model="row['Telefone']"/>
                <input name='Fax'           placeholder='Fax'           id="fax"         style="width: 185px;"  v-model="row['Fax']"/>
                <input name='Endereço eletrônico'   placeholder='Endereço eletrônico' id="email"            style="width: 100%;"    type="email"  v-model="row['Endereço eletrônico']" />
                <input name='Representante'         placeholder='Representante'       id="representante"    style="width: 100%;"  v-model="row['Representante']"/>
                <input name='Cargo Representante'   placeholder='Cargo Representante' id="carrepresentante" v-model="row['Cargo Representante']"/><input name='Data Registro ANS' placeholder='Data Registro ANS' id="dataregistro" type="date"  v-model="row['Data Registro ANS']" />            </form> 
            <footer>
                <button @click="hide">
                    Cancelar
                </button>
                <button @click="sendData">
                    Confirmar 
                </button>
            </footer>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    props: ["showed", "currow"],
    data() {
        return {
            row: {},
            styles: {
                display: "none",
            }
        }
    },
    created() {
        this.row = this.currow;
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
            
            const inputList = this.$el.querySelectorAll("form input");

            inputList.forEach(input => input.value = "");
        },
        getLocate(event) {
            axios.get(`http://viacep.com.br/ws/${event.target.value}/json/unicode/`).then(res => {
                const { bairro, complemento, localidade, logradouro, uf } = res.data;

                this.$el.querySelector("#bairro").value         = bairro;
                this.$el.querySelector("#complemento").value    = complemento;
                this.$el.querySelector("#cidade").value         = localidade;
                this.$el.querySelector("#uf").value             = uf;
                this.$el.querySelector("#logradouro").value     = logradouro;
            });
        },
        sendData() {
            const form = {};

            form["Registro ANS"] = this.row["Registro ANS"];
            form["CNPJ"]         = this.row["CNPJ"];

            const inputList = this.$el.querySelectorAll("form input");

            inputList.forEach(input => form[input.name] = input.value);

            axios.put(`http://localhost:5000/editrow/${form["Registro ANS"]}`, form).then(res => {
                this.hide();
                if(res.data["success"]) {
                    this.$emit("update-row", { ans_record: form["Registro ANS"], row: form});
                } else {
                    alert("Algo deu errado");
                    console.log(res.data["message"]);
                }
            });
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

.form-container {
    background: #fff;
    padding: 10px 60px;
    width: 600px;
    border-radius: 4px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, .2);
}

form {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    border-top: 1px solid #ddd;
    border-bottom: 1px solid #ddd;
    padding: 10px 0;
}

input {
    height: 40px;
    width: 235px;
    padding: 10px 20px;
    border-radius: 4px;
    border: 1px solid #ddd;
    outline: none;
    margin: 10px 0;
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
    justify-content: flex-end;
    align-items: center;
}

footer button {
    width: 130px;
    height: 40px;
    margin-left: 20px;
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