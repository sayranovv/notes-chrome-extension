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
  'red',
  'orange',
  'amber',
  'yellow',
  'lime',
  'green',
  'emerald',
  'teal',
  'cyan',
  'sky',
  'blue',
  'indigo',
  'violet',
  'purple',
  'fuchsia',
  'pink',
  'rose',
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
    color: `bg-${colors[Math.floor(Math.random() * colors.length)]}-900`,
  }
  console.log(newNote)
  notes.value.push(newNote)
  localStorage.setItem('notes', JSON.stringify(notes.value))

  noteText.value = ''
}

// const removeNote = (noteId) => {
//   notes.value = notes.value.filter((note) => note.id !== noteId)
//   localStorage.setItem('notes', JSON.stringify(notes.value))
// }

const saveUserName = () => {
  userName.value = newUserName.value
  localStorage.setItem('userName', userName.value)
}

const removeAllNotes = () => {
  notes.value = []
  localStorage.removeItem('notes')
}

const colorr = 'red'
</script>

<template>
  <main
    class="mx-auto max-w-md border-2 border-gray-500 rounded-3xl mt-10 px-5 pt-3 pb-5 shadow-2xl"
  >
    <p class="font-medium text-xl cursor-pointer w-fit" @click="userName = ''">
      Hello, {{ userName
      }}<Input
        v-if="userName === ''"
        placeholder="Enter name"
        v-model="newUserName"
        @keyup.enter="saveUserName"
        class="p-1 h-6 w-24 inline"
      />!
    </p>
    <h1 class="text-5xl font-black">Quick Notes</h1>
    <Textarea class="mt-3 rounded-3xl text-md resize-none h-[150px]" v-model="noteText" />
    <Button
      class="bg-black w-full mt-2 text-md text-white font-medium rounded-3xl py-5"
      @click="addNote"
      >Create</Button
    >
    <ScrollArea class="mt-3 h-[350px] w-full rounded-3xl">
      <ul>
        <li
          v-for="note in notes"
          :key="note.id"
          class="w-full min-h-36 flex flex-col justify-between gap-5 p-4 text-white rounded-3xl mb-2 last:mb-0"
          :class="`bg-${colorr}-900`"
        >
          <p class="w-full break-words font-medium">{{ note.text }}</p>
          <div class="flex justify-between">
            <p class="opacity-30 hover:opacity-100 transition-all">{{ note.date }}</p>
            <p class="hover:underline cursor-pointer opacity-30 hover:opacity-100 transition-all">
              Delete
            </p>
          </div>
        </li>
      </ul>
      <p
        class="text-center text-red-600 my-4 cursor-pointer hover:underline"
        @click="removeAllNotes()"
      >
        Delete all notes
      </p>
    </ScrollArea>
  </main>
</template>

<style scoped></style>

<!--{{ note.text }} | {{ note.id }} | {{ note.date }} |-->
<!--<span @click="removeNote(note.id)">Delete</span>-->
