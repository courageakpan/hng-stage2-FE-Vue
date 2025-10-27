<template>
  <div class="min-h-screen bg-gray-100 text-gray-800 relative overflow-hidden">
    <!-- Decorative Circles -->
    <div class="absolute top-10 -left-10 w-40 h-40 bg-blue-200 rounded-full opacity-60 z-0"></div>
    <div class="absolute bottom-20 -right-10 w-56 h-56 bg-blue-300 rounded-full opacity-50 z-0"></div>

    <!-- Header -->
    <header
      v-if="page === 'landing'"
      class="bg-white shadow-sm py-4 px-6 md:px-10 flex justify-between items-center relative z-10"
    >
      <h1 class="text-xl sm:text-2xl font-bold text-blue-600">TicketPro</h1>
      <nav class="hidden sm:flex space-x-4 sm:space-x-6">
        <button @click="page = 'login'" class="text-gray-700 hover:text-blue-600 font-medium">
          Login
        </button>
        <button
          @click="page = 'signup'"
          class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition"
        >
          Get Started
        </button>
      </nav>

      <!-- Mobile menu -->
      <button
        class="sm:hidden text-blue-600 text-xl font-bold"
        @click="showMobileMenu = !showMobileMenu"
      >
        ☰
      </button>

      <transition name="fade">
        <div
          v-if="showMobileMenu"
          class="absolute top-full left-0 w-full bg-white shadow-md flex flex-col space-y-2 p-4 sm:hidden"
        >
          <button @click="page = 'login'; showMobileMenu = false" class="py-2 text-left text-gray-700 hover:text-blue-600">
            Login
          </button>
          <button
            @click="page = 'signup'; showMobileMenu = false"
            class="py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
          >
            Get Started
          </button>
        </div>
      </transition>
    </header>

    <!-- Landing -->
    <section
      v-if="page === 'landing'"
      class="flex flex-col items-center justify-center text-center px-6 py-24 sm:py-32 md:py-40 relative overflow-hidden"
    >
      <div class="absolute inset-0 bg-gradient-to-b from-blue-50 to-white clip-path-wave z-0"></div>
      <h2 class="text-3xl sm:text-4xl md:text-5xl font-bold text-gray-800 mb-6 relative z-10">
        Manage Support Tickets Easily
      </h2>
      <p class="text-gray-600 mb-10 relative z-10 max-w-xl text-sm sm:text-base">
        Streamline your ticket workflow with TicketPro. Create, manage, and resolve tickets efficiently.
      </p>
      <div class="relative z-10 flex flex-col sm:flex-row gap-4">
        <button
          @click="page = 'login'"
          class="bg-blue-600 text-white px-6 py-3 rounded-lg hover:bg-blue-700 transition w-full sm:w-auto"
        >
          Login
        </button>
        <button
          @click="page = 'signup'"
          class="bg-gray-200 text-gray-700 px-6 py-3 rounded-lg hover:bg-gray-300 transition w-full sm:w-auto"
        >
          Sign Up
        </button>
      </div>
    </section>

    <!-- Login -->
    <section v-if="page === 'login'" class="flex items-center justify-center px-4 py-16 sm:py-20">
      <div class="bg-white shadow-lg rounded-xl p-6 sm:p-8 w-full max-w-sm sm:max-w-md">
        <h2 class="text-xl sm:text-2xl font-bold text-center mb-6">Login</h2>
        <form class="space-y-4" @submit.prevent="page = 'dashboard'">
          <input
            type="email"
            placeholder="Email"
            class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
          />
          <input
            type="password"
            placeholder="Password"
            class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
          />
          <button
            type="submit"
            class="w-full bg-blue-600 text-white py-3 rounded-lg hover:bg-blue-700 transition"
          >
            Login
          </button>
        </form>
        <p class="text-center text-sm text-gray-500 mt-4">
          Don’t have an account?
          <button @click="page = 'signup'" class="text-blue-600 hover:underline">Sign up</button>
        </p>
      </div>
    </section>

    <!-- Signup -->
    <section v-if="page === 'signup'" class="flex items-center justify-center px-4 py-16 sm:py-20">
      <div class="bg-white shadow-lg rounded-xl p-6 sm:p-8 w-full max-w-sm sm:max-w-md">
        <h2 class="text-xl sm:text-2xl font-bold text-center mb-6">Create Account</h2>
        <form class="space-y-4" @submit.prevent="page = 'dashboard'">
          <input
            type="text"
            placeholder="Full Name"
            class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
          />
          <input
            type="email"
            placeholder="Email"
            class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
          />
          <input
            type="password"
            placeholder="Password"
            class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
          />
          <button
            type="submit"
            class="w-full bg-blue-600 text-white py-3 rounded-lg hover:bg-blue-700 transition"
          >
            Sign Up
          </button>
        </form>
        <p class="text-center text-sm text-gray-500 mt-4">
          Already have an account?
          <button @click="page = 'login'" class="text-blue-600 hover:underline">Login</button>
        </p>
      </div>
    </section>

    <!-- Dashboard & Tickets -->
    <div
      v-if="page === 'dashboard' || page === 'tickets'"
      class="flex flex-col md:flex-row min-h-screen relative z-10"
    >
      <!-- Sidebar -->
      <aside
        class="w-full md:w-64 bg-white shadow-md flex flex-col md:h-auto order-last md:order-first"
      >
        <div class="p-4 md:p-6 border-b flex justify-between items-center md:block">
          <h1 class="text-xl md:text-2xl font-bold text-blue-600">TicketPro</h1>
          <button class="md:hidden text-gray-600 text-xl" @click="sidebarOpen = !sidebarOpen">☰</button>
        </div>

        <transition name="fade">
          <nav
            v-show="sidebarOpen || windowWidth >= 768"
            class="flex-1 p-4 space-y-2 md:block"
          >
            <button :class="navClass('dashboard')" @click="page = 'dashboard'">Dashboard</button>
            <button :class="navClass('tickets')" @click="page = 'tickets'">Tickets</button>
          </nav>
        </transition>

        <div class="p-4 border-t">
          <button
            @click="page = 'landing'"
            class="w-full py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition"
          >
            Logout
          </button>
        </div>
      </aside>

      <!-- Main -->
      <main class="flex-1 flex flex-col">
        <header class="bg-white shadow-sm py-4 px-6 sm:px-8 flex justify-between items-center">
          <h2 class="text-lg sm:text-xl font-semibold capitalize">{{ page }}</h2>
          <nav class="hidden sm:flex space-x-6">
            <button @click="page = 'tickets'" class="text-gray-700 hover:text-blue-600 font-medium">
              Tickets
            </button>
            <button class="text-gray-700 hover:text-blue-600 font-medium">Settings</button>
          </nav>
        </header>

        <!-- Dashboard -->
        <section
          v-if="page === 'dashboard'"
          class="p-6 sm:p-8 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6"
        >
          <div class="bg-white rounded-xl shadow-md p-6 border-l-4 border-blue-500">
            <h3 class="text-gray-500 text-sm mb-2">Total Tickets</h3>
            <p class="text-3xl font-bold">128</p>
          </div>
          <div class="bg-white rounded-xl shadow-md p-6 border-l-4 border-green-500">
            <h3 class="text-gray-500 text-sm mb-2">Open Tickets</h3>
            <p class="text-3xl font-bold">42</p>
          </div>
          <div class="bg-white rounded-xl shadow-md p-6 border-l-4 border-yellow-500">
            <h3 class="text-gray-500 text-sm mb-2">In Progress</h3>
            <p class="text-3xl font-bold">18</p>
          </div>
          <div class="bg-white rounded-xl shadow-md p-6 border-l-4 border-gray-400">
            <h3 class="text-gray-500 text-sm mb-2">Closed Tickets</h3>
            <p class="text-3xl font-bold">68</p>
          </div>
        </section>

        <!-- Tickets -->
        <section v-if="page === 'tickets'" class="p-6 sm:p-8 overflow-x-auto">
          <div class="bg-white rounded-xl shadow-md overflow-hidden min-w-[700px]">
            <div class="flex flex-col sm:flex-row justify-between items-center border-b p-4 gap-4">
              <h3 class="text-lg font-semibold">Manage Tickets</h3>
              <button class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition">
                + New Ticket
              </button>
            </div>
            <table class="min-w-full text-left text-sm">
              <thead class="bg-gray-50 border-b text-gray-600">
                <tr>
                  <th class="py-3 px-6">ID</th>
                  <th class="py-3 px-6">Subject</th>
                  <th class="py-3 px-6">Priority</th>
                  <th class="py-3 px-6">Assignee</th>
                  <th class="py-3 px-6">Status</th>
                  <th class="py-3 px-6">Last Updated</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(t, i) in tickets"
                  :key="i"
                  class="border-b hover:bg-gray-50"
                >
                  <td class="py-3 px-6 font-medium">{{ t.id }}</td>
                  <td class="py-3 px-6">{{ t.subject }}</td>
                  <td class="py-3 px-6">
                    <span
                      :class="`px-3 py-1 text-xs font-medium rounded-full ${t.priorityClass}`"
                    >
                      {{ t.priority }}
                    </span>
                  </td>
                  <td class="py-3 px-6">{{ t.assignee }}</td>
                  <td class="py-3 px-6">
                    <span
                      :class="`px-3 py-1 text-xs font-medium rounded-full ${t.statusClass}`"
                    >
                      {{ t.status }}
                    </span>
                  </td>
                  <td class="py-3 px-6">{{ t.date }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </section>
      </main>
    </div>

    <!-- Footer -->
    <footer
      v-if="page === 'landing'"
      class="bg-white py-6 text-center text-gray-500 mt-16 relative z-10 border-t"
    >
      <p>© {{ new Date().getFullYear() }} TicketPro. All rights reserved.</p>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue"

const page = ref("landing")
const showMobileMenu = ref(false)
const sidebarOpen = ref(true)
const windowWidth = ref(window.innerWidth)

const handleResize = () => (windowWidth.value = window.innerWidth)
onMounted(() => window.addEventListener("resize", handleResize))
onUnmounted(() => window.removeEventListener("resize", handleResize))

const navClass = (p: string) =>
  `block w-full text-left py-2 px-4 rounded-lg font-medium ${
    page.value === p ? "bg-blue-50 text-blue-600" : "hover:bg-gray-100"
  }`

onMounted(() => {
  const style = document.createElement("style")
  style.textContent = `
    .clip-path-wave {
      clip-path: polygon(0 75%, 100% 55%, 100% 100%, 0 100%);
    }
    .fade-enter-active, .fade-leave-active {
      transition: opacity 0.2s;
    }
    .fade-enter-from, .fade-leave-to {
      opacity: 0;
    }
  `
  document.head.appendChild(style)
})

const tickets = [
  {
    id: "#1024",
    subject: "Login not working",
    priority: "High",
    priorityClass: "bg-red-100 text-red-700",
    assignee: "Alex Doe",
    status: "Open",
    statusClass: "bg-green-100 text-green-700",
    date: "Oct 25, 2025",
  },
  {
    id: "#1025",
    subject: "API latency issue",
    priority: "Medium",
    priorityClass: "bg-yellow-100 text-yellow-700",
    assignee: "Sarah Lee",
    status: "In Progress",
    statusClass: "bg-amber-100 text-amber-700",
    date: "Oct 24, 2025",
  },
  {
    id: "#1026",
    subject: "Email delivery failed",
    priority: "Low",
    priorityClass: "bg-green-100 text-green-700",
    assignee: "John Smith",
    status: "Closed",
    statusClass: "bg-gray-200 text-gray-700",
    date: "Oct 23, 2025",
  },
]
</script>
