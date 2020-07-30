<template>
  <div class="tickets">
    <div class="container">
      <div class="tickets__header">
        <img class="tickets__logo" :src="logo" />
      </div>
      <div class="tickets__main">
        <Filters @filters="filterTickets" />
        <div class="tickets__content">
          <div class="tickets__mostBtns">
            <div
              class="tickets__cheap btn"
              :class="isCheapBtn ? 'active' : ''"
              @click="sortBtns"
              data-button="cheap"
            >
              САМЫЙ ДЕШЕВЫЙ
            </div>
            <div
              class="tickets__fast btn"
              :class="isCheapBtn ? '' : 'active'"
              @click="sortBtns"
              data-button="fast"
            >
              САМЫЙ БЫСТРЫЙ
            </div>
          </div>
          <Ticket :tickets="filteredTickets" />
          <button
            class="btn active border"
            v-if="tickets.length && filteredTickets.length < tickets.length"
            @click="addTen"
          >
            Еще 10
          </button>
          <button class="btn active border" v-if="isError" @click="realoadPage">
            reload
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Ticket from "@/components/Ticket"
import Filters from "@/components/Filters"
import logo from "@/assets/logo.png"

export default {
  components: {
    Ticket,
    Filters,
  },

  data: () => ({
    logo,
    isError: false,
    isCheapBtn: true,
    splicedTickets: [],
    filteredTickets: [],
    tickets: [],
    filters: [],
  }),

  methods: {
    sortBtns() {
      if (event.target.dataset.button === "cheap") {
        this.isCheapBtn = true
        this.sortTickets(this.filteredTickets)
      } else if (event.target.dataset.button === "fast") {
        this.isCheapBtn = false
        this.sortTickets(this.filteredTickets)
      }
    },

    sortTickets(array) {
      if (this.isCheapBtn) {
        array.sort((a, b) => a.price - b.price)
      } else {
        array.sort((a, b) => a.segments[0].duration - b.segments[0].duration)
      }
    },

    filterTickets(value) {
      this.filters = value

      const filtersLen = {
        All: null,
        WithoutTransfer: 0,
        OneTransfer: 1,
        TwoTransfer: 2,
        ThreeTransfer: 3,
      }

      this.sortTickets(this.splicedTickets)

      this.filteredTickets = this.splicedTickets.filter(ticket => {
        for (const i of value) {
          for (const j of value) {
            if (ticket.segments[0].stops.length === filtersLen[i]) {
              if (ticket.segments[1].stops.length === filtersLen[j]) {
                return ticket
              }
            } else if (ticket.segments[1].stops.length === filtersLen[i]) {
              if (ticket.segments[0].stops.length === filtersLen[j]) {
                return ticket
              }
            }
          }
        }
      })
    },

    firstTen() {
      this.tickets.sort((a, b) => a.price - b.price)
      return this.tickets.splice(0, 10)
    },

    addTen() {
      this.splicedTickets = [
        ...this.splicedTickets,
        ...this.tickets.splice(0, 10),
      ]
      this.filterTickets(this.filters)
    },

    realoadPage() {
      window.location.reload()
    },
  },

  async mounted() {
    const searchId = await fetch("https://front-test.beta.aviasales.ru/search")
      .then(response => {
        if (response.ok) {
          return response.json()
        }
        throw new Error("Something went wrong.")
      })
      .then(response => response.searchId)
      .catch(error => console.error(error))

    const tickets = await fetch(
      `https://front-test.beta.aviasales.ru/tickets?searchId=${searchId}`
    )
      .then(response => {
        if (response.ok) {
          return response.json()
        }
        this.isError = true
        throw new Error("Something went wrong.")
      })
      .then(response => response.tickets)
      .catch(error => console.error("Request failed", error))

    this.tickets = tickets
    this.splicedTickets = this.firstTen()
    this.filteredTickets = this.splicedTickets
  },
}
</script>
