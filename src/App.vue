<template>
<div class="conteudo">
  <div class="banner">
    <div class="row">
      <div class="col-sm-02 logo">
        <v-img :src="require('./assets/logo.png')" />
      </div>
      <div class="col-sm-10">
        <strong> <h2 class="titulo-banner">Cadastro de Imoveis Rurais </h2> </strong>
      </div>  
    </div>
    <div class="menu">
       <v-btn v-on:click="cadastro= true" class="mx-2" fab dark color="primary">
       <v-icon dark>mdi-plus</v-icon>
    </v-btn>
    </div>
  </div>

  <!-- Cadastro de propriedades-->
  <div class="cadastro" v-if="this.cadastro">
  <div class="cadastro-titulo">
    Cadastro de Propriedades
  </div>

  <v-form ref="form" v-model="valid" lazy-validation>
    <v-text-field
      v-model="nome"
      :rules="nomeRules"
      :counter="100"
      label="Nome"
      required
    ></v-text-field>
    <v-text-field
      v-model="proprietario"
      :rules="proprietarioRules"
      :counter="100"
      label="Proprietario"
      required
    ></v-text-field>
    <v-text-field
      v-model="endereco"
      :rules="enderecoRules"
      :counter="250"
      label="Endereço"
      required
    ></v-text-field>
    <v-text-field
      v-model="foto"
      :rules="fotoRules"
      label="Foto"
      required
    ></v-text-field>
    <v-text-field
      v-model="latitude"
      :rules="latitudeRules"
      :counter="20"
      label="Latitude"
      required
    ></v-text-field>
    <v-text-field
      v-model="longitude"
      :rules="longitudeRules"
      :counter="20"
      label="Longitude"
      required
    ></v-text-field>
    <v-btn :disabled="!valid" @click="salvar"> Salvar  </v-btn>
    <v-btn @click="limpar">Limpar</v-btn> 
    <v-btn @click="cadastro=false">Fechar</v-btn>
  </v-form>
  </div>

  <!-- Grid propriedades-->
<div class="grid-view" v-if="!this.cadastro">
  <v-simple-table>
    <template v-slot:default>
      <thead>
        <tr>
          <th class="text-left">Nome</th>
          <th class="text-left">Proprietario</th>
          <th class="text-left">Endereco</th>
          <th class="text-left">Foto</th>
          <th class="text-left">Latitude</th>
          <th class="text-left">Longitude</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in dados" :key="item.id">
          <td>{{ item.nome }}</td>
          <td>{{ item.proprietario }}</td>
          <td>{{ item.endereco }}</td>
          <td>{{ item.foto }}</td>
          <td>{{ item.latitude }}</td>
          <td>{{ item.longitude }}</td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</div>


 </div> 
</template>
<script>
  import axios from 'axios'

  export default {
 
  data: () => ({
      cadastro: false,
      valid: true,
      nome: '',
      dados: [],
      nomeRules: [
        v => !!v || 'O Nome é origatório',
        v => (v && v.length <= 100) || 'O nome deve conter menos que 100 caracteres'
      ],
       proprietario: '',
      proprietarioRules: [
        v => !!v || 'O proprietario é origatória',
        v => (v && v.length <= 100) || 'O proprietario deve conter menos que 100 caracteres'
      ],
       endereco: '',
      enderecoRules: [
        v => !!v || 'O endereco é origatório',
        v => (v && v.length <= 250) || 'O endereco deve conter menos que 10 caracteres'
      ],
       foto: '',
      fotoRules: [
        v => !!v || 'A foto é origatória',
        v => (v && v.length <= 10) || 'A foto deve conter menos que 10 caracteres'
      ],
      latitude: '',
      latitudeRules: [
        v => !!v || 'A latitude é origatória',
        v => (v && v.length <= 10) || 'A latitude deve conter menos que 10 caracteres'
      ],
       longitude: '',
      longitudeRules: [
        v => !!v || 'A longitude é origatória',
        v => (v && v.length <= 10) || 'A longitude deve conter menos que 10 caracteres'
      ],

    }),

    methods: {
      salvar () {
        let id= 20;
        if (this.$refs.form.validate()) {
          // Native form submission is not yet supported
          // 'nome='+ this.nome + '&'+ 'propriedade='+ this.propriedade +'&' + 'endereco =' + this.endereco +'&' + 'foto =' + this.foto +'&' + 'latitude =' + this.latitude +'&' + 'longitude =' + this.longitude, 
          axios.post('http://localhost:3333/cadastro?' + 'id=' + id + '&'  + 'nome=' + this.nome + '&' + 'proprietario=' + this.proprietario + '&' + 
          'endereco=' + this.endereco + '&' + 'foto=' + this.foto + '&' + 'latitude=' + this.latitude + '&' + 'longitude=' + this.longitude
          
          )
          id += 1;  
        }
       
      },
      limpar () {
        this.$refs.form.reset()
      },
    sortBy: function (key) {
      this.sortKey = key
      this.sortOrders[key] = this.sortOrders[key] * -1
    }
  

    },
     mounted () {
      axios
      .get('http://127.0.0.1:3333/cadastro/?')
      .then(response => (this.dados = response.data))
      },
 
}

</script>
<style scoped>
.conteudo{
  margin: 0% 0% 0% 0% !important;
  padding: 0% 0% 10% 0% !important;
  background-color: rgb(230, 230, 230);
  width: 100% !important;
  height: 760px !important;
  overflow: hidden !important;
}
.cadastro{
  margin: 3% 3% 3% 3% !important;
  padding: 3% 3% 3% 3% !important;
  width: 400px !important;
  background-color: rgb(255, 255, 255);
}
.cadastro-titulo{
    font-size: 20px!important;
    font-weight: 400;
    line-height: 32px!important;
    letter-spacing: normal!important;
}
.banner{
  margin: 0% 0% 0% 0% !important;
  padding: 0% 0% 0% 0% !important;
  width: 100%;
  height: 50px;
  background-color: rgb(34, 34, 212);
}
.titulo-banner{
    margin: 0% 0% 0% 0% !important;
    padding: 0% 0% 0% 45% !important;
    font-size: 20px!important;
    font-weight: 400;
    line-height: 32px!important;
    letter-spacing: normal!important;
    color: aliceblue;
}
.logo {
  margin: 0% 0% 0% 0% !important;
  padding: 3px 0% 0% 20px !important;
  width: 40px;
  height: 40px;
}
.menu{
  margin: 1%;
  padding: 1% 0% 0% 80%;
  color: blue !important;;
}
.grid-view{
  margin: 5% 3% 3% 3% !important;
  padding: 3% 3% 3% 3% !important;
}

</style>
