<script setup lang="ts">
import { onMounted, ref } from 'vue';

interface Data {
  name: string,
  class: string,
}

const nameRef = ref<string>('')
const classRef = ref<string>('')
const studentsRef = ref<Data[]>([])


const getStudent = () => {
  studentsRef.value = JSON.parse(localStorage.getItem("student") ?? "[]")
}


const saveStudent = () => {
  const nameVal: string = nameRef.value
  const classVal: string = classRef.value

  const data: Data = {
    name: nameVal,
    class: classVal
  }
  const strStudents = localStorage.getItem("student");

  const students: Data[] = JSON.parse(strStudents ?? "[]")

  students.push(data)
  localStorage.setItem("student", JSON.stringify(students))

  nameRef.value = ""
  classRef.value = ""

  getStudent()
}

const deleteStudent = (idx: number) => {
  const newStudents: Data[] = studentsRef.value
  newStudents.splice(idx, 1)
  studentsRef.value = newStudents
  localStorage.setItem("student", JSON.stringify(studentsRef.value))
}

onMounted(() => {
  getStudent()
})


</script>

<template>
  <div class="container mx-auto">
    <div class="formContainer">
      <h1 class="text-2xl font-semibold mb-5">Quản lý học sinh</h1>

      <form class="px-36 py-5" action="">
        <div class="flex gap-x-3">
          <label class="flex-1" for="name">Họ và tên</label>
          <label class="flex-1" for="class">Lớp</label>
        </div>

        <div class="flex gap-x-3 mb-3">
          <input class="border flex-1 w-full" type="text" id="name" v-model="nameRef">
          <input class="border flex-1 w-full" type="text" id="class" v-model="classRef">
        </div>

        <div class="flex justify-end">
          <button type="button" class="bg-blue-500 hover:bg-blue-700 text-white p-2 px-4 rounded" @click="saveStudent">
            Lưu
          </button>
        </div>
      </form>
    </div>


    <div class="tableContainer overflow-scroll">
      <table class="w-full border-collapse border border-black ">
        <thead>
          <tr>
            <th class="w-1/6 border border-black">ID</th>
            <th class="w-2/6 border border-black">Họ tên</th>
            <th class="w-2/6 border border-black">Lớp</th>
            <th class="w-2/6 border border-black"></th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(student, index) in studentsRef">
            <td class="border-r border-black px-2">{{ index + 1 }}</td>
            <td class="border-r border-black px-2">{{ student.name }}</td>
            <td class="border-r border-black px-2">{{ student.class }}</td>
            <td class="text-center px-2"><button @click="deleteStudent(index)"
                class="bg-red-600 hover:bg-red-700 text-white p-1 rounded">Delete</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.formContainer {
  height: 40vh;
}

.tableContainer {
  height: 60vh;
}
</style>
