<template>
  <div class="col-span-4 bg-white ">
    <div class="px-5 py-3 mx-auto bg-gray-50">
      <div class="mb-0 text-center rounded-0">
        <!-- Template name -->
        <div class="">
          <input
            type="text"
            id="name"
            v-model=" editTemplate.name"
            class="block w-full px-3 py-2 text-gray-900 border-0 rounded-md shadow-sm ring-1 ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 disabled:cursor-not-allowed disabled:bg-gray-50 disabled:text-gray-500 disabled:ring-gray-200 sm:text-sm sm:leading-6"
            placeholder="Enter Template Name"
          />
        </div>
        <!-- Template subject -->
        <div>
          <input
            type="text"
            name="subject"
            id="subject"
            v-model="editTemplate.subject"
            class="block w-full px-3 py-2 mt-4 text-gray-900 border-0 rounded-md shadow-sm ring-1 ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 disabled:cursor-not-allowed disabled:bg-gray-50 disabled:text-gray-500 disabled:ring-gray-200 sm:text-sm sm:leading-6"
            placeholder="Enter Subject"
          />
        </div>
      </div>
    </div>
  </div>
  <div class="row-span-2 col-span-4 bg-white mt-[-170px] ">
    <!-- Template body -->
    <div class="mx-4 h-[89%]">
      <textarea
        v-model="editTemplate.body"
        rows="4"
        name="comment"
        id="comment"
        class="p-4 h-[100%] block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-300 sm:py-1.5 sm:text-sm sm:leading-6"
        placeholder="Add Template Body..."
        />
    </div>
    <div class="flex justify-end mt-4 mr-3">
      <!-- Save -->
      <button
        @click="clearTemplate"
        type="button"
        class="px-3 py-2 mr-3 text-sm font-semibold text-gray-600 bg-white border rounded-md shadow-sm hover:bg-gray-50 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
      >
        Cancel
      </button>
      <!-- Cancel -->
      <button
        @click="saveTemplate"
        type="button"
        class="px-4 py-2 text-sm font-semibold text-white bg-indigo-600 rounded-md shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
      >
        Save
      </button>
    </div>
  </div>
</template>
<script setup lang="ts">
import { defineEmits,ref,defineProps } from "vue";
const props = defineProps({
    editTemplate: Object,
    editIndex:Number
});

const emit = defineEmits(['save','clear'])

// Save template data
const saveTemplate = () => {
    let form = {
    project_id: "111",
    name: props.editTemplate.name,
    subject: props.editTemplate.subject,
    body: props.editTemplate.body,
    is_active: "1",
    type: "PLAIN_TEXT",
    share_type: "PRIVATE",
    category: "category1",
  };
  if(props.editTemplate.uid)form["uid"]=props.editTemplate.uid
  emit('save',form)
  clearTemplate()

};
// Cancel template form
const clearTemplate = () => {
   emit('clear')
};
</script>
