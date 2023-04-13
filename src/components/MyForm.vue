<template>
  <form @submit.prevent="">
    <div class="wrapper">
      <div class="wrapper__field">
        <label for="">CEP</label>
        <input ref="pipe" v-model="inputData" autocomplete="off" :maxlength="maximumInputSize" type="text" />
      </div>

      <div class="wrapper__field">
        <label for="">Cidade</label>
        <input type="text" :disabled="fullAddress.city" v-model="fullAddress.city" />
      </div>
    </div>

    <div class="wrapper">
      <div class="wrapper__field">
        <label for="">Endereço</label>
        <input type="text" :disabled="fullAddress.address" v-model="fullAddress.address" />
      </div>
    </div>

    <div class="wrapper">
      <div class="wrapper__field">
        <label for="">Bairro</label>
        <input type="text" :disabled="fullAddress.neighborhood" v-model="fullAddress.neighborhood" />
      </div>

      <div class="wrapper__field">
        <label for="">Estado</label>
        <input type="text" :disabled="fullAddress.state" v-model="fullAddress.state">
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
      isVisible: false
    }
  },

  watch: {
    async inputData(value) {
        this.formatInputCep(value)

        if(value.length === this.maximumInputSize) {
        const response = await axios.get(`https://viacep.com.br/ws/${value}/json/`)
      
        this.data = response.data
      
        const data = response.data
      
        this.fullAddress = {
          city: data.localidade,
          address: data.logradouro,
          neighborhood: data.bairro,
          state: data.uf
        }
      }
    }
  },

  mounted() {
    return this.fieldFocused()
  },

  methods: {
    formatInputCep(cep) {
      cep.length === 5 ? this.inputData += '-' : '' 
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
}

.wrapper__buttons {
  display: flex;
  flex-direction: row;
  gap: 0 24px;
  justify-content: center;
  flex-wrap: wrap;
}
</style>