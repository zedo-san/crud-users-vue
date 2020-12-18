<template>
  <div class="bg-gray-500 absolute left-0 top-0 h-screen w-full flex items-center justify-center bg-opacity-25">
      <div class="bg-gray-white h-2/3 w-96 text-gray-600 text-left px-8 py-10 relative rounded-sm shadow-lg">
        <h2 class="font-semibold text-2xl mb-5">Edit User</h2>
        <form action="" class="flex flex-col" @submit.prevent="updateUser">
          <div class="mb-5">
            <label class="ml-3">Name</label>
            <input type="text" v-model="name" class="px-2 py-3 border border-gray-200 rounded-md w-full border">
            
          </div>
          <div class="mb-5">
            <label class="ml-3">Age</label>
            <input type="text" v-model="age" class="px-2 py-3 border border-gray-200 rounded-md w-full">
          </div>
          <div class="mb-5">
            <label class="ml-3">Gender</label>
            <select name="" id="" v-model="gender" class="px-2 py-3 border border-gray-200 rounded-md w-full outline-none">
              <option value="male">Male</option>
              <option value="female">Female</option>
            </select>
          </div>
          <div class="mb-5">
            <label class="ml-3">Status</label>
            <select name="" id="" v-model="status" :class="status === 'active' ? 'bg-green-400' : 'bg-red-400'" class="px-2 py-3 border border-gray-200 rounded-md w-full outline-none text-white">
              <option value="active" class="bg-green-400 text-white">Active</option>
              <option value="inactive" class="bg-red-400 text-white">Inactive</option>
            </select>
          </div>
          <div>
            <button class="px-4 py-1 bg-blue-400 text-white rounded-md focus:outline-none mr-3" type="submit">Update</button>
            <button class="px-4 py-1 bg-gray-400 text-white rounded-md focus:outline-none" @click="$emit('set-form-modal')">Cancel</button>
          </div>
        </form>
      </div>
  </div>
</template>

<script>
export default {
  props: {
    user: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      id: this.user.id,
      name: this.user.name,
      age: this.user.age,
      gender: this.user.gender,
      status: this.user.status,
    }
  },
  methods: {
    updateUser() {
      let newUserInfo = {
        id: this.id,
        name: this.name,
        age: this.age,
        gender: this.gender,
        status: this.status
      }
      this.$emit('update-user', newUserInfo)
    }
  }
}
</script>