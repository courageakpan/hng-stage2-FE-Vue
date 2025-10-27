<template>
  <div class="flex min-h-screen bg-gray-100 text-gray-800">
    <!-- Sidebar -->
    <aside class="w-64 bg-white shadow-md flex flex-col">
      <div class="p-6 border-b">
        <h1 class="text-2xl font-bold text-blue-600">TicketPro</h1>
      </div>
      <nav class="flex-1 p-4 space-y-2">
        <router-link 
          to="/" 
          class="block py-2 px-4 rounded-lg bg-blue-50 text-blue-600 font-medium"
          :class="{ 'bg-blue-50 text-blue-600 font-medium': $route.name === 'dashboard' }"
        >
          Overview
        </router-link>
        <router-link 
          to="/open-tickets" 
          class="block py-2 px-4 rounded-lg hover:bg-gray-100"
          :class="{ 'bg-blue-50 text-blue-600 font-medium': $route.name === 'open-tickets' }"
        >
          Open Tickets
        </router-link>
        <router-link 
          to="/closed-tickets" 
          class="block py-2 px-4 rounded-lg hover:bg-gray-100"
          :class="{ 'bg-blue-50 text-blue-600 font-medium': $route.name === 'closed-tickets' }"
        >
          Closed Tickets
        </router-link>
        <router-link 
          to="/reports" 
          class="block py-2 px-4 rounded-lg hover:bg-gray-100"
          :class="{ 'bg-blue-50 text-blue-600 font-medium': $route.name === 'reports' }"
        >
          Reports
        </router-link>
        <router-link 
          to="/admin" 
          class="block py-2 px-4 rounded-lg hover:bg-gray-100"
          :class="{ 'bg-blue-50 text-blue-600 font-medium': $route.name === 'admin' }"
        >
          Admin
        </router-link>
      </nav>
      <div class="p-4 border-t">
        <button class="w-full py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition">Logout</button>
      </div>
    </aside>

    <!-- Main Content -->
    <main class="flex-1 flex flex-col">
      <!-- Top Navbar -->
      <header class="bg-white shadow-sm py-4 px-8 flex justify-between items-center">
        <h2 class="text-xl font-semibold">Dashboard</h2>
        <nav class="space-x-6">
          <a href="#" class="text-gray-700 hover:text-blue-600 font-medium">Tickets</a>
          <a href="#" class="text-gray-700 hover:text-blue-600 font-medium">Users</a>
          <a href="#" class="text-gray-700 hover:text-blue-600 font-medium">Settings</a>
        </nav>
      </header>

      <!-- Dashboard Stats -->
      <section class="p-8 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
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

      <!-- Tickets Table -->
      <section class="p-8">
        <div class="bg-white rounded-xl shadow-md overflow-hidden">
          <div class="flex justify-between items-center border-b p-4">
            <h3 class="text-lg font-semibold">Recent Tickets</h3>
            <button class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition">+ New Ticket</button>
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
              <tr class="border-b hover:bg-gray-50">
                <td class="py-3 px-6 font-medium">#1024</td>
                <td class="py-3 px-6">Login not working</td>
                <td class="py-3 px-6">
                  <span class="px-3 py-1 text-xs font-medium bg-red-100 text-red-700 rounded-full">High</span>
                </td>
                <td class="py-3 px-6">Alex Doe</td>
                <td class="py-3 px-6">
                  <span class="px-3 py-1 text-xs font-medium bg-green-100 text-green-700 rounded-full">Open</span>
                </td>
                <td class="py-3 px-6">Oct 25, 2025</td>
              </tr>

              <tr class="border-b hover:bg-gray-50">
                <td class="py-3 px-6 font-medium">#1025</td>
                <td class="py-3 px-6">API latency issue</td>
                <td class="py-3 px-6">
                  <span class="px-3 py-1 text-xs font-medium bg-yellow-100 text-yellow-700 rounded-full">Medium</span>
                </td>
                <td class="py-3 px-6">Sarah Lee</td>
                <td class="py-3 px-6">
                  <span class="px-3 py-1 text-xs font-medium bg-amber-100 text-amber-700 rounded-full">In Progress</span>
                </td>
                <td class="py-3 px-6">Oct 24, 2025</td>
              </tr>

              <tr class="hover:bg-gray-50">
                <td class="py-3 px-6 font-medium">#1026</td>
                <td class="py-3 px-6">Email delivery failed</td>
                <td class="py-3 px-6">
                  <span class="px-3 py-1 text-xs font-medium bg-green-100 text-green-700 rounded-full">Low</span>
                </td>
                <td class="py-3 px-6">John Smith</td>
                <td class="py-3 px-6">
                  <span class="px-3 py-1 text-xs font-medium bg-gray-200 text-gray-700 rounded-full">Closed</span>
                </td>
                <td class="py-3 px-6">Oct 23, 2025</td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
// UI only — no logic or data fetching
</script>