<script setup>
import { ScrollArea } from '@/components/ui/scroll-area'
import { Textarea } from '@/components/ui/textarea'
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import { ref } from 'vue'

const noteText = ref('')
const notes = ref(JSON.parse(localStorage.getItem('notes')) || [])
const userName = ref(localStorage.getItem('userName') || '')
const newUserName = ref('')
const colors = [
  'bg-red-800',
  'bg-orange-800',
  'bg-amber-800',
  'bg-yellow-800',
  'bg-lime-800',
  'bg-green-800',
  'bg-emerald-800',
  'bg-teal-800',
  'bg-cyan-800',
  'bg-sky-800',
  'bg-blue-800',
  'bg-indigo-800',
  'bg-violet-800',
  'bg-purple-800',
  'bg-fuchsia-800',
  'bg-pink-800',
  'bg-rose-800',
]

const addNote = () => {
  if (!noteText.value.trim()) return

  const dateNow = Date.now()
  const date = new Date(dateNow)

  const hours = String(date.getHours()).padStart(2, '0')
  const minutes = String(date.getMinutes()).padStart(2, '0')
  const day = String(date.getDate()).padStart(2, '0')
  const month = String(date.getMonth() + 1).padStart(2, '0')
  const year = date.getFullYear()

  const newNote = {
    text: noteText.value,
    id: dateNow,
    date: `${hours}:${minutes} ${day}.${month}.${year}`,
    color: colors[Math.floor(Math.random() * colors.length)],
  }

  notes.value.push(newNote)
  localStorage.setItem('notes', JSON.stringify(notes.value))

  noteText.value = ''
}

const removeNote = (noteId) => {
  notes.value = notes.value.filter((note) => note.id !== noteId)
  localStorage.setItem('notes', JSON.stringify(notes.value))
}

const saveUserName = () => {
  userName.value = newUserName.value
  localStorage.setItem('userName', userName.value)
}

const removeAllNotes = () => {
  notes.value = []
  localStorage.removeItem('notes')
}
</script>

<template>
  <main class="mx-auto w-80 rounded-3xl px-5 pt-3 pb-5 overflow-hidden text-[16px]">
    <p class="font-medium text-md cursor-pointer w-fit" @click="userName = ''">
      Hello, {{ userName
      }}<Input
        v-if="userName === ''"
        placeholder="Enter name"
        v-model="newUserName"
        @keyup.enter="saveUserName"
        class="p-1 h-6 w-24 inline rounded-lg"
      />!
    </p>
    <h1 class="text-4xl font-black">Quick Notes</h1>
    <Textarea
      class="mt-3 rounded-3xl border-2 resize-none h-28 text-[16px]"
      placeholder="Enter your note right here!"
      v-model="noteText"
    />
    <Button
      class="bg-black w-full mt-2 text-white font-medium rounded-3xl py-5 cursor-pointer text-[16px]"
      @click="addNote"
      @keyup.enter="addNote"
      >Create</Button
    >
    <ScrollArea class="mt-3 h-[296px] w-full rounded-3xl outline-none">
      <transition-group name="fade" tag="ul" class="space-y-3">
        <li
          v-for="note in notes"
          :key="note.id"
          class="w-full min-h-36 flex flex-col justify-between gap-5 p-4 text-white rounded-3xl mb-2 last:mb-0"
          :class="note.color"
        >
          <p class="w-full break-words font-medium">{{ note.text }}</p>
          <div class="flex justify-between">
            <p class="opacity-30 transition-all">{{ note.date }}</p>
            <p
              class="cursor-pointer opacity-30 hover:opacity-100 transition-all"
              @click="removeNote(note.id)"
            >
              Delete
            </p>
          </div>
        </li>
      </transition-group>
      <p
        v-if="notes.length > 0"
        class="text-center text-red-600 opacity-30 my-4 cursor-pointer hover:opacity-100 transition-all"
        @click="removeAllNotes()"
      >
        Delete all notes
      </p>
      <p v-else class="text-center opacity-30 mt-5">You haven't added any notes yet!</p>
    </ScrollArea>
  </main>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s ease;
}
.fade-enter-from {
  opacity: 0;
  transform: translateY(-50px);
}
.fade-enter-to {
  opacity: 1;
  transform: translateY(0);
}
.fade-leave-from {
  opacity: 1;
  transform: translateY(0);
}
.fade-leave-to {
  opacity: 0;
  transform: translateY(0);
}
</style>
