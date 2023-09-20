<script setup lang="ts">
import { onMounted, ref } from 'vue';

interface Data {
  id: number,
  name: string,
  class: string,
}

const nameRef = ref<string>('')
const classRef = ref<string>('')
const studentsRef = ref<Data[]>([])


const getStudent = async () => {
  const dataRes = await fetch("http://localhost:5173/api");

  const data = await dataRes.json();
  studentsRef.value = data;
}

const postStudent = async () => {
  const nameVal: string = nameRef.value
  const classVal: string = classRef.value
  nameRef.value = ""
  classRef.value = ""

  const data = {
    name: nameVal,
    class: classVal
  }

  await fetch("http://localhost:5173/api", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(data)
  })

  getStudent()
}

const deleteStudent = async (idx: number) => {
  await fetch("http://localhost:5173/api", {
    method: "DELETE",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({id: idx})
  });

  getStudent()
}

onMounted(() => {
  getStudent()
})

</script>

<template>
  <div class="container mx-auto flex flex-col h-[100vh]">
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
          <button type="button" class="bg-blue-500 hover:bg-blue-700 text-white p-2 px-4 rounded" @click="postStudent">
            Lưu
          </button>
        </div>
      </form>
    </div>


    <div class="tableContainer overflow-scroll my-3">
      <table class="w-full border-collapse border border-black ">
        <thead>
          <tr class="sticky top-0">
            <th class="w-1/6">
              <div class="border border-solid border-black">ID</div>
            </th>
            <th class="w-2/6">
              <div class="border border-solid border-black">Họ tên</div>
            </th>
            <th class="w-2/6">
              <div class="border border-solid border-black">Lớp</div>
            </th>
            <th class="w-1/6">
              <div class="border border-solid border-black">Xóa</div>
            </th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(student, index) in studentsRef">
            <td class="border-r border-black px-2">{{ index + 1 }}</td>
            <td class="border-r border-black px-2">{{ student.name }}</td>
            <td class="border-r border-black px-2">{{ student.class }}</td>
            <td class="text-center px-2"><button @click="deleteStudent(student.id)"
                class="bg-red-600 hover:bg-red-700 text-white p-1 rounded">Delete</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped></style>
