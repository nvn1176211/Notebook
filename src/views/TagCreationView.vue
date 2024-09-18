<script setup>
import { ref, reactive } from 'vue';
import { useRouter } from 'vue-router';
import SubmitBtnComponent from '../components/SubmitBtn.vue';

const router = useRouter();
const input = reactive({
    name: {
        errMsg: null,
        isInvalid: false,
        val: null,
    },
    description: {
        errMsg: null,
        isInvalid: false,
        val: null,
    }
});
const isDisabledBtn = ref(false);
async function save() {
    // helpers.refreshFormErrInput(input);
    isDisabledBtn.value = true;
    let formdata = new FormData();
    if (input.name.val) formdata.append("name", input.name.val);
    if (input.description.val) formdata.append("description", input.description.val);
    const response = await fetch(`${env.API_BASE}/register`, {
        method: "POST",
        headers: {
            'Accept': 'application/json',
        },
        body: formdata
    });
    const resBodyObj = await response.json();
    switch (response.status) {
        case 422:
            // helpers.handleInvalidInput(resBodyObj, input);
            break;
        case 201:
            // helpers.setCookieY('api_token', resBodyObj.api_token, 1, '/');
            // sessionStorage.toastMsg = t("messages.successRegisterMsg")
            // router.push('/');
            break;
    }
    isDisabledBtn.value = false;
}
</script>
<template>
  <h1 class="mb-5">Tag Creation</h1>
  <div>
    <div class="row">
      <div class="col-12 col-lg-6">
        <div class="mb-3">
          <label for="tagName" class="form-label text-capitalize">name</label>
          <input type="text" class="form-control" id="tagName" v-model="input.name.val" required>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-12">
        <div class="mb-3">
          <label for="description" class="form-label text-capitalize">description</label>
          <textarea class="form-control" id="description" v-model="input.description.val" rows="3"></textarea>
        </div>
      </div>
    </div>
    <div class="text-end">
      <SubmitBtnComponent @submit="save" :isDisabled="isDisabledBtn" class="text-capitalize">Save</SubmitBtnComponent>
    </div>
  </div>
</template>
