<template>
  <div class="tickets__filters">
    <div class="tickets__filters_title">
      КОЛИЧЕСТВО ПЕРЕСАДОК
    </div>
    <ul class="tickets__filters_list">
      <li class="tickets__filters_item">
        <label class="checkbox">
          <input
            value="All"
            type="checkbox"
            v-model="filters"
            @change="checkbox"
          />
          <span class="checkmark"></span>
          <span>Все</span>
        </label>
      </li>
      <li class="tickets__filters_item">
        <label class="checkbox">
          <input
            value="WithoutTransfer"
            type="checkbox"
            v-model="filters"
            @change="checkbox"
          />
          <span class="checkmark"></span>
          <span>Без пересадок</span>
        </label>
      </li>
      <li class="tickets__filters_item">
        <label class="checkbox">
          <input
            value="OneTransfer"
            type="checkbox"
            v-model="filters"
            @change="checkbox"
          />
          <span class="checkmark"></span>
          <span>1 пересадка</span>
        </label>
      </li>
      <li class="tickets__filters_item">
        <label class="checkbox">
          <input
            value="TwoTransfer"
            type="checkbox"
            v-model="filters"
            @change="checkbox"
          />
          <span class="checkmark"></span>
          <span>2 пересадки</span>
        </label>
      </li>
      <li class="tickets__filters_item">
        <label class="checkbox">
          <input
            value="ThreeTransfer"
            type="checkbox"
            v-model="filters"
            @change="checkbox"
          />
          <span class="checkmark"></span>
          <span>3 пересадки</span>
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data: () => ({
    filters: [
      "All",
      "WithoutTransfer",
      "OneTransfer",
      "TwoTransfer",
      "ThreeTransfer",
    ],
  }),

  methods: {
    checkbox() {
      if (event.target.value === "All") {
        if (event.target.checked) {
          this.filters = [
            "All",
            "WithoutTransfer",
            "OneTransfer",
            "TwoTransfer",
            "ThreeTransfer",
          ]
        } else {
          this.filters = []
        }
      }

      if (this.filters.length === 4 && !this.filters.includes("All")) {
        this.filters.push("All")
      }
      if (this.filters.length < 5 && this.filters.includes("All")) {
        this.filters = this.filters.filter(value => value !== "All")
      }

      this.$emit("filters", this.filters)
    },
  },

  mounted() {
    this.$emit("filters", this.filters)
  }
}
</script>
