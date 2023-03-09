<script setup>
defineProps({
  data: {
    type: Array
  }
})
</script>

<script>
export default {
  methods: {
    deletar(id) {
      let produtosAtualizado = this.data.filter((item) => {
        return item.id != id;
      })

      localStorage.setItem('produtos', JSON.stringify(produtosAtualizado));

      this.$emit('deletar', produtosAtualizado);
    },
    editar(id) {
      let produto_editado = this.data.filter((item) => {
        return item.id == id;
      });

      let produto_editado_string = JSON.stringify(produto_editado);
      let produto_editado_array = JSON.parse(produto_editado_string);

      this.$emit('editar', produto_editado_array);
    }
  }
}
</script>

<template>
  <table class="table">
    <thead>
      <th align="center">id</th>
      <th align="center">nome</th>
      <th align="center">descricao</th>
      <th align="center">quantidade</th>
      <th align="center">valor</th>
      <th align="center" colspan="2">Ações</th>
    </thead>
    <tbody>
      <tr v-for="d in data" :key="d">
        <td align="center">{{ d.id }}</td>
        <td align="center">{{ d.nome }}</td>
        <td align="center">{{ d.descricao }}</td>
        <td align="center">{{ d.qtd }}</td>
        <td align="center">{{ d.valor }}</td>
        <td align="center">
          <button @click="deletar(d.id)" class="delete">deletar</button>
          <button @click="editar(d.id)" class="edit">editar</button>
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
}

.edit {
  background: blue;
  color: white;
  border: none;
  cursor: pointer;
  margin-left: 5px;
  padding: 2px 10px;
}

@media (max-width: 1024px) {
  table.table {
    border: none;
  }
}
</style>
