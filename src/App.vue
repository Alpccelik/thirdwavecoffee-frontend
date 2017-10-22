<template>
  <div id="app">
    <div class="columns">
    <div class="column is-half is-offset-one-quarter ">
        <b-field label="Name of Coffee" min="1" maxlength="32" ><b-input v-model="name" id="name"></b-input></b-field>
        <b-field label="Region of Coffee"><b-input v-model="region" id="region"></b-input></b-field>
        <b-field label="Type of Coffee" ><b-input v-model="type" id="type" ></b-input></b-field>
        <b-field label="Method of Coffee" ><b-input v-model="method" id="method" ></b-input></b-field>
        <b-field label="Description of Coffee"min="1" maxlength="1024"><b-input v-model="description" id="description"maxlength="1024" type="textarea"></b-input>  </b-field>
        <div class="field"> <b-checkbox v-model="milk" id="milk">There is milk in this coffee</b-checkbox>  </div>
        <b-field label="Grinding size  of Coffee Beans" ><b-input  v-model="grindingType" id="grindingType"></b-input></b-field>
        <b-field label="Preperation time of Coffee in Minutes"><b-input v-model="prepTime" id="prepTime"></b-input></b-field>
        <b-field> <p class="control">  <button class="button is-primary" v-on:click.prevent="sendata" > Send Coffee Recipes</button></p></b-field>
        </div>
        </div>
         <div class="columns" >
            <div class="column is-one-third" v-for="coffee in coffees">
                <div class="card">
                  <div class="card-content">
                    <div class="media">
                      <div class="media-content">
                        <p class="title is-4">{{coffee.name}}</p>
                        <p class="subtitle is-5">Region: {{coffee.region}} </br>
                         Type:  {{coffee.name}} </br>
                         With Milk: {{coffee.milk}} </br>
                         Grinding Type: {{coffee.grindingType}} </br>
                         Preperation Time : {{coffee.prepTime}} Minutes </br>
                        <p class="content is-4">Description: {{coffee.description}}</p>
                        
                      </div>
                    </div>
                  </div>
            </div>
            </div>
      </div>

  </div>
</template>

<script>
export default {
  name: 'app',
  created:function() {
    this.loadData();
  },
  methods: {
    loadData:function()
    {
      const vm = this;
      vm.$http.get("http://localhost:8080/api/coffee").then((resp) => {
        vm.coffees = resp.body;
      })
    },
  sendata:function(){
    const vm = this;
    var name =this.name;
    var region=this.region;
    var type=this.type;
    var method=this.method;
    var description=this.description;
    var milk=this.milk;
    var grindingType=this.grindingType;
    var prepTime=this.prepTime;
    vm.$http.post("http://localhost:8080/api/coffee",{name:name,region:region,type:type,method:method,description:description,milk:milk,grindingType:grindingType,prepTime:prepTime}).
    then(function (resp){
      this.loadData();
    },function(resp){});

  
  }},
  data() {
    return {
      name:'',
      region:'',
      type:'',
      method:'',
      description:'',
      milk:false,
      grindingType:'',
      prepTime:'',
      coffees: []
    }
    
  }
}
</script>