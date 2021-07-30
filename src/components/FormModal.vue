<template>
    <div 
        class="modal-container" 
        id="modal"
        :style="styles"
        @click.stop="outOfModal($event)"
    >
        <div class="form-container">
            <header>
                <h3>Adicionar registro</h3>
                <button id="close-button" @click="hide">
                    <img src="@/assets/icons/close.svg" alt="Fechar modal" />
                </button>
            </header>
            <form>
                <input  v-model="row['Registro ANS']"        placeholder='Registro ANS'        id="ans" />
                <input  v-model="row['CNPJ']"                placeholder='CNPJ'                id="cnpj"/>
                <input  v-model="row['Razão Social']"        placeholder='Razão Social'        id="razsocial"      style="width: 100%;"/>
                <input  v-model="row['Nome Fantasia']"       placeholder='Nome Fantasia'       id="nomefantasia"   style="width: 100%;" />
                <input  v-model="row['Modalidade' ]"         placeholder='Modalidade'          id="modalidade" />
                <input  v-model="row['CEP']"                 placeholder='CEP'                 id="cep"            @blur="getLocate($event)" />
                <input  v-model="row['UF']"                  placeholder='UF'                  id="uf"             style="width: 90px;" />
                <input  v-model="row['Cidade']"              placeholder='Cidade'              id="cidade"         style="width: 185px;" />
                <input  v-model="row['Bairro']"              placeholder='Bairro'              id="bairro"         style="width: 185px;" />
                <input  v-model="row['Logradouro']"          placeholder='Logradouro'          id="logradouro"     style="width: 100%;" />
                <input  v-model="row['Número']"              placeholder='nº'                  id="numero"         style="width: 90px;"    type="number" />
                <input  v-model="row['Complemento']"         placeholder='Complemento'         id="complemento"    style="width: 380px;"/>
                <input  v-model="row['DDD']"                 placeholder='DDD'                 id="ddd"            style="width: 90px;"    type="number" />
                <input  v-model="row['Telefone']"            placeholder='Telefone'            id="telefone"       style="width: 185px;"   type="tel" />
                <input  v-model="row['Fax']"                 placeholder='Fax'                 id="fax"            style="width: 185px;" />
                <input  v-model="row['Endereço eletrônico']" placeholder='Endereço eletrônico' id="email"          style="width: 100%;"    type="email" />
                <input  v-model="row['Representante']"       placeholder='Representante'       id="representante"  style="width: 100%;" />
                <input  v-model="row['Cargo Representante']" placeholder='Cargo Representante' id="carrepresentante" />
                <input  v-model="row['Data Registro ANS']"   placeholder='Data Registro ANS'   id="dataregistro"                           type="date" />
            </form> 
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
    props: {
        showed: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            row: {
                'Registro ANS': "",
                'CNPJ': "",
                'Razão Social': "",
                'Nome Fantasia': "",
                'Modalidade': "",
                'CEP': "",
                'UF': "",
                'Cidade': "",
                'Bairro': "",
                'Logradouro': "",
                'Número': "",
                'Complemento': "",
                'DDD': "",
                'Telefone': "",
                'Fax': "",
                'Endereço eletrônico': "",
                'Representante': "",
                'Cargo Representante': "",
                'Data Registro ANS': "",
            },
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
            
            this.$el.querySelector("#ans").style.border = "1px solid #ddd";
            this.$el.querySelector("#cnpj").style.border = "1px solid #ddd";

            Object.keys(this.row).forEach(key => this.row[key] = "");
        },
        getLocate(event) {
            axios.get(`http://viacep.com.br/ws/${event.target.value}/json/unicode/`).then(res => {
                const { bairro, complemento, localidade, logradouro, uf } = res.data;

                this.row["Bairro"]      = bairro;
                this.row["Complemento"] = complemento;
                this.row["Cidade"]      = localidade;
                this.row["UF"]          = uf;
                this.row["Logradouro"]  = logradouro;
            });
        },
        sendData() {
            if(this.row["Registro ANS"].trim() == "" || this.row["CNPJ"].trim() == "") {
                if(this.row["Registro ANS"].trim() == "") {
                    this.$el.querySelector("#ans").style.border = "1px solid red";
                } 
                if(this.row["CNPJ"].trim() == "") {
                    this.$el.querySelector("#cnpj").style.border = "1px solid red";
                }
                return;
            }

            axios.post("http://localhost:5000/newrow", this.row).then(() => {
                this.hide();
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
    transition: background 100ms, color 100ms;
    display: flex;
    justify-content: center;
    align-items: center;
}

footer button:hover {
    color: white;
    background: #aaa;
}

</style>