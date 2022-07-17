<template>
  <main>
    <section class="main-content">
      
      <div class="container">
        <b-container class="bv-example-row">
          <b-row>
            <b-col cols="8">
              <h1 class="current-page_name">Animals list</h1>
            </b-col>
            <b-col cols="4" class="action-button">
              <b-button variant="dark" @click="updateAnimalsList">Update List</b-button>
              <b-button variant="primary" @click="openModalAddAnimal">+ Add Animal</b-button>
            </b-col>
          </b-row>
        </b-container>
        <div v-if="hasAnimals">
            <div>
                <b-table striped hover :items="animals" :fields="fields" caption-top>
                  <template #table-caption>This is a list of animals.</template>
                  <template v-slot:cell(actions)="{ item }">
                    <b-button @click="openModalEditAnimal(item)" variant="primary">Edit</b-button>
                    <b-button @click="openModalDeleteAnimal(item)" variant="danger">Delete</b-button>
                  </template>
                </b-table>
            </div>
        </div>
        <div v-else>
          <h5>Animals list is empty!</h5>
        </div>
      </div>

      <b-modal id="modal-delete-animal" v-model="modalDeleteIAnimalShow" title="Delete" @ok="confirmDeleteAnimal">
        <div class="d-block">Delete this animal?</div>
        <div class="d-block">Id: {{ animalToDelete.id }}</div>
        <div class="d-block">Name: {{ animalToDelete.name }}</div>
        <div class="d-block">Description: {{ animalToDelete.description }}</div>
      </b-modal>

      <b-modal id="modal-edit-animal" v-model="modalEditAnimalShow" title="Edit" @ok="confirmEditAnimal" @hidden="clearEditAnimal">
        <div class="submit-form">
          <div class="d-block">Id: {{ animalToEdit.id }}</div>
          <div class="form-group">
            <label for="name">Name</label>
            <input type="text" class="form-control" id="name" required v-model="animalToEdit.name" name="name" placeholder="Name"/>
            <span class="text-danger" v-if="formValidation.nameError">
              {{ formValidation.nameError }}
            </span>
          </div>
          <div class="form-group">
            <label for="description">Description</label>
            <input type="text" class="form-control" id="description" required v-model="animalToEdit.description" name="description" placeholder="Description"/>
            <span class="text-danger" v-if="formValidation.descriptionError">
              {{ formValidation.descriptionError }}
            </span>
          </div>
        </div>
      </b-modal>

      <b-modal id="modal-add-animal" v-model="modalAddAnimalShow" title="Add" @ok="createAnimal">
        <div class="submit-form">
          <div class="form-group">
            <label for="name">Name</label>
            <input type="text" class="form-control" id="name" required v-model="animal.name" name="name" placeholder="Name"/>
            <span class="text-danger" v-if="formValidation.nameError">
              {{ formValidation.nameError }}
            </span>
          </div>
          <div class="form-group">
            <label for="description">Description</label>
            <input type="text" class="form-control" id="description" required v-model="animal.description" name="description" placeholder="Description"/>
            <span class="text-danger" v-if="formValidation.descriptionError">
              {{ formValidation.descriptionError }}
            </span>
          </div>
        </div>
      </b-modal>

    </section>
  </main>
</template>

<script>
import { ref } from "vue";
import { directus } from "@/services/directus";
import { isArray } from "@vue/shared";

const fields = ref(null);
const animals = ref(null);
const hasAnimals = ref(false);
const modalDeleteIAnimalShow = ref(false);
const modalEditAnimalShow = ref(false);
const animalToDelete = ref({ name: "", description: "" });
const animalToEdit = ref({ name: "", description: "" });
const formValidation = ref({ nameError: "", descriptionError: ""});
const animal = ref({ name: "", description: "" });
const modalAddAnimalShow = ref(false);

export default {
  name: "#animals",
  setup() {
    //reactive fields
    fields.value = [{}];
    animals.value = [{}];
    hasAnimals.value = false;
    modalDeleteIAnimalShow.value = false;
    modalEditAnimalShow.value = false;
    animalToDelete.value = { name: "", description: "" };
    animalToEdit.value = { name: "", description: "" };
    formValidation.value = { nameError: "", descriptionError: ""};
    animal.value = { name: "", description: "" };
    modalAddAnimalShow.value = false;

    //reactive fields
    return { fields, animals, hasAnimals, modalDeleteIAnimalShow, modalEditAnimalShow, animalToDelete, animalToEdit, formValidation, animal, modalAddAnimalShow };
  },
  data() {
    return {};
  },
  created: function(){
    this.fetchData();
  },
  methods: {
    async fetchData() {
      const response = await directus.items("animals").readByQuery({
        fields: ['id', 'name', 'description']
      });

      fields.value = ['id', 'name', 'description', 'actions'];
      animals.value = response.data;
      
      if (isArray(animals.value) && animals.value.length > 0) {
        hasAnimals.value = true;
      }
    },
    async deleteAnimal(animal) {
      console.log('async deleteAnimal ' + JSON.stringify(animal));
      
      await directus.items("animals").deleteOne(animal.id);

      console.log('successfuly deleted animal: ' + JSON.stringify(animal));

      this.fetchData();
    },
    async updateAnimal(animal) {
      console.log('async updateAnimal ' + JSON.stringify(animal));

      let id = animal.id;
      await directus.items("animals").updateOne(id, animal);

      console.log('successfuly updateAnimal animal: ' + JSON.stringify(animal));

      this.fetchData();
    },
    async createAnimal() {
      console.log('async createAnimal ' + JSON.stringify(animal.value));

      formValidation.value = this.clearFormValidation();

      if (this.checkFormIsValid(animal)) {
        const createdAnimal = await directus.items("animals").createOne(animal.value);
        console.log('successfuly created animal: ' + JSON.stringify(createdAnimal));
        
        modalAddAnimalShow.value = false;
        animal.value = this.clearAnimal();
        formValidation.value = this.clearFormValidation();
        this.fetchData();
        
      } else {
        console.log("Check required fields!!");
      }
    }, 
    updateAnimalsList() {
      this.fetchData();
    },
    openModalDeleteAnimal(animal){
      modalDeleteIAnimalShow.value = true;
      animalToDelete.value = animal;
    },
    confirmDeleteAnimal() {
      console.log('confirmDeleteAnimal ' + JSON.stringify(animalToDelete.value));
      this.deleteAnimal(animalToDelete.value);
      modalDeleteIAnimalShow.value = false;
      animalToDelete.value = {};
    },
    openModalEditAnimal(animal){
      modalEditAnimalShow.value = true;
      animalToEdit.value = animal;
    },
    confirmEditAnimal() {
      console.log('confirmEditAnimal ' + JSON.stringify(animalToEdit.value));
      formValidation.value = this.clearFormValidation();

      if (this.checkFormIsValid(animalToEdit)) {
        this.updateAnimal(animalToEdit.value);
        modalEditAnimalShow.value = false;
        animalToEdit.value = {};
        formValidation.value = this.clearFormValidation();
        
      } else {
        console.log("Check required fields!!");
      }
    },
    checkFormIsValid(animalToCheck) {
      if ((!animalToCheck.value.name || animalToCheck.value.name === "") 
          && (!animalToCheck.value.description || animalToCheck.value.description === "")) {
        formValidation.value = { nameError: "Name is requered", descriptionError: "Description is required"}
        return false;
      }

      if (!animalToCheck.value.name || animalToCheck.value.name === "") {
        formValidation.value = { nameError: "Name is requered", descriptionError: ""};
        return false;
      }

      if (!animalToCheck.value.description || animalToCheck.value.description === "") {
        formValidation.value = { nameError: "", descriptionError: "Description is required"}
        return false;
      }

      return true;
    },
    clearFormValidation() {
      console.log('clearFormValidation');
      return { nameError: "", descriptionError: ""};
    },
    clearEditAnimal() {
      console.log('clearEditAnimal');
      animalToEdit.value = {};
      this.fetchData();
    },
    openModalAddAnimal(){
      modalAddAnimalShow.value = true;
      animal.value = this.clearAnimal();
      formValidation.value = this.clearFormValidation();
    },
    clearAnimal() {
      console.log('clearAnimal');
      return { name: "", description: "" };
    }  
  }
};
</script>

<style>
.action-button {
  text-align: right;
}
</style>