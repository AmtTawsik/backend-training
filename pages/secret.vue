<script setup>
import { ref } from "vue";
const {app} = useMyRealmApp();

const mongo = app.currentUser?.mongoClient("mongodb-atlas");
const collection = mongo?.db("db1").collection("todo");

// functions
const contacts = ref([])

var myHeaders = new Headers();
myHeaders.append("Authorization", `Bearer ${app.currentUser?._accessToken}`);
myHeaders.append("Content-Type", "application/json");

var graphql = JSON.stringify({
  query: "query {\n    todos {\n        _id\n        title\n      status\n       description\n    }\n}",
  variables: {}
})
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: graphql,
  redirect: 'follow'
};
const fetches = true;
watch(
  () => fetches,
  fetch("https://ap-south-1.aws.realm.mongodb.com/api/client/v2.0/app/application-0-ivwxk/graphql", requestOptions)
  .then(response => response.json())
  .then(result => {
    contacts.value = result.data.todos
    fetches = false
  })
  .catch(error => console.log('error', error))
)

console.log('good job',contacts)

const editFormData = ref({
  service: "",
  last3: "",
  next3: "",
})

const editContactId = ref(null)

const handleEditFormChange = (event) => {

  event.preventDefault();

  const fieldName = event.target.getAttribute("name");
  const fieldValue = event.target.value;

  const newFormData = { ...editFormData._rawValue };
  newFormData[fieldName] = fieldValue;

  editFormData.value = newFormData
};

const handleAddFormSubmit = (event) => {
  event.preventDefault();

  const newContact = {
    title: "",
    description: "some text",
    status: "",
    userId: app.currentUser.id,
  };

  collection
		.insertOne(newContact)
		.then((data) => {
			console.log(data);
      fetches = true
		})
		.catch((err) => {
			console.log(err);
		});

  

  // contacts.value = [ ...contacts.value ];
  // const lastElement = contacts.value[contacts.value.length - 1]
  // handleEditClick(newContact)
};

const handleEditFormSubmit = () => {

  // event.preventDefault();

  const editedContact = {
    title: editFormData.value.service,
    status: editFormData.value.last3,
    description: editFormData.value.next3,
  };

  console.log('omg',editContactId.value);
  
  collection
		.updateOne({ _id: editContactId }, { $set: editedContact })
		.then((data) => {
			console.log(data);
		})
		.catch((err) => {
			console.log(err);
		});

  

  // console.log(editedContact)

  // const newContacts = [...contacts.value];

  // const index = contacts.value.findIndex((contact) => contact._id === editContactId.value);


  // console.log('index', index)

  // newContacts[index] = editedContact;
  // console.log('newContacts', newContacts)


  // contacts.value = newContacts
  editContactId.value = null;
};

const handleEditClick = (contact) => {

  console.log('contact',contact)
  event.preventDefault();
  editContactId.value = contact._id;

  const formValues = {
    id:contact._id,
    service: contact.title,
    last3: contact.status,
    next3: contact.description,
  };


  editFormData.value = formValues;
};

const handleCancelClick = () => {
  // setEditContactId(null);
  editContactId.value = null
};

const handleDeleteClick = (id) => {

  // const index = contacts.findIndex(contact => contact._id === contactId)
  // const index = contacts.value.findIndex((contact) => contact._id === contactId);

  // contacts.value.splice(index, 1);
 
console.log(id)
  collection
		.deleteOne({
      title:id,
    })
		.then((data) => {
			console.log(data);
		})
		.catch((err) => {
			console.log(err);
		});

};



const consoleData = () =>{
  collection
		.find()
		.then((data) => {
			console.log(data)
		})
		.catch((err) => {
			console.log(err);
		});
}
</script>


<template>
  <div class="mb-10">
    <h2 class="md:text-4xl text-2xl text-center font-bold mb-5">Todo Lists</h2>
    <div class="overflow-x-auto">
      <form @submit.prevent="handleEditFormSubmit">
        <table v-if="contacts.length > 0" class="w-10/12 mx-auto text-sm text-left text-gray-500 bg-gray-50 ">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50  ">
            <tr>
              <th scope="col" class="px-4 py-3">Title</th>
              <th scope="col" class="px-4 py-3">Status</th>
              <th scope="col" class="px-4 py-3">Description</th>
              <th scope="col" class="px-4 py-3">ACTION</th>
            </tr>
          </thead>
          <tbody>
            <template v-for="contact in contacts" :key="contact._id">
              <template v-if="editContactId === contact._id">
                <EditableRow :editFormData="editFormData" :handleEditFormChange="handleEditFormChange"
                  :handleCancelClick="handleCancelClick" />
              </template>
              <template v-else>
                <ReadOnlyRow :contact="contact" :handleEditClick="handleEditClick"
                  :handleDeleteClick="handleDeleteClick" />
              </template>
            </template>
          </tbody>
        </table>
      </form>
      <form @submit.prevent="handleAddFormSubmit" class="flex justify-center mt-5">
        <button type="submit"
          class="text-white bg-gradient-to-r from-blue-500 via-blue-600 to-blue-700 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-blue-300  font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2">
          <span v-if="contacts.length > 0">Add Todo</span>
          <span v-else>Create New Todo List</span>
        </button>
      </form>
    </div>
  </div>
</template>