<template>
  <div v-if="!loader" class="mt-5 mb-5 d-none" id="spinner">
    <div class="flex-row d-flex justify-content-center">
      <div class="spinner-border" role="status"></div>
    </div>
  </div>
  <!-- List view -->
  <div v-else>
  <CollectionList
    :emailTemplates="emailTemplates"
    @save="saveEmailTemplate"
    @delete="deleteEmailTemplate"
  />
</div>
</template>

<script setup lang="ts">
const loader = ref(false);
// Get all email templates
const { pending, data: emailTemplates } = await useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/email-templates/?offset=0&limit=100&sort_column=id&sort_direction=desc"
);
loader.value=true
// Update template data
const updateTemplate = async (form: Object) => {
  let options = {
    method: "Put",
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
      Authorization:
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYzljMGM3MjkzMWI0NGZjOWE1NTc5ZWMyOTg4NzVlYzMiLCJkIjoiMTY4MDA2MiIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNjczNzl9.3_zIDyeZ0ACxOF0VpywmxGpzdhUadzmvMRggb106s5E",
    },
    body: JSON.stringify(form),
  };
  const response = await useAuthLazyFetchPut(
    `https://v1-orm-lib.mars.hipso.cc/email-templates/${form.uid}`,
    options
  );
  let index = emailTemplates.value.findIndex((mail) => mail.uid == form.uid);
  emailTemplates.value[index] = form;
};
// Save template data
const saveEmailTemplate = async (form: Object) => {
  if (!form.uid) {
    let options = {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
        Authorization:
          "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYzljMGM3MjkzMWI0NGZjOWE1NTc5ZWMyOTg4NzVlYzMiLCJkIjoiMTY4MDA2MiIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNjczNzl9.3_zIDyeZ0ACxOF0VpywmxGpzdhUadzmvMRggb106s5E",
      },
      body: JSON.stringify(form),
    };
    const addTemplateData = await useAuthLazyFetchPost(
      "https://v1-orm-lib.mars.hipso.cc/email-templates/",
      options
    );
    // If new data added add that data to array
    emailTemplates.value.unshift(addTemplateData.data.value);
  } else updateTemplate(form);
};
// Delete template data
const deleteEmailTemplate = async (form: Object) => {
  console.log(form,"deleteEmailTemplate")
  const deleteTemplateData = await useAuthLazyFetchDelete(
    `https://v1-orm-lib.mars.hipso.cc/email-templates/${form.uid}`
  );
  // If data deleted remove from list
  if (deleteTemplateData.data.value == 1) {
    emailTemplates.value = emailTemplates.value.filter(
      (email) => email.uid != form.uid
    );
  }
};
</script>
