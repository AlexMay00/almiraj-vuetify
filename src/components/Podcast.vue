<template>
  <v-text-field v-model="search" clearable hide-details persistent-clear label="Search Episodes" prepend-inner-icon="mdi-magnify" variant="solo-filled">
  </v-text-field>
  <v-data-iterator
  :items="events"
  :items-per-page="-1"
  :search="search"
  >
    <template v-slot:default="{ items }">
      <v-list lines="three">
        <v-list-item
          v-for="n in items"
          :key="n.raw.title"
        >
        <template v-slot:prepend><v-icon :icon="n.raw.icon" :color="n.raw.color"></v-icon></template>
          <v-row>
            <v-col>
              <v-list-item-title>{{ n.raw.title }}</v-list-item-title>
              <v-list-item-subtitle style="word-break: break-word;">{{ n.raw.description }}</v-list-item-subtitle>
            </v-col>
            <v-col>
              <audio id="audio" v-if="n.raw.file" oncontextmenu="return false;" controls controlslist="nodownload noplaybackrate" preload="none" :src="n.raw.file" onplay="focus()"></audio>
            </v-col>
          </v-row>
          <v-divider></v-divider>
        </v-list-item>
      </v-list>
    </template>
  </v-data-iterator>  
</template>

<script>
  export default {
    data: () => ({
      search: '',
      events: [
        {
          icon: 'mdi-podcast',
          color: 'red',
          title: 'Fate\'s Needle - Episode 1',
          description: 'Recapping the first year of the campaign.',
          file: 'podcast/fnep1.mp3',
        },
        {
          icon: 'mdi-podcast',
          color: 'red',
          title: 'Fate\'s Needle - Episode 2',
          description: 'Session 20: Carolyn\'s kidnapping & Dahlen\'s past.',
          file: 'podcast/fnep2.mp3',
        },
        {
          icon: 'mdi-podcast',
          color: 'red',
          title: 'Fate\'s Needle - Episode 3',
          description: 'Sessions 21-23: \'Teamwork and snacks save the day.\'',
          file: 'podcast/fnep3.mp3',
        },
        {
          icon: 'mdi-podcast',
          color: 'red',
          title: 'Bonus Episode! - Dave Nornackle',
          description: 'What is Dave\'s Story so far?',
          file: 'podcast/bonusDave.mp3',
        },
        {
          icon: 'mdi-podcast',
          color: 'red',
          title: 'Fate\'s Needle - Episode 4',
          description: 'Sessions 24-26: \'Dashiel sings, Dahlen shines, and Sefy speaks her mind.\'',
          file: 'podcast/fnep4.mp3',
        },
      ].sort(function(a, b){return a.title - b.title}),
    })
  }
</script>