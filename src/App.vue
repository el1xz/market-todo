<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
</script>

<template>
  <div class="flex flex-col justify-between h-[100vh]">
    <div class="flex items-center gap-4 ml-4 mt-4 main">
    <div
      class="text-[14px] text-gray-600 cursor-pointer transition-all hover:border-gray-400 border rounded content px-2 py-1">
      Sign in
    </div>
    <div
      class="text-[14px] text-gray-600 cursor-pointer transition-all hover:border-gray-400 border rounded content px-2 py-1">
      Login
    </div>
    <input class="text-[14px] outline-none border transition-all rounded h-full px-2 py-[5px] focus:border-gray-500"
      type="text" placeholder="Введите данные" name="" id="">
  </div>
  <footer class="bg-gray-800 text-white footer py-4 px-4">
    <div class="flex justify-between">
      <a target=_blank class="text-gray-400 hover:text-white transition-all" href="https://www.youtube.com/watch?v=qhjxAP1hFuI">Video</a>
      <a target=_blank class="text-gray-400 hover:text-white transition-all" href="https://github.com/el1xz">el1xz ©</a>
    </div>
  </footer>
  </div>
</template>

<style>
.content {
  width: fit-content;
}
</style>