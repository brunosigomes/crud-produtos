<script>
export default {
  methods: {
    deletar(id) {
      let produtosAtualizado = this.produtos.filter((item) => {
        return item.id != id
      })

      localStorage.setItem('produtos', JSON.stringify(produtosAtualizado))

      this.$emit('deletar', produtosAtualizado)
    },
    editar(id) {
      let produto_editado = this.produtos.filter((item) => {
        return item.id == id
      })

      let produto_editado_string = JSON.stringify(produto_editado)
      let produto_editado_array = JSON.parse(produto_editado_string)

      this.$emit('editar', produto_editado_array)
    }
  },
  props: {
    produtos: Array
  }
}
</script>

<template>
  <table class="table" v-if="produtos.length > 0">
    <thead>
      <th align="center">id</th>
      <th align="center">nome</th>
      <th align="center">descricao</th>
      <th align="center">quantidade</th>
      <th align="center">valor</th>
      <th align="center" colspan="2">Ações</th>
    </thead>
    <tbody>
      <tr v-for="produto in produtos" :key="produto.id">
        <td align="center">{{ produto.id }}</td>
        <td align="center">{{ produto.nome }}</td>
        <td align="center">{{ produto.descricao }}</td>
        <td align="center">{{ produto.qtd }}</td>
        <td align="center">{{ produto.valor }}</td>
        <td align="center">
          <button @click="deletar(produto.id)" class="delete">deletar</button>
          <button @click="editar(produto.id)" class="edit">editar</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
table.table {
  border: 1px solid;
  margin-top: 30px;
  padding: 20px;
  width: 800px;
}

th {
  border: 1px solid;
  background: #fafafa;
  color: black;
  padding: 10px 30px;
}

td {
  color: white;
}

h3 {
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 0.4rem;
  color: var(--color-heading);
}

.delete {
  background: red;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.edit {
  background: blue;
  color: white;
  border: none;
  cursor: pointer;
  margin-left: 5px;
  padding: 2px 10px;
  border-radius: 5px;
}

@media (max-width: 1024px) {
  table.table {
    border: none;
  }
}
</style>
