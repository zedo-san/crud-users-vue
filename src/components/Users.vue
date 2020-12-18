<template>
  <div class="flex flex-col mx-10">
    <div class="flex flex-col mb-10">
    <div class="mt-5">
      <h2 class="text-2xl font-semibold leading-tight">Users</h2>
      <div class="">
        <select name="" id="" class="w-28 py-2 px-2 border border-gray-300 rounded-sm text-gray-500 mb-1 focus:outline-none">
          <option>All</option>
          <option>Active</option>
          <option>Inactive</option>
        </select><br>
        <div class="relative">
          <span class="absolute top-3 left-3">
            <svg viewBox="0 0 24 24" class="h-4 w-4 fill-current text-gray-500">
                <path d="M10 4a6 6 0 100 12 6 6 0 000-12zm-8 6a8 8 0 1114.32 4.906l5.387 5.387a1 1 0 01-1.414 1.414l-5.387-5.387A8 8 0 012 10z"></path>
            </svg>
          </span>
          <input 
            type="text" 
            placeholder="Search"
            class="w-full md:w-96 py-2 pr-5 pl-8 border border-gray-300 rounded-sm text-gray-500 mb-1 focus:outline-none">
        </div>
      </div>
    </div>
    <div class="mt-5">
      <table class="border border-gray-200 shadow-sm w-full">
        <thead>
          <tr class="text-gray-400 text-sm border border-gray-200">
            <th class="font-thin px-4 py-3 text-left">NAME</th>
            <th class="font-thin px-4 py-3 text-left">AGE</th>
            <th class="font-thin px-4 py-3 text-left">GENDER</th>
            <th class="font-thin px-4 py-3 text-left">STATUS</th>
            <th class="font-thin px-4 py-3 text-left">ACTION</th>
          </tr>
        </thead>
        <tbody class="bg-white text-left">
          <tr class="text-gray-600 relative" v-for="user in users" :key="user.id">
            <td class="px-4 py-6">{{ user.name }}</td>
            <td class="px-4 py-6">{{ user.age }}</td>
            <td class="px-4 py-6">{{ user.gender }}</td>
            <td class="px-4 py-6">
              <div :class="user.status === 'active' ? 'bg-green-400' : 'bg-red-400'" class="px-3 py-1 rounded-lg text-center text-white">{{ user.status }}</div>
            </td>
            <td class="px-4 py-6 flex flex-col">
              <a href="" class="text-blue-400" @click.prevent="setFormModal(user)">Edit</a>
              <a href="" class="text-red-400" @click.prevent="setDeleteModal(user.id)">Delete</a>
            </td>          
          </tr>
          <tr class="bg-gray-100" v-if="!addUser" @click="setAddUser">
            <td class="px-4 py-6">
              <button class="px-3 py-1 bg-blue-400 rounded-md text-white focus:outline-none"><span class="absolute -ml-3 sm:relative sm:ml-0">+</span> Add User</button>
            </td>
          </tr>
          <tr class="bg-white" v-if="addUser">
            <td class="px-4 py-6">
              <input type="text" v-model="name" class="border border-gray-300 rounded-md px-2 py-1 w-20 md:w-32">
            </td>
            <td class="px-4 py-6">
              <input type="text" v-model="age" class="border border-gray-300 rounded-md px-2 py-1 w-12">
            </td>
            <td class="px-4 py-6">
              <select v-model="gender" name="" id="" class="border border-gray-300 rounded-md px-2 py-1 w-20">
                <option value="male">Male</option>
                <option value="female">Female</option>
              </select>
            </td>
            <td class="px-4 py-6">
              <select v-model="status" :class="status === 'active' ? 'bg-green-400' : 'bg-red-400'" name="" id="" class="focus:outline-none text-white border border-gray-300 rounded-md px-2 py-1 w-22">
                <option value="active" class="bg-green-400 text-white">Active</option>
                <option value="inactive" class="bg-red-400 text-white">Inactive</option>
              </select>
            </td>
            <td class="px-4 py-6">
              <button 
                class="px-2 py-1 bg-blue-400 rounded-md text-white focus:outline-none md:mr-1"
                @click="submitUser">Add</button>
              <button 
                class="px-2 py-1 bg-red-400 rounded-md text-white focus:outline-none"
                @click="setAddUser">Cancel
              </button>
            </td>
          </tr>
        </tbody>
      </table>
      <delete-modal v-if="deleteModal" @set-delete-modal="setDeleteModal" @delete-user="deleteUser" :toDeleteId="toDeleteId"></delete-modal>
      <form-modal v-if="formModal" @set-form-modal="setFormModal" @update-user="updateUser" :user="toEditUser"></form-modal>
    </div>
  </div>
  
  </div>
</template>

<script>
import Axios from 'axios';
import DeleteModal from './DeleteModal.vue';
import FormModal from './FormModal.vue';

export default {
  components: { DeleteModal, FormModal },
  props: {},
  data() {
    return {
      addUser: false,
      name: '',
      age: '',
      gender: 'male',
      status: 'active',
      users: [
        {
          id: 12324,
          name: 'Janri',
          age: '55',
          gender: 'female',
          status: 'active'
        }
      ],
      deleteModal: false,
      formModal: false,
      toDeleteId: '',
      toEditUser: {}
    }
  },
  mounted() {
    this.getUsers()
  },
  updated() {
    this.getUsers()
  },
  methods: {
    setAddUser() {
      this.addUser = !this.addUser
    },
    setDeleteModal(id) {
      this.deleteModal = !this.deleteModal
      if(id !== ''){
        this.toDeleteId = id
      }

    },
    setFormModal(user) {
      this.formModal = !this.formModal
      if(user !== undefined || user !== '') {
        this.toEditUser = user
      }
    },
    submitUser() {
      if(this.name !== '' && this.age !== '' && this.users.filter(user => user.name === this.name).length === 0) {
        let newUser = {
          id: this.users.length + 1,
          name: this.name,
          age: this.age,
          gender: this.gender,
          status: this.status
        }
        // this.users = [...this.users, newUser]
        // this.name = '';
        // this.age = '';
        // this.gender = 'male',
        // this.status = 'active'
        // this.setAddUser()
        Axios
          .post('https://mednefits.getsandbox.com/users', newUser)
          .then(res => {
            this.name = '';
            this.age = '';
            this.gender = 'male',
            this.status = 'active'
            this.setAddUser()
          })
          .catch(err => console.log(err))
        
      }
      if(this.users.filter(user => user.name === this.name).length > 0){
        console.log('User already taken')
      }
      if(this.name === '' || this.age === '' || this.status ==='' || this.gender ===''){
        console.log('Do not leave fields blank')
      }
    },
    getUsers() {
      Axios
        .get('https://mednefits.getsandbox.com/users')
        .then(res => this.users = res.data)
        .catch(err => console.log(err.message))
    },
    updateUser(newUserInfo) {
      
      Axios
        .get(`https://mednefits.getsandbox.com:443/users/update/${newUserInfo.id}`, newUserInfo)
        .then(res => this.formModal = false)
        .catch(err => console.log(err))
      // this.users.map(user => {
      //   if(user.id === newUserInfo.id){
      //     user.name = newUserInfo.name
      //     user.age = newUserInfo.age
      //     user.gender = newUserInfo.gender
      //     user.status = newUserInfo.status
      //   }
      // })
      // this.formModal = false
      
    },
    deleteUser(id) {
      Axios
        .get(`https://mednefits.getsandbox.com:443/users/delete/${id}`)
        .then(res => this.deleteModal = false)
        .catch(err => console.log(err))
      // this.users = this.users.filter(user => user.id !== id)
      // this.deleteModal = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
