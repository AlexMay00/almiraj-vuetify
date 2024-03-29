<template>
  <v-text-field v-model="search" clearable hide-details persistent-clear label="Search for anything" prepend-inner-icon="mdi-magnify" variant="solo-filled">
  </v-text-field>
  <Filter @optionChanged="optionChanged($event)">
  </Filter>
  <v-data-iterator
    :items="currentOption === 'all' ? nouns : nouns.filter(noun => noun.type === currentOption || noun.subtype == currentOption)"
    :items-per-page="-1"
    :search="search"
  >
    <template v-slot:default="{ items }">
      <v-row>
        <v-col v-for="n in items" :key="n" xs="12" sm="12" md="6">
          <NounCard :item="n">
          </NounCard>
        </v-col>
      </v-row>
    </template>
  </v-data-iterator>
</template>

<script>
  export default {
    data: () => ({
      currentOption: 'all',
      search: '',
      nouns: [
        {
          title: 'Dave Nornackle',
          subtitle: 'Goliath Barbarian, Burrowburgh',
          avatar: '../dave.jpeg',
          text: 'David Nornackle is a goliath barbarian who hails from Burrowburgh, a small village outside of Alpharen in the Land of the Small. Dave was adopted by two gnomes, Pabavyc and Lorininn Nornackle, when he was a baby - a \"huge fuckin\' baby\" as a family friend once eloquently stated.',
          info: 'Dave received his scarab for saving his village from a giant toad attack.',
          tags: ['PC', 'Barbarian', 'Goliath', 'Burrowburgh', 'Ruby Basilisk', 'Student'],
          associations: ['Almiraj', 'Pabavyc Nornackle', 'Lorininn Nornackle', 'Sunbeam Wonderbite', 'Timber Wonderbite', 'Land of the Small'],
          voice: '../dave.ogg',
          icon: 'mdi-account',
          type: 'character',
          subtype: 'pc',
          order: 10
        },
        {
          title: 'Chipp  Dawnglow',
          subtitle: 'Aasimar Monk, Isla Relampalgo',
          avatar: '../chipp.jpeg',
          text: 'A lightly tan skinned Aasimar with three pairs of seraphim-like wings.',
          info: 'Chipp received her scarab when she sneezed real hard.',
          tags: ['PC', 'Aasimar', 'Monk', 'Student', 'Ruby Basilisk'],
          associations: ['Almiraj'],
          icon: 'mdi-account',
          type: 'character',
          subtype: 'pc',
          order: 20
        },
        {
          title: 'Dahlen Mirkwood',
          subtitle: 'Half Elf Warlock, Aplace',
          avatar: '../dahlen.jpeg',
          text: 'A 16 year old boy with shiny dark brown hair, bright green eyes, pointy ears, and somewhat elven features. Wearing a kind-of-too-big ruby fancy shirt that looks borrowed. Always carrying books.',
          info: 'Dahlen is a shy fellow, but deeply curious about his surroundings. He has largely existed in the shadow of his (pureblood) elven grandfather, who raised him. Dahlen is not very well traveled, and has never left the comfort of his home-forest prior to this adventure. He received his scarab when he found a thing and accidentally killed a guy. I wasn\'t supposed to say that.\n\n"Elmon delmon. Teamwork makes the dream work!"',
          tags: ['PC', 'Half Elf', 'Warlock', 'Student', 'Ruby Basilisk'],
          associations: ['Almiraj'],
          icon: 'mdi-account',
          type: 'character',
          subtype: 'pc',
          order: 30
        },
        {
          title: 'Dashiel Graves',
          subtitle: 'Variant Human Bard, Areatown',
          avatar: '../dashiel.jpeg',
          text: 'With a jaw approved by the CW, Dashiel is as handsome as they come. Brown hair, brown eyes, paler skin with a slight tan, 6\'4" tall, lean, swimmers build. Dashiel is wearing a dark gray tunic, a red scarf, and drapery around his waist.',
          info: 'Dashiel received his scarab after completing an amazing performance.',
          tags: ['PC', 'Human', 'Bard', 'Student', 'Ruby Basilisk'],
          associations: ['Almiraj'],
          icon: 'mdi-account',
          type: 'character',
          subtype: 'pc',
          order: 40
        },
        {
          title: 'Sefy Hart',
          subtitle: 'Satyr Cleric, Trueheart Forest',
          avatar: '../sefy.jpeg',
          text: 'A satyr girl with curly gray hair, large golden brown ram horns that curl backwards, and white fur with random patches of gray. Around her neck she wears a faded red blanket as a scarf, and a carved pendant of Ehlonna. Her set of armor appears to be pieced together, and her earrings are unmatched.',
          info: 'Sefy\'s scarab just sort of showed up one day.',
          tags: ['PC', 'Satyr', 'Cleric', 'Student', 'Ruby Basilisk'],
          associations: ['Almiraj', 'Valon'],
          icon: 'mdi-account',
          type: 'character',
          subtype: 'pc',
          order: 50
        },
        {
          title: 'Selmenoch Lowenoch',
          subtitle: 'Fairy Fighter, Feywild',
          avatar: '../sel.jpeg',
          text: 'A small fairy who very recently arrived from the Feywild.',
          info: 'Roughly 3 feet tall, 30-35 lbs, with 90s JRPG spiky red hair. Wearing chainmail and donning a shield. Selmenoch dresses primarily in autumn colors like gold, brass, and orange with purple accents. His shield is decorated with a paw, crossed with a lance. He rides atop a larger than average corgi named Cosmo.\n\nSelmenoch is a well trained dragoon who served in the first battalion calvary under the leadership of Lady Ophelia Cogwin. He was part of the Corgi Battalion!\n\n\n"Nothing bad ever happens in the feywild. You\'re good."',
          tags: ['PC', 'Fairy', 'Fighter', 'Feywild', 'Student', 'Ruby Basilisk'],
          associations: ['Almiraj', 'Cosmo'],
          icon: 'mdi-account',
          type: 'character',
          subtype: 'pc',
          order: 60
        },
        {
          title: 'Alpharen',
          subtitle: 'City in the Land of the Small',
          avatar: '../alpha-a-circle-outline.svg',
          text: 'A city of gnomes that is known for its wild parties and upbeat attitude. Famously celebrate over 200 different holidays a year, most of the towns income comes from tourists that come to have a good time.',
          info: null,
          tags: ['Land of the Small', 'City', 'Gnome', 'Halfling', 'Fairy'],
          associations: ['Burrowburgh'],
          icon: 'mdi-city',
          type: 'location',
          subtype: 'city',
          order: 70
        },
        {
          title: 'Land of the Small',
          subtitle: 'Western Coast',
          avatar: '../los.jpg',
          text: 'A west-coast nation consisting of mostly small-statured races such as Gnomes, Halflings, Fairies, and Grung.',
          info: 'Also known as the Small-Lands, The Land of the Small is a region that used to be a giant forest but over the years has been cut back into Groves as more and more towns and cities grew inside it. They are home to gnomes and halflings mostly but grung and fairy both also live within the safety of the woods. The small lands have no Overarching ruler, each city takes care of itself. Because of this quite a few land disputes break out and the cities are constantly fighting small skirmishes for expansion.',
          tags: ['Nation', 'Gnome', 'Halfling', 'Fairy'],
          associations: ['Burrowburgh', 'Alpharen', 'Dave Nornackle', 'Selmenoch Lowenoch', 'Sunbeam Wonderbite', 'Timber Wonderbite', 'n\'Rob', 'Pabavyc Nornackle', 'Lorininn Nornackle'],
          icon: 'mdi-earth',
          type: 'location',
          subtype: 'nation',
          order: 80
        },
        {
          title: 'Burrowburgh',
          subtitle: 'Town near Alpharen - Land of the Small',
          avatar: '../burrowburgh.jpg',
          text: 'Dave\'s hometown. Once attacked by a giant toad. Population mostly consists of halflings, gnomes, fairies, and grung.',
          info: null,
          tags: ['Town', 'Gnome', 'Halfling', 'Fairy'],
          associations: ['Dave Nornackle', 'Pabavyc Nornackle', 'Lorininn Nornackle', 'Sunbeam Wonderbite', 'Timber Wonderbite'],
          icon: 'mdi-home-city',
          type: 'location',
          subtype: 'town',
          order: 90
        },
        {
          title: 'Cursed Weapon',
          subtitle: '+2 to hit, -2 to your soul',
          avatar: '../cursed.jpg',
          text: 'IT IS I, SKRTTATTEN OF THE UNDEREARTH. YOU WILL WIELD ME AND ALL WILL SUFFER.',
          info: 'WISDOM SAVING THROW!\nHA YOU HAVE FAILED\nYOU CANNOT RESIST SKRTTATTEN',
          tags: ['Cursed', 'Weapon', 'Item', 'Magic', 'Yikes'],
          associations: ['Slashing Damage', 'Fire Damage'],
          icon: 'mdi-sword-cross',
          type: 'item',
          subtype: 'weapon',
          order: 100
        },
        {
          title: 'Sunbeam Wonderbite',
          subtitle: 'Fairy Cleric, Burrowburgh',
          avatar: '../sunbeam.jpg',
          text: 'Sunbeam is the best friend of Dave Nornackle and the daughter of Timber Wonderbite. One of her wings is significantly smaller than the other, making it difficult for her to fly. She was recruited by Almiraj for her healing prowess.',
          info: 'She wears a pendant with a symbol of Lurue, a silvery moon, around her neck.',
          tags: ['NPC', 'Fairy', 'Cleric', 'Burrowburgh', 'Ruby Basilisk'],
          associations: ['Almiraj','Timber Wonderbite', 'Dave Nornackle', 'Land of the Small'],
          voice: '../sunbeam.ogg',
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 110
        },
        {
          title: 'Timber Wonderbite',
          subtitle: 'Fairy, Burrowburgh',
          avatar: '../carl.jpg',
          text: 'Timber Wonderbite is a crude speaking, greasy, grumpy, slightly-drunk-all-the-time fairy. He is the father of Sunbeam Wonderbite and a family friend of the Nornackle\'s.',
          info: '\"Holy shit, that\'s a huge fuckin\' baby!\"',
          tags: ['NPC', 'Fairy', 'Burrowburgh'],
          associations: ['Sunbeam Wonderbite', 'Dave Nornackle', 'Land of the Small', 'Pabavyc Nornackle', 'Lorininn Nornackle'],
          voice: '../carl.ogg',
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 120
        },
        {
          title: 'The Radiant Lands',
          subtitle: 'Eastern, Isla Relampalgo',
          avatar: '../radiant.jpg',
          text: 'The northern nation of the Isla Relampalgo, located on the far east of the continent.',
          info: null,
          tags: ['Nation', 'Aasimir'],
          associations: ['Chipp Dawnglow'],
          icon: 'mdi-earth',
          type: 'location',
          subtype: 'nation',
          order: 130
        },
        {
          title: 'Cosmo',
          subtitle: 'Corgi',
          avatar: '../cosmo.jpg',
          text: 'Selmenoch\'s Corgi mount. A fierce warrior in the Feywild, and Sel\'s most trusted confidant. Known to bite a finger off here or there, especially if approached too quickly.',
          info: 'Bark.',
          tags: ['PC', 'Mount', 'Selmenoch Lowenoch', 'Dog'],
          associations: ['Feywild'],
          voice: null,
          //icon: 'mdi-dog-side',
          icon: 'mdi-skull-crossbones',
          type: 'character',
          subtype: 'pc',
          order: 140
        },
        {
          title: 'Torm',
          subtitle: 'Western Continent, City in Dyvesta',
          avatar: '../alpha-t-circle-outline.svg',
          text: 'An industrial city in the nation of Dyvesta, where the campaign began.',
          info: null,
          tags: ['Industrial', 'Train Stop'],
          associations: null,
          voice: null,
          icon: 'mdi-city',
          type: 'location',
          subtype: 'city',
          order: 150
        },
        {
          title: 'Pepper',
          subtitle: 'Earth Genasi',
          avatar: '../pepper.jpg',
          text: 'An employee aboard the Darning Needle, and assistant to the train\'s owner, Mr. Toggins.',
          info: 'Pepper has strong, almost masculine features. Her skin is a pale gray, with a stone-like edifice to it containing sparkly freckles. Her uniform consists of a blue/gray pair of slacks, a white ruffled shirt, and a jacket that matches her pants. Dark charcoal hair pulled back into a bun of thick dreadlocks.',
          tags: ['NPC', 'Earth Genasi', 'Darning Needle'],
          associations: ['Toggins'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 160
        },
        {
          title: 'Slake Toggins',
          subtitle: 'Verdan',
          avatar: '../toggins.jpg',
          text: 'Owner of the Darning Needle, and an Almiraj alumni. Mr. Toggins was a Ruby Basilisk in his adventuring days, and now gives free rides to freshmen.',
          info: "BASILISKS!!!",
          tags: ['NPC', 'Earth Genasi', 'Darning Needle'],
          associations: ['Pepper', 'Ruby Basilisk'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 170
        },
        {
          title: 'Nelaryen Mirkwood',
          subtitle: 'Elf, Dahlen\'s Grandpa',
          avatar: '../grandpa.jpg',
          text: 'Grandfather of Dahlen Mirkwood. Proud (by extension) of Dahlen for becoming a Ruby Basilisk, just as he had been. Seems to resent Dahlen for only being a half elf.',
          info: '"I find the lack of trees...     upsetting."',
          tags: ['NPC', 'Elf', 'Dahlen Mirkwood','Boomer'],
          associations: ['Ruby Basilisk'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 180
        },
        {
          title: 'Bumper Nosedust',
          subtitle: 'Human, "Bartender"',
          avatar: '../bumpy.jpg',
          text: 'The animated, over the top mixologist working in the bar car of the Darning Needle. Slicked back hair, wearing a blue/gray uniform.',
          info: '"What can I get ya, boy?"',
          tags: ['NPC', 'Darning Needle', 'Bartender'],
          associations: ['Toggins', 'Pepper'],
          voice: '../bumper.ogg',
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 190
        },
        {
          title: 'Bird Cage Lady',
          subtitle: '???, Dahlen\'s Patron',
          avatar: '../patron.jpg',
          text: 'Summoned Dahlen and 19 others to a shared dream where she said "You\'ve all earned a fraction of my power, I\'m looking for 5 that deserve all of my power. Be bold mortals. Be adventurous, be ruthless. I\'ll be watching. You don\'t want to be one of the losers."',
          info: '"Don\'t be boring."',
          tags: ['NPC', 'Dahlen Mirkwood', 'Patron'],
          associations: [],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 200
        },
        {
          title: 'Aaron Meltlake',
          subtitle: 'Elf, Fighter',
          avatar: '../aaron.jpg',
          text: 'Shy elf fighter we met on the train. Wearing oversized chain mail with a sword and board.',
          info: 'Aaron received his scarab after his aunt FORCED HIM TO DESTROY A BRONZE DUNGEON CRYSTAL BY HIMSELF.\n\nAaron is from The Prime Island, which is on the southwest of the continent.\n\nAaron\'s aunt teaches offensive magic at Almiraj.\n\nAaron has somehow accidentally drank ink several times. He also lacks self confidence.',
          tags: ['NPC', 'Student', 'Ruby Basilisk'],
          associations: ['Almiraj'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 210
        },
        {
          title: 'Apple',
          subtitle: 'Kobold, Wizard',
          avatar: '../apple-min.jpg',
          text: 'A small, talkative kobold we met on the train ride to Almiraj. She quickly became good friends with Sunbeam.',
          info: null,
          tags: ['NPC', 'Student', 'Ruby Basilisk'],
          associations: ['Almiraj'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 220
        },
        {
          title: 'Topin',
          subtitle: 'Dragonborn, Monk',
          avatar: '../topin-min.jpg',
          text: 'A very large dragonborn with black scales we met on the train ride to Almiraj.',
          info: 'Topin was sad when Dave said that Dahlen was his biggest friend.',
          tags: ['NPC', 'Student', 'Ruby Basilisk'],
          associations: ['Almiraj'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 230
        },
        {
          title: 'Redd',
          subtitle: 'Dwarf, Fighter (Fuckist)',
          avatar: '../redd-min.jpg',
          text: 'Redd is a dwarven fighter we met on the train headed to Almiraj. He seems dumb.',
          info: 'Redd got his scarab after making passionate love to a shifter who was showing as a half-armadillo. The scarab was on the window the following day. He is considering a class change to bard after talking with Dashiel about his fucking prowess.',
          tags: ['NPC', 'Student', 'Ruby Basilisk'],
          associations: ['Almiraj'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 240
        },
        {
          title: 'Thri Kreen Kid',
          subtitle: 'Thri Kreen, ???',
          avatar: '../thrikreen.jpg',
          text: 'Sips juice. Doesn\'t piss.',
          info: 'Communicates telepathically. Cut off her hand, spit on it, and it morphed into a hammer. She refers to herself in the third person and is just taking the train home (not a student).',
          tags: ['NPC'],
          associations: ['The Darning Needle'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 250
        },
        {
          title: 'Valon',
          subtitle: 'Eastern Coast',
          avatar: '../valon.jpg',
          text: 'A nation located on the eastern coast of Manta, west of the Radiant Lands.',
          info: null,
          tags: ['Nation', 'Gnome', 'Satyr', 'Trueheart Forest'],
          associations: ['Sefy Hart'],
          icon: 'mdi-earth',
          type: 'location',
          subtype: 'nation',
          order: 260
        },
        {
          title: 'Trueheart Forest',
          subtitle: 'A forest in southern Valon',
          avatar: '../trueheart-forest.jpg',
          text: 'Sefy\'s hometown.',
          info: null,
          tags: ['Nation', 'Gnome', 'Satyr', 'Trueheart Forest'],
          associations: ['Sefy Hart'],
          icon: 'mdi-home-city',
          type: 'location',
          subtype: 'town',
          order: 270
        },
        {
          title: 'Mary',
          subtitle: 'Human, Warlok',
          avatar: '../mary.jpg',
          text: 'Mary is a loud witch, who overshares and oversleeps.',
          info: 'Her patron is a genie that lives in a weird cottage does all sorts of crazy shit.',
          tags: ['NPC', 'Student', 'Ruby Basilisk'],
          associations: ['The Darning Needle'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 280
        },
        {
          title: 'Gregory',
          subtitle: 'Human, Barbarian',
          avatar: '../greg-min.jpg',
          text: 'Gregory is a guard on The Darning Needle. A very large man, who is a bit older and starting to gray. He wields a large sledgehammer and gave Dave some barbarian advice on the train ride to school.',
          info: '"People think we\'re brash and quick to anger... which we are... but..."',
          tags: ['NPC', 'Guard'],
          associations: ['The Darning Needle'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 290
        },
        {
          title: 'Top Hat Man',
          subtitle: '???, ???',
          avatar: '../villain-min.jpg',
          text: 'This bunnies-riding-chickens summoning man stole Dovin\'s tears from the Darning Needle. Gold level aura. He is wearing a white mask with a red smile.',
          info: null,
          tags: ['NPC', 'Villain'],
          associations: ['Chickens', 'Bunnies', 'BBEG'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 300
        },
        {
          title: 'Aunt Meltlake',
          subtitle: 'Elf, ???',
          avatar: '../aunt.jpg',
          text: 'The strongest fire made in the world. Offensive magic teacher at Almiraj, and Aaron\'s aunt. She forced Aaron to destroy a dungeon crystal on his own, and Aaron thought that was preferable to dealing with her.',
          info: 'Ms. Meltlake refused to help defend the train because the top hat man was a much lower aura level than she was. She refused to disclose her aura ranking, but it is higher than platinum.\n\nShe states that her attacking someone is essentially an act of war. She is wearing a marker of the Primes (Prime Island).',
          tags: ['NPC', 'Professor', 'Almiraj'],
          associations: ['Aaron Meltlake'],
          voice: null,
          icon: 'mdi-account-outline',
          type: 'character',
          subtype: 'npc',
          order: 310
        },
        {
          title: 'Dovin\'s Tears',
          subtitle: 'Powerful healing potion',
          avatar: '../dovins-tears.jpg',
          text: 'A staple item of Zephyr industries. This high-end healing potion is very powerful, and very valuable.',
          info: 'A Potion of HEAL. Zephyr Industries owns all of the snakes that can produce the potion.',
          tags: ['Healing', 'Potion', 'Item', 'Magic'],
          associations: ['Dovin', 'Zephyr Industries'],
          icon: 'mdi-bottle-tonic-plus',
          type: 'item',
          subtype: 'consumable',
          order: 320
        },
        {
          title: 'Arrows of Aura Sleep',
          subtitle: 'Inactive (5)',
          avatar: '../arrow.jpg',
          text: 'Arrows we collected after they were shot by Top Hat Man into the Darning Needle\'s various cars. They are currently inactive but can be reactivated with strong magic. Very valuable.',
          info: null,
          tags: ['Arrow', 'Aura', 'Item', 'Magic', 'Sleep'],
          associations: ['Top Hat Man'],
          icon: 'mdi-arrow-projectile-multiple',
          type: 'item',
          subtype: 'weapon',
          order: 330
        },
      ].sort(function(a, b){return b.order - a.order})
    }),
    methods: {
      optionChanged (option) {
        this.currentOption = option
      }
  }
}
</script>