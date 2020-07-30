<template>
  <div class="tickets__ticket_list">
    <div class="tickets__ticket" v-for="(ticket, i) of tickets" :key="i">
      <div class="tickets__ticket_header">
        <div>
          {{
            ticket.price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ") + " Р"
          }}
        </div>
        <div class="tickets__ticket_img">
          <img :src="`https://pics.avs.io/99/36/${ticket.carrier}.png`" />
        </div>
      </div>
      <div class="tickets__ticket_routes">
        <div class="tickets__ticket_route">
          <div class="second-color">
            {{
              ticket.segments[0].origin + " - " + ticket.segments[0].destination
            }}
          </div>
          <div>
            {{
              new Date(ticket.segments[0].date).toLocaleTimeString([], {
                hour: "2-digit",
                minute: "2-digit",
              }) +
                " - " +
                getDestTime(
                  ticket.segments[0].date,
                  ticket.segments[0].duration
                )
            }}
          </div>
        </div>
        <div class="tickets__ticket_length">
          <div class="second-color">В ПУТИ</div>
          <div>
            {{
              Math.floor(ticket.segments[0].duration / 60) +
                "ч " +
                (ticket.segments[0].duration % 60) +
                "м"
            }}
          </div>
        </div>
        <div class="tickets__ticket_stops">
          <div class="second-color">
            {{
              !ticket.segments[0].stops.length
                ? "БЕЗ ПЕРЕСАДОК"
                : ticket.segments[0].stops.length === 1
                ? "1 ПЕРЕСАДКА"
                : `${ticket.segments[0].stops.length} ПЕРЕСАДКИ`
            }}
          </div>
          <div>{{ ticket.segments[0].stops.join(", ") }}</div>
        </div>
      </div>
      <div class="tickets__ticket_routes">
        <div class="tickets__ticket_route">
          <div class="second-color">
            {{
              ticket.segments[1].origin + " - " + ticket.segments[1].destination
            }}
          </div>
          <div>
            {{
              new Date(ticket.segments[1].date).toLocaleTimeString([], {
                hour: "2-digit",
                minute: "2-digit",
              }) +
                " - " +
                getDestTime(
                  ticket.segments[1].date,
                  ticket.segments[1].duration
                )
            }}
          </div>
        </div>
        <div class="tickets__ticket_length">
          <div class="second-color">В ПУТИ</div>
          <div>
            {{
              parseInt(ticket.segments[1].duration / 60) +
                "ч " +
                (ticket.segments[1].duration % 60) +
                "м"
            }}
          </div>
        </div>
        <div class="tickets__ticket_stops">
          <div class="second-color">
            {{
              !ticket.segments[1].stops.length
                ? "БЕЗ ПЕРЕСАДОК"
                : ticket.segments[1].stops.length === 1
                ? "1 ПЕРЕСАДКА"
                : `${ticket.segments[1].stops.length} ПЕРЕСАДКИ`
            }}
          </div>
          <div>{{ ticket.segments[1].stops.join(", ") }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["tickets"],

  methods: {
    getDestTime(date, duration) {
      const dateOne = new Date(date)
      const dateTwo = new Date(dateOne)
      dateTwo.setMinutes(dateOne.getMinutes() + duration)

      return dateTwo.toLocaleTimeString([], {
        hour: "2-digit",
        minute: "2-digit",
      })
    },
  },
}
</script>
