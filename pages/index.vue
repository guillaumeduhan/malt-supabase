<script setup>
const client = useSupabaseClient()

const state = reactive({
  loading: false,
  disabled: false,
  error: undefined
})

const messages = ref([])
const newMessage = ref({
  name: undefined,
  description: undefined
})

const sendMessage = async () => {
  if (!newMessage.value.name || !newMessage.value.description) {
    return
  }
  try {
    state.loading = true
    const { data, error } = await client
      .from('posts')
      .insert(newMessage.value)

    if (data) {
      messages.value.push(newMessage.value);
      newMessage.value = {
        name: undefined,
        description: undefined
      }
    }
  } catch (e) {
    console.log(e)
  } finally {
    state.loading = false
  }
}

const fetchMessages = async () => {
  try {
    state.loading = true
    const { data, error } = await client
      .from('posts')
      .select('*')
    if (data) {
      messages.value = data;
    }
  } catch (e) {
    console.log(e)
  } finally {
    state.loading = false
  }
}

const posts = client
  .channel('custom-all-channel')
  .on(
    'postgres_changes',
    { event: '*', schema: 'public', table: 'posts' },
    (payload) => {
      messages.value.push(payload.new)
    }
  )
  .subscribe()

fetchMessages()
</script>

<template>
  <div class="container" style="max-width: 500px;">
    <header class="my-8 text-center">
      <h1 class="mb-2 text-2xl font-bold">Supabase Realtime test</h1>
      <p class="mb-2 text-base text-slate-500">By Codewithguillaume</p>
    </header>
    <div class="mx-auto">
      <h2 class="mb-2 font-medium">Post a kind & new message:</h2>
      <input v-model="newMessage.name" placeholder="Freddy Mercury" class="w-full px-2 py-2 mb-1 border rounded-lg">
      <input v-model="newMessage.description" placeholder="Hello there !" class="w-full px-2 py-2 border rounded-lg">
      <button :disabled="state.loading" @click="sendMessage"
        class="px-2 py-2 my-2 border rounded-lg disabled:opacity-25">Send</button>
    </div>
    <main class="my-12">
      <div class="px-4 py-3 mb-3 border rounded-lg" v-for="item, index in messages" :key="index">
        <p class="mb-2 text-lg" v-if="item.description">{{ item.description }}</p>
        <p class="text-xs text-slate-500" v-if="item.name">Par {{ item.name }}</p>
      </div>
    </main>
  </div>
</template>