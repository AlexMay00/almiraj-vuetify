<template>
  <v-text-field v-model="search" clearable hide-details persistent-clear label="Search Recaps" prepend-inner-icon="mdi-magnify" variant="solo-filled">
  </v-text-field>
  <v-data-iterator
  :items="events"
  :search="search"
  >
    <template v-slot:default="{ items }">
      <v-timeline :side="mdAndUp ? null : 'end'">
        <v-timeline-item
          v-for="event in items"
          :key="event.raw.year"
          :dot-color="event.raw.color"
          :icon="event.raw.icon"
          :size="mdAndUp ? 'x-large' : 'xx-large'"
          :density="mdAndUp ? 'default' : 'compact'"
        >
          <template v-slot:opposite v-if="mdAndUp">
            <span>{{ event.raw.year }}</span>
          </template>
          <v-card class="elevation-2">
            <v-card-title class="text-h5">
              {{ event.raw.headline }}
            </v-card-title>
            <v-card-subtitle v-if="smAndDown">{{ event.raw.year }}</v-card-subtitle>
            <v-card-text style="white-space: pre-wrap;">
              <AvatarDialog v-if="event.raw.avatar" :n="event"></AvatarDialog>{{ event.raw.text }}
            </v-card-text>
            <audio v-if="event.raw.audio" oncontextmenu="return false;" controls controlslist="nodownload noplaybackrate" preload="none" :src="event.raw.audio" onplay="focus()"></audio>
          </v-card>
        </v-timeline-item>
      </v-timeline>
    </template>
  </v-data-iterator>
</template>

<script>
  export default {
    data: () => ({
      search:'',
      events: [
        {
          color: 'white',
          year: 'Jan 24, 2024',
          headline: 'Session -1',
          text: 'Mitch busted out his huge-ass map and we asked him questions about his 2+ year, 500+ city world.\n\n\n\n\nWe love Mitch.',
          icon: 'mdi-map-search-outline',
          avatar: '../session-1.jpg'
        },
        {
          color: 'green',
          year: 'Feb 7, 2024',
          headline: 'Session 0',
          text: 'We all met and discussed the cool plans we have. We decided against crunchy crits and agreed to not be assholes. No dice were rolled this day.\n\nNotable mentions:\n- Drinking potions is a Bonus Action\n- Administering potions to others is an action\n- Death saves will be secret\n- Tone is closer to HP than GOT\n- Strength can be used as intimidation (pending DM approval)\n- We all want solid RP moments\n- Icebreakers & fun facts!\n- Quick combat, be prepared!\n- Death is scary. Reviving PCs will be very hard. The closer we are to each other, the easier it will be.\n- Inspiration nominations! Inspiration tokens grant advantage.',
          icon: 'mdi-play-pause'
        },
        {
          color: 'orange',
          year: 'Feb 21, 2024',
          headline: 'Session 1',
          text: 'The adventure began with the six PCs meeting each other on a train named The Darning Needle, owned and operated by a Verdan man named Slake Toggins. Mr. Toggins provides free rides to Almiraj for students who have been newly accepted to the school as Ruby Basilisks.\n\nAfter some awkward introductions over explosive drinks from Bumper Nosedust, the group settled down for dinner with some of their new friends - Topin the dragonborn, Apple the kobold, and Aaron the elf.\n\nAs the rest of the group sleeps soundly, Dahlen is summoned in his dreams by a woman in a birdcage made of thorns. She tells Dahlen and the 19 other people she summoned “You\'ve all earned a fraction of my power, I\'m looking for 5 that deserve all of my power." The Darning Needle continues on toward Almiraj as we sleep, which is where we leave things until next time!',
          icon: 'mdi-train',
          audio:'../session1.ogg'
        },
        {
          color: 'indigo',
          year: 'Mar 6, 2024',
          headline: 'Session 2',
          text: 'Sleepy Sefy awoke to a chipper Chipp hovering over her and was soon introduced to Dashiel over waffles. Dave, Dahlen, and Selmenoch made their way to the bar cart for some morning Malort.\n\nAura-stunning arrows rained down on the Darning Needle as a strange man with a black & white checkered cape and a 20ft extendable wand summoned an army of cartoon-like chicken mounted bunnies. The party managed to defend the sleeping occupants and kill most of the bunnies, however, a few were missed in the chaos. The owlen guard was killed in the cargo train and several crates of pharmaceutical-grade potions were stolen.\n\nPerhaps worst of all, Cosmo was slain in battle.\n\nThe students spend the next hour cleaning up the mess, healing the wounded, and getting their story straight as they wait for the adults to wake up.',
          icon: 'mdi-rabbit',
          avatar: '../dead-cosmo.jpg',
          audio: '../hedidntmakeit.ogg'
        },
        {
          color: 'red',
          year: 'Mar 20, 2024',
          headline: 'Session 3',
          text: 'The Teenage Magic Nimble Troopers (patent pending) lick their wounds and wait for the adults to wake up to see how much trouble they\'re in.',
          icon: 'mdi-dice-d20'
        },
      ].reverse(),
    }),
  }
</script>

<script setup>
import { useDisplay } from 'vuetify'
import AvatarDialog from './AvatarDialog.vue';

  // Destructure only the keys you want to use
  const { smAndDown, mdAndUp } = useDisplay()
</script>

<style scoped>
audio {
  max-height: 42px;
  margin-left: 10px;
  margin-bottom: 10px;
}
</style>