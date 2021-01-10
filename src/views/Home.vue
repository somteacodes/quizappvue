<template>
  <main class="flex h-screen items-center justify-center bg-gray-100">
    <!-- quiz container -->

    <div
      class="overflow-hidden bg-white flex-none container relative shadow-lg rounded-lg px-12 py-6"
    >
      <img
        src="@/assets/images/abstract.svg"
        alt=""
        class="absolute -top-10 left-0 object-none"
      />

      <!-- contents -->
      <div class="relative z-20">
        <!-- score container -->
        <div class="text-right text-gray-800">
          <p class="text-sm leading-3">Score</p>
          <p class="font-bold">60</p>
        </div>

        <!-- timer container -->
        <div class="bg-white shadow-lg p-1 rounded-full w-full h-5 mt-4">
          <div class="bg-blue-700 rounded-full w-11/12 h-full"></div>
        </div>

        <!-- question container -->
        <div
          class="rounded-lg bg-gray-100 p-2 neumorph-1 text-center font-bold text-gray-800 mt-8"
        >
          <div class="bg-white p-5">
            Who is the most powerful Avenger in the marvel cinematic universe?
          </div>
        </div>

        <!-- options container -->

        <div class="mt-8">
          <!-- option container -->
          <div
            class="neumorph-1 option-default bg-gray-100 p-2 rounded-lg mb-3 relative"
          >
            <div
              class="bg-blue-700 p-1 transform rotate-45 rounded-md h-10 w-10 text-white font-bold absolute right-0 top-0 shadow-md"
            >
              <p class="transform -rotate-45">+10</p>
            </div>
            <div class="rounded-lg font-bold flex p-2">
              <!-- option ID -->
              <div class="p-3 rounded-lg">A</div>

              <!-- option name -->
              <div class="flex items-center pl-6">Thor</div>
            </div>
          </div>
          <!-- option container -->
          <div
            class="neumorph-1 option-correct bg-gray-100 p-2 rounded-lg mb-3 relative"
          >
            <div
              class="bg-blue-700 p-1 transform rotate-45 rounded-md h-10 w-10 text-white font-bold absolute right-0 top-0 shadow-md"
            >
              <p class="transform -rotate-45">+10</p>
            </div>
            <div class="rounded-lg font-bold flex p-2">
              <!-- option ID -->
              <div class="p-3 rounded-lg">B</div>

              <!-- option name -->
              <div class="flex items-center pl-6">Doctor Strange</div>
            </div>
          </div>
          <!-- option container -->
          <div
            class="neumorph-1 option-default bg-gray-100 p-2 rounded-lg mb-3 relative"
          >
            <div
              class="bg-blue-700 p-1 transform rotate-45 rounded-md h-10 w-10 text-white font-bold absolute right-0 top-0 shadow-md"
            >
              <p class="transform -rotate-45">+10</p>
            </div>
            <div class="rounded-lg font-bold flex p-2">
              <!-- option ID -->
              <div class="p-3 rounded-lg">C</div>

              <!-- option name -->
              <div class="flex items-center pl-6">Ms Marvel</div>
            </div>
          </div>
          <!-- option container -->
          <div
            class="neumorph-1 option-default bg-gray-100 p-2 rounded-lg mb-3 relative"
          >
            <div
              class="bg-blue-700 p-1 transform rotate-45 rounded-md h-10 w-10 text-white font-bold absolute right-0 top-0 shadow-md"
            >
              <p class="transform -rotate-45">+10</p>
            </div>
            <div class="rounded-lg font-bold flex p-2">
              <!-- option ID -->
              <div class="p-3 rounded-lg">D</div>

              <!-- option name -->
              <div class="flex items-center pl-6">Hulk</div>
            </div>
          </div>
        </div>

        <!-- progress indicator container -->
        <div class="mt-8 text-center">
          <div class="h-1 w-12 bg-gray-800 rounded-full mx-auto"></div>
          <p class="font-bold text-gray-800">2/10</p>
        </div>
      </div>
    </div>
  </main>
</template>

 <style scoped>
.neumorph-1 {
  box-shadow: 6px 6px 18px rgba(0, 0, 0, 0.09), -6px -6px 18px #ffffff;
}
.container {
  max-width: 400px;
  border-radius: 25px;
}
</style>