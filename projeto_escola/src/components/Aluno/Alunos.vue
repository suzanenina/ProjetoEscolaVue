<template>
  <div>
    <titulo texto="Aluno" />

    <input
      type="text"
      placeholder="Nome do Aluno"
      v-model="nome"
      @keyup.enter="addAluno()"
    />
    <button class="btn btnInput" @click="addAluno()">Adicionar</button>
    <table>
      <thead>
        <th>Mat.</th>
        <th>Nome</th>
        <th>Opções</th>
      </thead>
      <tbody v-if="alunos.length">
        <tr v-for="(aluno, index) in alunos" :key="index">
          <td>{{ aluno.id }}</td>
          <td>{{ aluno.nome }} {{ aluno.sobrenome }}</td>
          <td>
            <button class="btn btn_danger" @click="remover(aluno)">
              Remover
            </button>
          </td>
        </tr>
      </tbody>
      <tfoot v-else>
        Nenhum aluno encontrado
      </tfoot>
    </table>
  </div>
</template>

<script>
import Titulo from "../_share/Titulo";

export default {
  components: {
    Titulo,
  },
  data() {
    return {
      titulo: "Aluno",
      professorId: this.$route.params.prof_id,
      nome: "",
      alunos: [],
    };
  },
  created() {
    if (this.professorId) {
      this.$http
        .get("http://localhost:3000/alunos?professor.id=" + this.professorId)
        .then((res) => res.json())
        .then((alunos) => (this.alunos = alunos));
    } 
    else {
      this.$http
        .get("http://localhost:3000/alunos")
        .then((res) => res.json())
        .then((alunos) => (this.alunos = alunos));
    }
  },
  props: {},
  methods: {
    addAluno() {
      let _aluno = {
        nome: this.nome,
        sobrenome: "",
      };

      this.$http
        .post("http://localhost:3000/alunos", _aluno)
        .then((res) => res.json())
        .then((alunoRetornado) => this.alunos.push(alunoRetornado));
      this.nome = "";
    },
    remover(aluno) {
      this.$http.delete(`http://localhost:3000/alunos/${aluno.id}`).then(() => {
        let indice = this.alunos.indexOf(aluno);
        this.alunos.splice(indice, 1);
      });
    },
  },
};
</script>

<style scoped>
input {
  width: calc(100% - 195px);
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  color: #687f7f;
  display: inline;
}
.btnInput {
  background-color: green;
  border: 0px;
  padding: 20px;
  font-size: 1.3em;
  width: 150px;
}
.btnInput:hover {
  border: 0px;
}
</style>
