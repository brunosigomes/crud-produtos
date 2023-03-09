<script setup>
import Titulo from './components/Titulo.vue'
import Produtos from './components/Produtos.vue'
</script>

<script>
export default {
  data() {
    return {
      produtos:
        localStorage.getItem('produtos') != null
          ? JSON.parse(localStorage.getItem('produtos'))
          : [],
      id: localStorage.getItem('last_id') != null ? Number(localStorage.getItem('last_id')) + 1 : 1,
      nome: '',
      descricao: '',
      qtd: '',
      valor: '',
      idx: ''
    }
  },
  methods: {
    criar() {
      if (
        this.isEmpty(this.nome) ||
        this.isEmpty(this.descricao) ||
        this.isEmpty(this.qtd) ||
        this.isEmpty(this.valor)
      ) {
        alert('Preencha todos os campos!')
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
      }
    },
    deletar(novoValor) {
      this.produtos = novoValor
    },
    editar(novoValor) {
      novoValor.forEach((element) => {
        this.id = element.id
        this.nome = element.nome
        this.descricao = element.descricao
        this.qtd = element.qtd
        this.valor = element.valor
      })

      document.querySelector('.table').style.display = 'none'
      document.querySelector('#create').style.display = 'none'
      document.querySelector('#edit').style.display = 'block'
    },
    salvar() {
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

      document.querySelector('.table').style.display = 'inline-table'
      document.querySelector('#create').style.display = 'block'
      document.querySelector('#edit').style.display = 'none'
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
      <button @click="criar" id="create">Criar</button>
      <button @click="salvar" id="edit">Salvar</button>
    </div>

    <main>
      <Produtos :data="produtos" @deletar="deletar" @editar="editar" />
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

button {
  display: block;
  margin: 20px auto;
  height: 50px;
  width: 100%;
  max-width: 300px;
  border: none;
  border-radius: 10px;
  background: #00bd7e;
  color: white;
  cursor: pointer;
}

button#edit {
  display: none;
  margin: 20px auto;
  height: 50px;
  width: 100%;
  max-width: 300px;
  border: none;
  border-radius: 10px;
  background: blue;
  color: white;
  cursor: pointer;
}

@media (max-width: 1024px) {
  main {
    overflow: auto;
  }
}
</style>
