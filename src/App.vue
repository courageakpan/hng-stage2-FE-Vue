<template>
  <div class="min-h-screen bg-gray-100 text-gray-800 relative overflow-hidden">
    <!-- Decorative Circles -->
    <div class="absolute top-10 -left-10 w-40 h-40 bg-blue-200 rounded-full opacity-60 z-0"></div>
    <div class="absolute bottom-20 -right-10 w-56 h-56 bg-blue-300 rounded-full opacity-50 z-0"></div>

    <!-- Toast Notification -->
    <transition name="toast">
      <div
        v-if="toast.show"
        :class="`fixed top-4 right-4 z-50 p-4 rounded-lg shadow-lg max-w-sm ${
          toast.type === 'success' ? 'bg-green-500 text-white' : 
          toast.type === 'error' ? 'bg-red-500 text-white' : 
          'bg-blue-500 text-white'
        }`"
      >
        <div class="flex items-center">
          <span class="mr-2">
            {{ toast.type === 'success' ? '✓' : toast.type === 'error' ? '✗' : 'ℹ' }}
          </span>
          {{ toast.message }}
        </div>
      </div>
    </transition>

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
        <form class="space-y-4" @submit.prevent="handleLogin">
          <input
            type="email"
            v-model="loginForm.email"
            placeholder="Email"
            class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
            :class="{ 'border-red-500': loginErrors.email }"
            aria-describedby="login-email-error"
          />
          <p v-if="loginErrors.email" id="login-email-error" class="text-red-500 text-sm mt-1">{{ loginErrors.email }}</p>
          
          <input
            type="password"
            v-model="loginForm.password"
            placeholder="Password"
            class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
            :class="{ 'border-red-500': loginErrors.password }"
            aria-describedby="login-password-error"
          />
          <p v-if="loginErrors.password" id="login-password-error" class="text-red-500 text-sm mt-1">{{ loginErrors.password }}</p>
          
          <button
            type="submit"
            class="w-full bg-blue-600 text-white py-3 rounded-lg hover:bg-blue-700 transition"
          >
            Login
          </button>
        </form>
        <p class="text-center text-sm text-gray-500 mt-4">
          Don't have an account?
          <button @click="page = 'signup'" class="text-blue-600 hover:underline">Sign up</button>
        </p>
      </div>
    </section>

    <!-- Signup -->
    <section v-if="page === 'signup'" class="flex items-center justify-center px-4 py-16 sm:py-20">
      <div class="bg-white shadow-lg rounded-xl p-6 sm:p-8 w-full max-w-sm sm:max-w-md">
        <h2 class="text-xl sm:text-2xl font-bold text-center mb-6">Create Account</h2>
        <form class="space-y-4" @submit.prevent="handleSignup">
          <input
            type="text"
            v-model="signupForm.name"
            placeholder="Full Name"
            class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
            :class="{ 'border-red-500': signupErrors.name }"
            aria-describedby="signup-name-error"
          />
          <p v-if="signupErrors.name" id="signup-name-error" class="text-red-500 text-sm mt-1">{{ signupErrors.name }}</p>
          
          <input
            type="email"
            v-model="signupForm.email"
            placeholder="Email"
            class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
            :class="{ 'border-red-500': signupErrors.email }"
            aria-describedby="signup-email-error"
          />
          <p v-if="signupErrors.email" id="signup-email-error" class="text-red-500 text-sm mt-1">{{ signupErrors.email }}</p>
          
          <input
            type="password"
            v-model="signupForm.password"
            placeholder="Password"
            class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
            :class="{ 'border-red-500': signupErrors.password }"
            aria-describedby="signup-password-error"
          />
          <p v-if="signupErrors.password" id="signup-password-error" class="text-red-500 text-sm mt-1">{{ signupErrors.password }}</p>
          
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
            <p class="text-3xl font-bold">{{ tickets.length }}</p>
          </div>
          <div class="bg-white rounded-xl shadow-md p-6 border-l-4 border-green-500">
            <h3 class="text-gray-500 text-sm mb-2">Open Tickets</h3>
            <p class="text-3xl font-bold">{{ openTickets }}</p>
          </div>
          <div class="bg-white rounded-xl shadow-md p-6 border-l-4 border-yellow-500">
            <h3 class="text-gray-500 text-sm mb-2">In Progress</h3>
            <p class="text-3xl font-bold">{{ inProgressTickets }}</p>
          </div>
          <div class="bg-white rounded-xl shadow-md p-6 border-l-4 border-gray-400">
            <h3 class="text-gray-500 text-sm mb-2">Closed Tickets</h3>
            <p class="text-3xl font-bold">{{ closedTickets }}</p>
          </div>
        </section>

        <!-- Tickets -->
        <section v-if="page === 'tickets'" class="p-6 sm:p-8">
          <!-- Create Ticket Form -->
          <div class="bg-white rounded-xl shadow-md p-6 mb-6">
            <h3 class="text-lg font-semibold mb-4">Create New Ticket</h3>
            <form class="space-y-4" @submit.prevent="handleCreateTicket" novalidate>
              <input
                type="text"
                v-model="newTicket.subject"
                placeholder="Ticket Subject"
                class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
                :class="{ 'border-red-500': ticketErrors.subject }"
                aria-describedby="newticket-subject-error"
              />
              <p v-if="ticketErrors.subject" id="newticket-subject-error" class="text-red-500 text-sm mt-1">{{ ticketErrors.subject }}</p>
              
              <textarea
                v-model="newTicket.description"
                placeholder="Description"
                rows="4"
                class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
                :class="{ 'border-red-500': ticketErrors.description }"
                aria-describedby="newticket-description-error"
              ></textarea>
              <p v-if="ticketErrors.description" id="newticket-description-error" class="text-red-500 text-sm mt-1">{{ ticketErrors.description }}</p>
              
              <select
                v-model="newTicket.priority"
                class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
              >
                <option value="">Select Priority</option>
                <option value="Low">Low</option>
                <option value="Medium">Medium</option>
                <option value="High">High</option>
              </select>
              
              <input
                type="text"
                v-model="newTicket.assignee"
                placeholder="Assignee"
                class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
              />
              
              <button
                type="submit"
                class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition"
              >
                Create Ticket
              </button>
            </form>
          </div>

          <!-- Tickets List -->
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
            <div
              v-for="ticket in tickets"
              :key="ticket.id"
              class="bg-white rounded-xl shadow-md p-6 border-l-4 relative"
              :class="{
                'border-blue-500': ticket.priority === 'Low',
                'border-yellow-500': ticket.priority === 'Medium',
                'border-red-500': ticket.priority === 'High'
              }"
            >
              <!-- Edit/Delete Buttons -->
              <div class="absolute top-2 right-2 flex space-x-2">
                <button
                  @click="editTicket(ticket)"
                  class="text-blue-600 hover:text-blue-800 text-sm"
                >
                  ✏️
                </button>
                <button
                  @click="confirmDelete(ticket)"
                  class="text-red-600 hover:text-red-800 text-sm"
                >
                  🗑️
                </button>
              </div>
              
              <!-- Ticket Content -->
              <div class="mb-4">
                <h4 class="font-semibold text-lg mb-2">{{ ticket.subject }}</h4>
                <p class="text-gray-600 mb-3">{{ ticket.description }}</p>
                
                <div class="flex flex-wrap gap-2 mb-3">
                  <span
                    :class="`px-3 py-1 text-xs font-medium rounded-full ${
                      ticket.priority === 'High' ? 'bg-red-100 text-red-700' :
                      ticket.priority === 'Medium' ? 'bg-yellow-100 text-yellow-700' :
                      'bg-green-100 text-green-700'
                    }`"
                  >
                    {{ ticket.priority }}
                  </span>
                  <span
                    :class="`px-3 py-1 text-xs font-medium rounded-full ${
                      ticket.status === 'Open' ? 'bg-green-100 text-green-700' :
                      ticket.status === 'In Progress' ? 'bg-amber-100 text-amber-700' :
                      'bg-gray-200 text-gray-700'
                    }`"
                  >
                    {{ ticket.status }}
                  </span>
                </div>
                
                <div class="text-sm text-gray-500">
                  <p><strong>Assignee:</strong> {{ ticket.assignee }}</p>
                  <p><strong>Created:</strong> {{ ticket.date }}</p>
                  <p><strong>Last Updated:</strong> {{ ticket.lastUpdated }}</p>
                </div>
              </div>
              
              <!-- Edit Form (shown when editing) -->
              <div v-if="editingTicket && editingTicket.id === ticket.id" class="mt-4 pt-4 border-t">
                <h4 class="font-semibold mb-3">Edit Ticket</h4>
                <form class="space-y-3" @submit.prevent="handleUpdateTicket" novalidate>
                  <input
                    type="text"
                    v-model="editingTicket.subject"
                    placeholder="Ticket Subject"
                    class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
                    :class="{ 'border-red-500': editingErrors.subject }"
                    aria-describedby="edit-subject-error"
                  />
                  <p v-if="editingErrors.subject" id="edit-subject-error" class="text-red-500 text-sm mt-1">{{ editingErrors.subject }}</p>
                  <textarea
                    v-model="editingTicket.description"
                    placeholder="Description"
                    rows="3"
                    class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
                    :class="{ 'border-red-500': editingErrors.description }"
                    aria-describedby="edit-description-error"
                  ></textarea>
                  <p v-if="editingErrors.description" id="edit-description-error" class="text-red-500 text-sm mt-1">{{ editingErrors.description }}</p>
                  <select
                    v-model="editingTicket.priority"
                    class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
                  >
                    <option value="Low">Low</option>
                    <option value="Medium">Medium</option>
                    <option value="High">High</option>
                  </select>
                  <input
                    type="text"
                    v-model="editingTicket.assignee"
                    placeholder="Assignee"
                    class="w-full border rounded-lg p-3 focus:ring-2 focus:ring-blue-500"
                  />
                  <div class="flex space-x-2">
                    <button
                      type="submit"
                      class="bg-green-600 text-white px-4 py-2 rounded-lg hover:bg-green-700 transition"
                    >
                      Save
                    </button>
                    <button
                      type="button"
                      @click="cancelEdit"
                      class="bg-gray-600 text-white px-4 py-2 rounded-lg hover:bg-gray-700 transition"
                    >
                      Cancel
                    </button>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </section>
      </main>
    </div>

    <!-- Delete Confirmation Modal -->
    <transition name="modal">
      <div
        v-if="showDeleteModal"
        class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
      >
        <div class="bg-white rounded-xl p-6 max-w-md mx-4">
          <h3 class="text-lg font-semibold mb-4">Confirm Delete</h3>
          <p class="mb-6">Are you sure you want to delete ticket "{{ ticketToDelete?.subject }}"?</p>
          <div class="flex space-x-4">
            <button
              @click="showDeleteModal = false"
              class="px-4 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700 transition"
            >
              Cancel
            </button>
            <button
              @click="deleteTicket"
              class="px-4 py-2 bg-red-600 text-white rounded-lg hover:bg-red-700 transition"
            >
              Delete
            </button>
          </div>
        </div>
      </div>
    </transition>

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
import { ref, onMounted, onUnmounted, computed, watch } from "vue"

// Page state
const page = ref("landing")
const showMobileMenu = ref(false)
const sidebarOpen = ref(true)
const windowWidth = ref(window.innerWidth)

// Toast notification
const toast = ref({
  show: false,
  message: '',
  type: 'success' as 'success' | 'error' | 'info'
})

// Login form
const loginForm = ref({
  email: '',
  password: ''
})

const loginErrors = ref({
  email: '',
  password: ''
})

// Signup form
const signupForm = ref({
  name: '',
  email: '',
  password: ''
})

const signupErrors = ref({
  name: '',
  email: '',
  password: ''
})

// Tickets data
const tickets = ref([
  {
    id: "#1024",
    subject: "Login not working",
    description: "Users are unable to login to the system with their credentials. Need to investigate authentication flow.",
    priority: "High",
    assignee: "Alex Doe",
    status: "Open",
    date: "Oct 25, 2025",
    lastUpdated: "Oct 25, 2025"
  },
  {
    id: "#1025",
    subject: "API latency issue",
    description: "API responses are taking longer than expected. Need to optimize database queries.",
    priority: "Medium",
    assignee: "Sarah Lee",
    status: "In Progress",
    date: "Oct 24, 2025",
    lastUpdated: "Oct 26, 2025"
  },
  {
    id: "#1026",
    subject: "Email delivery failed",
    description: "Email notifications are not being sent to users. Check SMTP configuration.",
    priority: "Low",
    assignee: "John Smith",
    status: "Closed",
    date: "Oct 23, 2025",
    lastUpdated: "Oct 25, 2025"
  }
])

// New ticket form
const newTicket = ref({
  subject: '',
  description: '',
  priority: '',
  assignee: ''
})

const ticketErrors = ref({
  subject: '',
  description: ''
})

// Edit ticket
const editingTicket = ref<any | null>(null)
const editingErrors = ref({ subject: '', description: '' })

// Delete modal
const showDeleteModal = ref(false)
const ticketToDelete = ref<any | null>(null)

// Computed properties
const openTickets = computed(() => tickets.value.filter(t => t.status === 'Open').length)
const inProgressTickets = computed(() => tickets.value.filter(t => t.status === 'In Progress').length)
const closedTickets = computed(() => tickets.value.filter(t => t.status === 'Closed').length)

// Window resize handler
const handleResize = () => (windowWidth.value = window.innerWidth)
onMounted(() => window.addEventListener("resize", handleResize))
onUnmounted(() => window.removeEventListener("resize", handleResize))

// Navigation helper
const navClass = (p: string) =>
  `block w-full text-left py-2 px-4 rounded-lg font-medium ${
    page.value === p ? "bg-blue-50 text-blue-600" : "hover:bg-gray-100"
  }`

// Toast notification helper
const showToast = (message: string, type: 'success' | 'error' | 'info' = 'success') => {
  toast.value = { show: true, message, type }
  setTimeout(() => {
    toast.value.show = false
  }, 3000)
}

// Validation helpers
const validateEmail = (email: string) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(email)
}

const validateLogin = () => {
  loginErrors.value = { email: '', password: '' }
  
  if (!loginForm.value.email) {
    loginErrors.value.email = 'Email is required'
  } else if (!validateEmail(loginForm.value.email)) {
    loginErrors.value.email = 'Please enter a valid email'
  }
  
  if (!loginForm.value.password) {
    loginErrors.value.password = 'Password is required'
  } else if (loginForm.value.password.length < 6) {
    loginErrors.value.password = 'Password must be at least 6 characters'
  }
  
  return Object.values(loginErrors.value).every(error => !error)
}

const validateSignup = () => {
  signupErrors.value = { name: '', email: '', password: '' }
  
  if (!signupForm.value.name) {
    signupErrors.value.name = 'Name is required'
  } else if (signupForm.value.name.length < 2) {
    signupErrors.value.name = 'Name must be at least 2 characters'
  }
  
  if (!signupForm.value.email) {
    signupErrors.value.email = 'Email is required'
  } else if (!validateEmail(signupForm.value.email)) {
    signupErrors.value.email = 'Please enter a valid email'
  }
  
  if (!signupForm.value.password) {
    signupErrors.value.password = 'Password is required'
  } else if (signupForm.value.password.length < 6) {
    signupErrors.value.password = 'Password must be at least 6 characters'
  }
  
  return Object.values(signupErrors.value).every(error => !error)
}

const validateTicket = () => {
  ticketErrors.value = { subject: '', description: '' }
  
  if (!newTicket.value.subject) {
    ticketErrors.value.subject = 'Subject is required'
  } else if (newTicket.value.subject.length < 3) {
    ticketErrors.value.subject = 'Subject must be at least 3 characters'
  }
  
  if (!newTicket.value.description) {
    ticketErrors.value.description = 'Description is required'
  } else if (newTicket.value.description.length < 10) {
    ticketErrors.value.description = 'Description must be at least 10 characters'
  }
  
  return Object.values(ticketErrors.value).every(error => !error)
}

// Editing validation (real-time)
const validateEditingTicket = () => {
  editingErrors.value = { subject: '', description: '' }
  if (!editingTicket.value) return false

  if (!editingTicket.value.subject) {
    editingErrors.value.subject = 'Subject is required'
  } else if (editingTicket.value.subject.length < 3) {
    editingErrors.value.subject = 'Subject must be at least 3 characters'
  }

  if (!editingTicket.value.description) {
    editingErrors.value.description = 'Description is required'
  } else if (editingTicket.value.description.length < 10) {
    editingErrors.value.description = 'Description must be at least 10 characters'
  }

  return Object.values(editingErrors.value).every(error => !error)
}

// Form handlers
const handleLogin = () => {
  if (validateLogin()) {
    showToast('Login successful! Redirecting to dashboard...', 'success')
    setTimeout(() => {
      page.value = 'dashboard'
    }, 1500)
  } else {
    showToast('Please fix login errors', 'error')
  }
}

const handleSignup = () => {
  if (validateSignup()) {
    showToast('Account created successfully! Redirecting to dashboard...', 'success')
    setTimeout(() => {
      page.value = 'dashboard'
    }, 1500)
  } else {
    showToast('Please fix signup errors', 'error')
  }
}

const handleCreateTicket = () => {
  if (validateTicket()) {
    const ticket = {
      id: `#${1027 + tickets.value.length}`,
      subject: newTicket.value.subject,
      description: newTicket.value.description,
      priority: newTicket.value.priority || 'Medium',
      assignee: newTicket.value.assignee || 'Unassigned',
      status: 'Open',
      date: new Date().toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' }),
      lastUpdated: new Date().toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' })
    }
    
    tickets.value.unshift(ticket)
    
    // Reset form
    newTicket.value = { subject: '', description: '', priority: '', assignee: '' }
    ticketErrors.value = { subject: '', description: '' }
    
    showToast('Ticket created successfully!', 'success')
  } else {
    showToast('Please fix the ticket form errors', 'error')
  }
}

const editTicket = (ticket: any) => {
  editingTicket.value = { ...ticket }
  editingErrors.value = { subject: '', description: '' }
}

const cancelEdit = () => {
  editingTicket.value = null
  editingErrors.value = { subject: '', description: '' }
}

const handleUpdateTicket = () => {
  if (!editingTicket.value) return

  // validate before saving
  if (!validateEditingTicket()) {
    showToast('Please fix the edit form errors', 'error')
    return
  }
  
  const index = tickets.value.findIndex(t => t.id === editingTicket.value.id)
  if (index !== -1) {
    tickets.value[index] = {
      ...editingTicket.value,
      lastUpdated: new Date().toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' })
    }
    
    editingTicket.value = null
    editingErrors.value = { subject: '', description: '' }
    showToast('Ticket updated successfully!', 'success')
  }
}

const confirmDelete = (ticket: any) => {
  ticketToDelete.value = ticket
  showDeleteModal.value = true
}

const deleteTicket = () => {
  if (!ticketToDelete.value) return
  
  const index = tickets.value.findIndex(t => t.id === ticketToDelete.value.id)
  if (index !== -1) {
    tickets.value.splice(index, 1)
    showToast('Ticket deleted successfully!', 'success')
  }
  
  showDeleteModal.value = false
  ticketToDelete.value = null
}

// real-time (watch) validations for create form
watch(() => newTicket.value.subject, (val) => {
  if (!val) ticketErrors.value.subject = 'Subject is required'
  else if (val.length < 3) ticketErrors.value.subject = 'Subject must be at least 3 characters'
  else ticketErrors.value.subject = ''
})

watch(() => newTicket.value.description, (val) => {
  if (!val) ticketErrors.value.description = 'Description is required'
  else if (val.length < 10) ticketErrors.value.description = 'Description must be at least 10 characters'
  else ticketErrors.value.description = ''
})

// real-time validation for editing form fields
watch(() => editingTicket.value && editingTicket.value.subject, () => {
  if (editingTicket.value) validateEditingTicket()
})
watch(() => editingTicket.value && editingTicket.value.description, () => {
  if (editingTicket.value) validateEditingTicket()
})

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
    .toast-enter-active, .toast-leave-active {
      transition: all 0.3s ease;
    }
    .toast-enter-from {
      transform: translateX(100%);
      opacity: 0;
    }
    .toast-leave-to {
      transform: translateX(100%);
      opacity: 0;
    }
    .modal-enter-active, .modal-leave-active {
      transition: opacity 0.2s;
    }  dont change any other code on this . Ticket Management Screen (CRUD)
Implement a fully functional Ticket Management page with:
Create: Form to create new tickets.
Read: Display a list of existing tickets using card-style boxes with status tags.
Update: Edit existing ticket details with form validation.
Delete: Allow users to remove tickets with a confirmation step.
All actions must:
          - Include real-time validation.
          - Display clear success or error feedback via inline messages or toast notifications. 
    .modal-enter-from, .modal-leave-to {
      opacity: 0;
    }
  `
  document.head.appendChild(style)
})
</script>