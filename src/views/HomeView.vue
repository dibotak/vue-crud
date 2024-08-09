<script setup>
import { computed, onMounted, ref } from 'vue';
import axios from 'axios';

const startingItems = [
  {
    id: 0,
    text: 'This is item',
  },
  {
    id: 1,
    text: 'This is item',
  },
  {
    id: 2,
    text: 'This is item',
  },
  {
    id: 3,
    text: 'Change item',
  },
  {
    id: 4,
    text: 'This is item',
  },
  {
    id: 5,
    text: 'This is item',
  },
  {
    id: 6,
    text: 'This is item',
  },
]
let itemsId = 7
const baseURL = import.meta.env.VITE_API_URL || 'http://localhost:1323'

const items = ref([])
const text = ref('')
const editId = ref(-1)
const isEdit = computed(() => editId.value >= 0)

async function getItems() {
  try {
    const res = await axios.get(baseURL + '/all')
    items.value = res.data.items
    if (items.value.length) {
      itemsId = items.value[items.value.length-1].id + 1
    }
  } catch (err) {
    console.error(err)
  }
}

onMounted(() => {
  getItems()
})

async function handleText() {
  if (!isEdit.value) {
    // items.value.push({
    //   id: itemsId,
    //   text: text.value,
    // })
    // itemsId++
    try {
      await axios.post(baseURL + '/item', {
        text: text.value
      })
      await getItems()
    } catch (err) {
      console.error(err)
    }
  } else {
    // const editIndex = items.value.findIndex((el) => el.id === editId.value)
    // items.value[editIndex] = {
    //   id: editId.value,
    //   text: text.value,
    // }
    try {
      await axios.put(baseURL + '/item/' + editId.value, {
        text: text.value
      })
      await getItems()
    } catch (err) {
      console.error(err)
    }

    editId.value = -1
  }
  text.value = ''
}

function editText(selectedItem) {
  text.value = selectedItem.text
  editId.value = selectedItem.id
}

async function removeText(selectedItem) {
  const answer = confirm('Are you sure to delete this item?')
  if (answer) {
    try {
      await axios.delete(baseURL + '/item/' + selectedItem.id)
      await getItems()
    } catch (err) {
      console.error(err)
    }
  }
}
</script>

<template>
  <main class="max-w-[1000px] mx-auto">
    <div class="mb-4 mt-8">
      <h1 class="text-2xl font-semibold">CRUD App</h1>
    </div>
    <div class="mb-4">
      <input
        class="border border-gray-500 px-2 py-0.5 rounded-md mr-4 focus:outline-none focus:ring ring-gray-500 focus:ring-2"
        v-model="text"
        @keypress.enter="handleText"
        type="text" placeholder="Type here"
      >
      <button
        class="bg-green-300 rounded-sm px-2 py-0.5"
        @click="handleText"
      >
        {{ editId >= 0 ? 'update' : 'add' }}
      </button>
    </div>
    <div style="display: flex;">
      <table class="min-w-[500px] p-2">
        <thead>
          <tr class="border-b border-slate-500">
            <th class="bg-slate-300 px-2 py-2 rounded-tl-md">No.</th>
            <th class="bg-slate-300 px-2 py-2">Text</th>
            <th class="bg-slate-300 px-2 py-2">Id</th>
            <th class="bg-slate-300 px-2 py-2 rounded-tr-md">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(item, index) in items"
            :key="'item-'+item.id"
            class=" bg-slate-50 border-b border-slate-400"
          >
            <td class="text-center">{{ index+1 }}.</td>
            <td>{{ item.text }}</td>
            <td class="text-center">{{ item.id }}</td>
            <td class="text-center px-2">
              <button
                class="bg-orange-200 rounded-sm px-2 py-0.5 m-0.5"
                @click="editText(item)"
              >
                edit
              </button>
              <button
                class="bg-red-200 rounded-sm px-2 py-0.5 m-0.5"
                @click="removeText(item)"
              >
                remove
              </button>
            </td>
          </tr>
        </tbody>
        <tfoot>
          <tr>
            <td colspan="4" class="text-center bg-slate-300 rounded-b-md">
              Total of items: {{ items.length }}
            </td>
          </tr>
        </tfoot>
      </table>
    </div>
  </main>
</template>
