<template>
  <div class="container is-fluid" id="app">
    <div class="columns">
      <div class="column is-half is-offset-one-quarter ">
        <form @submit.prevent="validateBeforeSubmit">
          <b-field label="Name of Coffee">
           <p class="control has-icon has-icon-right">
            <input v-model="name" id="name" v-validate="'required|alpha'" :class="{'input': true, 'is-danger': errors.has('name') }" name="name" type="text" placeholder="name" ></b-input>
            <span v-show="errors.has('name')" class="help is-danger">{{ errors.first('name') }}</span>
                </p>
          </b-field>
          <b-field label="Region of Coffee">
            <p class="control has-icon has-icon-right">
            <input v-model="region" id="region" v-validate="'required|alpha'" :class="{'input': true, 'is-danger': errors.has('region') }" name="region" type="text" placeholder="region" ></b-input>
            <span v-show="errors.has('region')" class="help is-danger">{{ errors.first('region') }}</span>
                </p>
          </b-field>
          <b-field label="Type of Coffee">
            <p class="control has-icon has-icon-right">
            <input v-model="type" id="type" v-validate="'required|alpha'" :class="{'input': true, 'is-danger': errors.has('type') }" name="type" type="text" placeholder="type" ></b-input>
            <span v-show="errors.has('type')" class="help is-danger">{{ errors.first('type') }}</span>
                </p>
          </b-field>
          <b-field label="Method of Coffee">
            <p class="control has-icon has-icon-right">
            <input v-model="method" id="method" v-validate="'required|alpha'" :class="{'input': true, 'is-danger': errors.has('method') }" name="method" type="text" placeholder="method" ></b-input>
            <span v-show="errors.has('method')" class="help is-danger">{{ errors.first('method') }}</span>
                </p>
          </b-field>
          <b-field label="Description of Coffee">
            <p class="control has-icon has-icon-right">
            <textarea v-model="description" id="description"  type="textarea"  maxlength="1024"  v-validate="'required|alpha'" :class="{'textarea is-primary': true, 'is-danger': errors.has('description') }" name="description" placeholder="description" ></textarea>
            <span v-show="errors.has('description')" class="help is-danger">{{ errors.first('description') }}</span>
                </p>
          </b-field>
          <div class="field">
            <b-checkbox v-model="milk" id="milk">There is milk in this coffee</b-checkbox>
          </div>
          <b-field label="Grinding size  of Coffee Beans">
            <b-input v-model="grindingType" id="grindingType" type="number"></b-input>
          </b-field>
          <b-field label="Preperation time of Coffee in Minutes">
            <b-input v-model="prepTime" id="prepTime" type="number" min="1"></b-input>
          </b-field>
          <b-field>
            <p class="control">
              <button class="button is-primary" type="submit" v-on:click.prevent="sendata"> Send Coffee Recipes</button>
            </p>
          </b-field>
        </form>
      </div>
    </div>
    <div class="columns is-multiline">
      <div class="column is-one-third" v-for="coffee in reverseItems">
        <div class="card">
          <div class="card-content">
            <div class="media">
              <div class="media-content">
                <p class="title is-3">{{coffee.name}}<hr class="dropdown-divider">
                  Region:{{coffee.region}}<hr class="dropdown-divider">
                  Type: {{coffee.type}}<hr class="dropdown-divider">
                  With Milk: {{coffee.milk}}<hr class="dropdown-divider">
                  Grinding Type: {{coffee.grindingType}}<hr class="dropdown-divider">
                  Preperation Time : {{coffee.prepTime}} Minutes<hr class="dropdown-divider">
                  Description: {{coffee.description}}</p>
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
  created: function() {
    this.loadData();
  },
  methods: {

    loadData: function() {
      const vm = this;
      vm.$http.get("http://localhost:8080/api/coffee").then((resp) => {
        vm.coffees = resp.body;
      })
    },
    sendata: function() {
       this.$validator.validateAll().then((result) => {
        if (result) {
          const vm = this;
      var name = this.name;
      var region = this.region;
      var type = this.type;
      var method = this.method;
      var description = this.description;
      var milk = this.milk;
      var grindingType = this.grindingType;
      var prepTime = this.prepTime;
      vm.$http.post("http://localhost:8080/api/coffee", { name: name, region: region, type: type, method: method, description: description, milk: milk, grindingType: grindingType, prepTime: prepTime }).
        then(function(resp) {
          this.loadData();
        }, function(resp) { });

          // eslint-disable-next-line
          alert('From Submitted!');
          return;
        }
        alert('Correct them errors!');
      });
      


    }
  },
  data() {
    return {
      name: '',
      region: '',
      type: '',
      method: '',
      description: '',
      milk: false,
      grindingType: '',
      prepTime: '',
      coffees: []
    }

  },
  computed: {
    reverseItems() {
      return this.coffees.slice().reverse();
    }
  }
}

</script>