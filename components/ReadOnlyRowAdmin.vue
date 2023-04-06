<template>
    <tr class="border-b ">
      <th scope="row" class="px-4 py-3 font-medium text-gray-900 whitespace-nowrap">
        {{ contact.title }}
      </th>
      <td class="px-4 py-3">{{ contact.status }}</td>
      <td class="px-4 py-3">{{ contact.description }}</td>
    </tr>
  </template>
  
  <script>
  import edit from '../assets/photo/edit.png'
  export default {
    props: {
      contact: {
        type: Object,
        required: true,
      },
      handleEditClick: {
        type: Function,
        required: true,
      },
      handleDeleteClick: {
        type: Function,
        required: true,
      },
    },
  };
  </script>