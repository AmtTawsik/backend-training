<template>
  <tr>
    <td>
      
      <input
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-11/12 p-2.5 "
        type="text" placeholder="Ex: 60.25" name="service" :value="editFormData.service" @input="handleEditFormChange" />
    </td>
    <td>
      <input
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-11/12 p-2.5 "
        type="text" placeholder="Ex: 60.25" name="last3" :value="editFormData.last3" @input="handleEditFormChange" />
    </td>
    <td>
      <input
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-11/12 p-2.5 "
        type="text" placeholder="Ex: 60.25" name="next3" :value="editFormData.next3" @input="handleEditFormChange" />
    </td>
    

    <td class="text-center">
      <div class="flex ml-3 items-center">
        <button type="submit">
          <svg height="24" version="1.1" width="24" xmlns="http://www.w3.org/2000/svg"
            xmlns:cc="http://creativecommons.org/ns#" xmlns:dc="http://purl.org/dc/elements/1.1/"
            xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
            <g transform="translate(0 -1028.4)">
              <path
                d="m22 12c0 5.523-4.477 10-10 10-5.5228 0-10-4.477-10-10 0-5.5228 4.4772-10 10-10 5.523 0 10 4.4772 10 10z"
                fill="#27ae60" transform="translate(0 1029.4)" />
              <path
                d="m22 12c0 5.523-4.477 10-10 10-5.5228 0-10-4.477-10-10 0-5.5228 4.4772-10 10-10 5.523 0 10 4.4772 10 10z"
                fill="#2ecc71" transform="translate(0 1028.4)" />
              <path d="m16 1037.4-6 6-2.5-2.5-2.125 2.1 2.5 2.5 2 2 0.125 0.1 8.125-8.1-2.125-2.1z" fill="#27ae60" />
              <path d="m16 1036.4-6 6-2.5-2.5-2.125 2.1 2.5 2.5 2 2 0.125 0.1 8.125-8.1-2.125-2.1z" fill="#ecf0f1" />
            </g>
          </svg>
        </button>
        <button type="button" @click="handleCancelClick">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
            class="w-[26px] h-[26px] text-red-600">
            <path stroke-linecap="round" stroke-linejoin="round"
              d="M9.75 9.75l4.5 4.5m0-4.5l-4.5 4.5M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
        </button>
      </div>
    </td>
  </tr>
</template>
<!-- @click.prevent="submitEditForm" -->
<script>
export default {
  props: {
    editFormData: {
      type: Object,
      required: true,
    },
    handleEditFormChange: {
      type: Function,
      required: true,
    },
    handleCancelClick: {
      type: Function,
      required: true,
    },
  },
};
</script>
