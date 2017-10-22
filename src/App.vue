<template>
  <div id="app">
    <div class="container is-desktop">
        <b-field label="Name of Coffee"><b-input v-model="name" id="name"></b-input></b-field>
        <b-field label="Region of Coffee"><b-input v-model="region" id="region"></b-input></b-field>
        <b-field label="Type of Coffee"><b-input v-model="type" id="type" ></b-input></b-field>
        <b-field label="Method of Coffee"><b-input v-model="method" id="method" ></b-input></b-field>
        <b-field label="Description of Coffee"><b-input v-model="description" id="description"maxlength="200" type="textarea"></b-input>  </b-field>
        <div class="field"> <b-checkbox v-model="milk" id="milk">There is milk in this coffee</b-checkbox>  </div>
        <b-field label="Grinding size  of Coffee Beans"><b-input  v-model="grindingType" id="grindingType"></b-input></b-field>
        <b-field label="Preperation time of Coffee"><b-input v-model="prepTime" id="prepTime"></b-input></b-field>
        <b-field> <p class="control">  <button class="button is-primary" v-on:click.prevent="sendata" > Send Coffee Recipes</button></p></b-field>
        </div>
    <table>
      <tr>
        <th>ID</th>
        <th>Name</th>
      </tr>
      <tr v-for="coffee in coffees"> 
        <td>{{coffee.id}}</td>
        <td>{{coffee.name}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'app',
   methods: {
  created:function() {
    const vm = this;
    vm.$http.get("http://localhost:8080/api/coffee")
      .then((resp) => {
        vm.coffees = resp.body;
      })
  },
  sendata:function(){
    const vm = this;
    var name =this.coffee.name;
    var region=this.region;
    var type=this.type;
    var method=this.method;
    var description=this.description;
    var milk=this.milk;
    var grindingType=this.grindingType;
    var prepTime=this.prepTime;
    vm.$http.post("http://localhost:8080/api/coffee",{name:name,region:region,type:type,method:method,description:description,milk:milk,grindingType:grindingType,prepTime:prepTime}).
    then(function (resp){
    },function(resp){});

  }},
  data() {
    return {
      coffees: [],
      coffee:{
      name:'',
      region:'',
      type:'',
      method:'',
      description:'',
      milk:false,
      grindingType:'',
      prepTime:''}
    }
  }
}
</script>