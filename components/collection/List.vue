<template>
  <div
    class="grid grid-rows-3 grid-flow-col grid-cols-4 border w-[80vw] mx-auto my-5 rounded-lg pr-[4px] h-[90vh]"
  >
    <div
      class="row-span-3 p-5 border-r rounded-l-lg shadow bg-gray-50 h-[calc(100vh-150px)] overflow-auto"
    >
      <!-- Add template buntton for LHS -->
      <div class="pb-3 w-[100%]">
        <button
          @click="showForm()"
          class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
        >
          <span>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-5 h-5 mr-2"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M12 4.5v15m7.5-7.5h-15"
              />
            </svg>
          </span>
          Add Template
        </button>
      </div>
      <!-- Show all template data -->
      <div
        v-for="(item, index) in emailTemplates"
        :key="item.uid"
        class="p-4 mb-3 bg-white border rounded-md shadow-sm"
      >
        <div class="flex justify-between group">
          <h5 class="font-[500] text-md mb-2">{{ item.name }}</h5>
          <p class="mb-2 text-sm">{{ item.subject }}</p>
          <p class="mb-2 text-sm">{{ item.body }}</p>
          <div class="flex group">
            <!-- Edit template -->
            <div
              @click="editEmailTemplate(item, index)"
              class="flex items-center justify-center w-12 h-12 mx-auto sm-blue-100"
            >
              <PencilIcon
                class="invisible w-6 h-6 group-hover:visible"
                aria-hidden="true"
              />
            </div>
            <!-- Delete template -->

            <div
              @click="deleteEmailTemplate(item)"
              class="flex items-center justify-center w-12 h-12 mx-auto sm-red-100"
            >
              <TrashIcon
                class="invisible w-6 h-6 hover:text-red-600 group-hover:visible"
                aria-hidden="true"
              />
            </div>
          </div>
        </div>
      </div>
      <!-- Delete confirmation model -->
      <TransitionRoot as="template" :show="open">
        <Dialog as="div" class="relative z-10" @close="open = false">
          <TransitionChild
            as="template"
            enter="ease-out duration-300"
            enter-from="opacity-0"
            enter-to="opacity-100"
            leave="ease-in duration-200"
            leave-from="opacity-100"
            leave-to="opacity-0"
          >
            <div
              class="fixed inset-0 transition-opacity bg-gray-500 bg-opacity-75"
            />
          </TransitionChild>
          <div class="fixed inset-0 z-10 overflow-y-auto">
            <div
              class="flex items-end justify-center min-h-full p-4 text-center sm:items-center sm:p-0"
            >
              <TransitionChild
                as="template"
                enter="ease-out duration-300"
                enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
                enter-to="opacity-100 translate-y-0 sm:scale-100"
                leave="ease-in duration-200"
                leave-from="opacity-100 translate-y-0 sm:scale-100"
                leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
              >
                <DialogPanel
                  class="relative px-4 pt-5 pb-4 overflow-hidden text-left transition-all transform bg-white rounded-lg shadow-xl sm:my-8 sm:w-full sm:max-w-lg sm:p-6"
                >
                  <div>
                    <div
                      class="flex items-center justify-center w-12 h-12 mx-auto bg-red-100 rounded-full"
                    >
                      <TrashIcon
                        class="w-6 h-6 text-red-600"
                        aria-hidden="true"
                      />
                    </div>

                    <div class="mt-3 text-center sm:mt-5">
                      <DialogTitle
                        as="h3"
                        class="text-base font-semibold leading-6 text-gray-900"
                        >Are you sure want to delete ?</DialogTitle
                      >
                    </div>
                  </div>

                  <div
                    class="mt-5 sm:mt-6 sm:grid sm:grid-flow-row-dense sm:grid-cols-2 sm:gap-3"
                  >
                    <button
                      type="button"
                      class="inline-flex justify-center w-full px-3 py-2 text-sm font-semibold text-white bg-indigo-600 rounded-md shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 sm:col-start-2"
                      @click="deleteModal(item)"
                    >
                      Delete
                    </button>

                    <button
                      type="button"
                      class="inline-flex justify-center w-full px-3 py-2 mt-3 text-sm font-semibold text-gray-900 bg-white rounded-md shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:col-start-1 sm:mt-0"
                      @click="open = false"
                      ref="cancelButtonRef"
                    >
                      cancel
                    </button>
                  </div>
                </DialogPanel>
              </TransitionChild>
            </div>
          </div>
        </Dialog>
      </TransitionRoot>
    </div>
    <!-- Empty state -->
    <CollectionAdd
      v-if="showTemplate"
      :editTemplate="editTemplate"
      :editIndex="editIndex"
      @save="saveTemplate"
      @hideTemplateForm="showTemplate = false"
      @clear="clearForm"
    />
    <div
      class="absolute top-[23rem] left-[61rem] font-extrabold text-3xl"
      v-else
    >
      <!-- Add template button in RHS in empty state condition -->
      <div>
        <div class="pb-3 w-[100%]">
          <button
            class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
            @click="showForm()"
          >
            Add Template
          </button>
        </div>
        Please click on add button to add email template
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { PencilIcon, TrashIcon } from "@heroicons/vue/24/outline";
import {
  Dialog,
  DialogPanel,
  DialogTitle,
  TransitionChild,
  TransitionRoot,
} from "@headlessui/vue";
import { defineEmits, ref } from "vue";

const selectedemail = ref({});
const open = ref(false);
const props = defineProps({
  emailTemplates: Array,
});

const editTemplate = ref({
  name: "",
  subject: "",
  body: "",
});
const showTemplate = ref(false);
const editIndex = ref(0);

const emit = defineEmits(["save", "delete"]);
// Save template data
const saveTemplate = (form: Object) => {
  emit("save", form);
};
// Delete template data
const deleteEmailTemplate = (selectedEmail: Object) => {
  selectedemail.value = selectedEmail;
  open.value = true;
};
// Edit template data
const editEmailTemplate = (selectedEmail: Object, index: Number) => {
  editTemplate.value = { ...selectedEmail };
  editIndex.value = index;
  showTemplate.value = true;
};
// Delete confirmation
const deleteModal = () => {
  open.value = false;
  emit("delete", selectedemail.value);
};

// Open input form
const showForm = () => {
  editTemplate.value = {
    name: "",
    subject: "",
    body: "",
  };
  showTemplate.value = true;
};
// Clear input form
const clearForm = () => {
  editTemplate.value = {
    name: "",
    subject: "",
    body: "",
  };
  showTemplate.value = false;
};
</script>
