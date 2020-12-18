<template>
  <div class="bg-gray-500 absolute left-0 top-0 h-screen w-full flex items-center justify-center bg-opacity-25">
      <div class="bg-white h-1/3 w-96 text-gray-600 text-left px-8 py-10 relative rounded-sm shadow-lg">
        <div class="text-lg">
          {{ message }}
        </div>
        <div class="absolute bottom-8">
          <button class="bg-red-400 px-8 py-2 rounded-md text-white focus:outline-none mr-4" @click="$emit('delete-user', toDeleteId)">Delete</button>
          <button class="bg-blue-400 px-8 py-2 rounded-md text-white focus:outline-none" @click="$emit('set-delete-modal')">Cancel</button>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  props: {
    toDeleteId: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      message: 'Are you sure you want to delete?'
    }
  }
}
</script>