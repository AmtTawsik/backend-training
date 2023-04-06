<script setup>
const { app, fetchData } = useMyRealmApp();
const logout = () => {
    app.currentUser.logOut();
    navigateTo("/");
};



const mongo = app.currentUser?.mongoClient("mongodb-atlas");
const collection = mongo?.db("db1").collection("todo");
const collectionUser = mongo?.db("db1").collection("users");

// functions
const contacts = ref([])
const appUsers = ref([])


const findUserData = (id) => {
    collection
        .find({ userId: id })
        .then((data) => {
            contacts.value = data
            console.log('oooooooooooooooo',data)
        })
        .catch((err) => {
            console.log(err);
        });
}

collectionUser
    .find()
    .then((data) => {
        appUsers.value = data
    })
    .catch((err) => {
        console.log(err);
    });

</script>


<template>
    <div class="flex justify-center items-center my-4 gap-5">
        <h1 class="text-xl font-semibold">Admin Page</h1>
        <button class="px-3 py-1 bg-yellow-600 rounded-md" @click="logout">Logout</button>
        <button class="px-3 py-1 bg-yellow-600 rounded-md" @click="fetchData">Fetch</button>
    </div>
    <hr>
    <section class="grid grid-cols-12">
        <div class="col-span-2 mx-auto">
            <div v-for="user in appUsers" w-full :key="user._id">
                <button @click="findUserData(user.user_id)" class="px-5 py-2 my-1 bg-cyan-400 text-xl font-bold text-white rounded-md" v-if="user.email !== 'admin@gmail.com'">{{ user.email }}</button>
            </div>
        </div>

        <div class="col-span-10">
            <div class="overflow-x-auto">
                <form @submit.prevent="handleEditFormSubmit">
                    <table v-if="contacts.length > 0" class="w-10/12 mx-auto text-sm text-left text-gray-500 bg-gray-50 ">
                        <thead class="text-xs text-gray-700 uppercase bg-gray-50  ">
                            <tr>
                                <th scope="col" class="px-4 py-3">Title</th>
                                <th scope="col" class="px-4 py-3">Status</th>
                                <th scope="col" class="px-4 py-3">Description</th>
                            </tr>
                        </thead>
                        <tbody>
                            <template v-for="contact in contacts" :key="contact._id">
                                <ReadOnlyRowAdmin :contact="contact" :handleEditClick="handleEditClick"
                                    :handleDeleteClick="handleDeleteClick" />
                            </template>
                        </tbody>
                    </table>
                </form>
            </div>
        </div>
    </section>
</template>
