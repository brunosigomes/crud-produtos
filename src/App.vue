<script setup>
import Titulo from './components/CustomTitle.vue'
import Produtos from './components/CustomProdutos.vue'
import Button from './components/CustomButton.vue'
</script>

<script>
export default {
  data() {
    return {
      produtos: this.getProdutos(),
      id: this.getLastId(),
      nome: '',
      descricao: '',
      qtd: null,
      valor: null,
      idx: '',
      showEdit: false
    }
  },
  methods: {
    // Cria novo produto
    criar() {
      if (
        this.isEmpty(this.nome) ||
        this.isEmpty(this.descricao) ||
        this.isEmpty(this.qtd) ||
        this.isEmpty(this.valor)
      ) {
        alert('Preencha todos os campos!')
      } else if (isNaN(this.qtd) || isNaN(this.valor)) {
        alert('Valor quantidade e valor tem que ser numérico!')
      } else {
        this.produtos.push({
          id: this.id,
          nome: this.nome,
          descricao: this.descricao,
          qtd: this.qtd,
          valor: this.valor
        })
        localStorage.setItem('produtos', JSON.stringify(this.produtos))
        localStorage.setItem('last_id', this.id)
        this.nome = ''
        this.descricao = ''
        this.qtd = ''
        this.valor = ''
        this.id += 1
        this.showEdit
      }
    },
    // Deleta produto
    deletar(novoValor) {
      this.produtos = novoValor
    },
    // Edita o produto
    editar(novoValor) {
      novoValor.forEach((element) => {
        this.id = element.id
        this.nome = element.nome
        this.descricao = element.descricao
        this.qtd = element.qtd
        this.valor = element.valor
      })

      this.showEdit = true
    },
    // Salva o produto editado
    salvar() {
      if (
        this.isEmpty(this.nome) ||
        this.isEmpty(this.descricao) ||
        this.isEmpty(this.qtd) ||
        this.isEmpty(this.valor)
      ) {
        alert('Preencha todos os campos!')
      } else if (isNaN(this.qtd) || isNaN(this.valor)) {
        alert('Valor quantidade e valor tem que ser numérico!')
      } else {
        this.produtos.forEach((item, index) => {
          if (item.id == this.id) {
            this.idx = index
          }
        })

        this.produtos[this.idx] = {
          id: this.id,
          nome: this.nome,
          descricao: this.descricao,
          qtd: this.qtd,
          valor: this.valor
        }

        localStorage.setItem('produtos', JSON.stringify(this.produtos))

        this.nome = ''
        this.descricao = ''
        this.qtd = ''
        this.valor = ''
        this.id += 1

        this.showEdit = false
      }
    },
    // Recupera os produtos do localStorage
    getProdutos() {
      return localStorage.getItem('produtos') != null
        ? JSON.parse(localStorage.getItem('produtos'))
        : []
    },
    // Recupera o ultimo id salvo do localStorage
    getLastId() {
      return localStorage.getItem('last_id') != null
        ? Number(localStorage.getItem('last_id')) + 1
        : 1
    },
    isEmpty(value) {
      return (
        value === undefined ||
        value === null ||
        value.trim() === '' ||
        value.trim() === '\b' ||
        (Array.isArray(value) && value.length === 0)
      )
    }
  }
}
</script>

<template>
  <div>
    <header>
      <Titulo msg="CRUD de Produtos" />
    </header>

    <div class="form">
      <div class="field">
        <input type="text" placeholder="Nome" v-model="nome" />
        <input type="text" placeholder="Descrição" v-model="descricao" />
        <input type="text" placeholder="Quantidade" v-model="qtd" />
        <input type="text" placeholder="Valor" v-model="valor" />
      </div>

      <Button texto="Criar" :style="{ background: '#00bd7e' }" @click="criar" v-if="!showEdit" />
      <Button texto="Salvar" :style="{ background: 'blue' }" @click="salvar" v-if="showEdit" />
    </div>

    <main>
      <Produtos :produtos="produtos" @deletar="deletar" @editar="editar" v-if="!showEdit" />
    </main>
  </div>
</template>

<style scoped>
.form {
  margin-top: 30px;
}

.field {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

input[type='text'] {
  height: 50px;
  border: none;
  border-radius: 10px;
  padding-left: 20px;
  outline: 0;
}

.show {
  display: block !important;
}

.hide {
  display: none !important;
}

.show-table {
  display: inline-table !important;
}

@media (max-width: 1024px) {
  main {
    overflow: auto;
  }
}
</style>
