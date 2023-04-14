<template>
  <form @submit.prevent="">
    <div class="wrapper">
      <div class="wrapper__field">
        <label for="">CEP *</label>
        <input 
          ref="pipe" 
          v-model="inputData" 
          autocomplete="off" 
          type="text"
          :class="{ 'invalid-field': invalidField }"
          :maxlength="maximumInputSize" 
        />
      </div>

      <div class="wrapper__field">
        <label for="">Cidade</label>
        <input 
          v-model="fullAddress.city"
          type="text" 
          :disabled="fullAddress.city" 
        />
      </div>
    </div>

    <div class="wrapper">
      <div class="wrapper__field">
        <label for="">Endereço</label>
        <input 
          v-model="fullAddress.address" 
          type="text" 
          :disabled="fullAddress.address" 
        />
      </div>
    </div>

    <div class="wrapper">
      <div class="wrapper__field">
        <label for="">Bairro</label>
        <input 
          v-model="fullAddress.neighborhood" 
          type="text" 
          :disabled="fullAddress.neighborhood" 
        />
      </div>

      <div class="wrapper__field">
        <label for="">Estado</label>
        <input 
          v-model="fullAddress.state"
          type="text" 
          :disabled="fullAddress.state" 
        />
      </div>
    </div>

    <div class="wrapper">
      <div class="wrapper__field">
        <label for="">Número</label>
        <input type="number" />
      </div>

      <div class="wrapper__field">
        <label for="">Complemento</label>
        <input type="text" />
      </div>
    </div>

    <div class="wrapper__buttons">
      <MyButton
        name="LIMPAR"
        @clear-all-data="wipeData"
        @blastoise="hideDataOnScreen"
      />
       <MyButton
        name="VER JSON"
        @click="showDataOnScreen"
      />
    </div>

    <MyDataView
      :displayable="isVisible" 
      :dataApi="data" 
    />
  </form>
</template>

<script>
import axios from "axios"

import MyButton from "./MyButton.vue"
import MyDataView from "./MyDataView.vue"

export default {
  name: "MyForm",
  components: {
    MyButton,
    MyDataView
  },
  
  data() {
    return {
      maximumInputSize: 9,
      inputData: '',
      data: [],
      fullAddress: {
        city: null,
        address: null,
        neighborhood: null,
        state: null,
        number: null
      },
      isVisible: false,
      invalidField: false
    }
  },

  watch: {
    async inputData(value) {
        this.formatInputCep(value)

        if(value.length === this.maximumInputSize) {
        const response = await axios.get(`https://viacep.com.br/ws/${value}/json/`)

        if (!response.data.erro) {
          this.invalidField = false
          this.data = response.data
      
          const data = response.data
      
          this.fullAddress = {
            city: data.localidade,
            address: data.logradouro,
            neighborhood: data.bairro,
            state: data.uf
          }
        } else {
          this.invalidField = true
        }       
      }
    }
  },

  mounted() {
    return this.fieldFocused()
  },

  methods: {
    formatInputCep(cep) {
      const numberText = cep.replace(/[^\d]/g, "");
      const formatedCep = numberText.replace(/^(\d{5})(\d{3})$/, "$1-$2");
      this.inputData = formatedCep
    },

    wipeData() {
      this.inputData = ''
      this.data = []
      this.fullAddress = {
        city: null,
        address: null,
        neighborhood: null,
        state: null,
        number: null
      }

      this.fieldFocused()
      this.hideDataOnScreen()
    },

    showDataOnScreen() {
      this.isVisible = true
    },

    hideDataOnScreen() {
      this.isVisible = false
    },

    fieldFocused() {
      return this.$refs.pipe.focus()
    }
  }
}
</script>

<style lang="scss">
.wrapper {
  display: flex;
  justify-content: center;
  gap: 0 24px;

  &__field {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    flex: 1;
  }

  label {
    padding: 8px 0;
  }

  input {
    padding: 0 0 0 8px;
    margin: 0;
    width: calc(100% - 8px);
    height: 44px;
    border: 1px solid #eaeaea;
    border-radius: 5px;
    outline: none;
    
    &::-webkit-outer-spin-button,
    &::-webkit-inner-spin-button {
      display: none;
      -webkit-appearance: none;
      margin: 0;
    }

    &:disabled {
      cursor: no-drop;
    }
  }

  .invalid-field {
    border: 1px solid #ff6f6f;
  }
}

.wrapper__buttons {
  display: flex;
  flex-direction: row;
  gap: 0 24px;
  justify-content: center;
  flex-wrap: wrap;
}
</style>