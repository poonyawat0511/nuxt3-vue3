<template>
  <div>
    <h1>Google Calendar</h1>
    <button v-if="!isSignedIn" @click="signIn">Sign In</button>
    <button v-else @click="signOut">Sign Out</button>
    <div v-if="isSignedIn">
      <h2>Upcoming Events</h2>
      <ul>
        <li v-for="event in events" :key="event.id">
          {{ event.summary }} - {{ event.start.dateTime || event.start.date }}
        </li>
      </ul>
    </div>
  </div>
</template>
  
  <script>
import { useGoogleAPI } from "vue-google-api";

export default {
  data() {
    return {
      isSignedIn: false,
      events: [],
    };
  },
  async mounted() {
    await this.$gapi.load("client:auth2");
    await this.$gapi.client.init({
      apiKey: "<YOUR_API_KEY>",
      clientId: "<YOUR_CLIENT_ID>",
      discoveryDocs: [
        "https://www.googleapis.com/discovery/v1/apis/calendar/v3/rest",
      ],
      scope: "https://www.googleapis.com/auth/calendar.readonly",
    });
    this.isSignedIn = this.$gapi.auth2.getAuthInstance().isSignedIn.get();
    if (this.isSignedIn) {
      this.loadEvents();
    }
  },
  methods: {
    async signIn() {
      await this.$gapi.auth2.getAuthInstance().signIn();
      this.isSignedIn = true;
      this.loadEvents();
    },
    async signOut() {
      await this.$gapi.auth2.getAuthInstance().signOut();
      this.isSignedIn = false;
    },
    async loadEvents() {
      const response = await this.$gapi.client.calendar.events.list({
        calendarId: "primary",
        timeMin: new Date().toISOString(),
        showDeleted: false,
        singleEvents: true,
        maxResults: 10,
        orderBy: "startTime",
      });
      this.events = response.result.items;
    },
  },
};
</script>
  