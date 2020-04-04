<template>
  <v-app>
    <div class="text-xs-center">
      <v-card>
        <v-card-title>Template Builder 2.0</v-card-title>
        <v-form>
          <v-card-text>
            <v-text-field
              v-model="title"
              :rules="this.max_length(32)"
              counter="32"
              placeholder="Toast Title"
            ></v-text-field>
            <v-switch @change="hasHero = !hasHero" :label="(hasHero)?'Custom Hero':'Default Hero'"></v-switch>
            <v-file-input
              label="hero image"
              filled
              prepend-icon="mdi-camera"
              v-model="hero"
              @change="createImage('hero',hero)"
              v-if="hasHero"
            ></v-file-input>
            <v-switch
              @change="hasPicture = !hasPicture"
              :label="(hasPicture)?'Add Picture':'No Picture'"
            ></v-switch>
            <v-file-input
              label="picture image"
              filled
              prepend-icon="mdi-camera"
              v-model="picture"
              @change="createImage('picture', picture)"
              v-if="hasPicture"
            ></v-file-input>
            <v-textarea
              v-model="body"
              :rules="this.max_length(256)"
              counter="256"
              placeholder="Body Content"
            ></v-textarea>
            <v-text-field
              v-model="positive"
              :rules="this.max_length(16)"
              counter="16"
              placeholder="Positive Button"
            ></v-text-field>
            <v-text-field
              v-model="negative"
              :rules="this.max_length(16)"
              counter="16"
              placeholder="Negative Button"
            ></v-text-field>
            <v-btn color="primary" dark v-on:click="() => {dialog=true;click++}">Windows Preview</v-btn>
            <v-btn color="primary" :disabled="true">Email Preview</v-btn>
            <v-btn color="primary" :disabled="true">Mac Preview</v-btn>
            <v-btn color="primary" :disabled="true">Linux Preview</v-btn>
          </v-card-text>
        </v-form>
      </v-card>
    </div>
    <v-dialog v-model="dialog" max-width="400" style="overflow:hidden;">
      <v-card style="overflow:hidden;">
        <v-card-title class="headline">Windows 10 Preview</v-card-title>
        <v-card-text>
          <transition name="slide-fade" appear>
            <div v-if="dialog" style="position: relative; width: 360px; height:auto;">
              <div
                style="position: relative; background-color:red; width:360px; height:175px; bottom: 0px;"
              >
                <img style="width: 360px; height: 175px;" :src="displayhero">
              </div>
              <div
                style="position: relative; background: rgb(31,31,31); width: 360px; min-height: 150px;"
              >
                <div
                  style="position: relative; padding-left: 15px; color: white; font-size:15px; padding-top:15px; font-family: 'Segoe UI', Georgia, Serif;"
                >
                  <b>{{title}}</b>
                  <div
                    style="position: relative; display: inline-block; color: rgb(165, 165, 165); font-size:15px; padding-top:10px; font-family: 'Segoe UI', Georgia, Serif; max-width: 355px; width:100%;"
                  >
                    <p style="line-height:20px;">{{body}}</p>
                  </div>
                  <div
                    v-if="hasPicture"
                    style="position: relative; width:100%; padding-left:20%; margin-top:30px; height:150px;"
                  >
                    <img style="max-width:100%;max-height:100%;" :src="displaypicture">
                  </div>
                </div>
                <div style="position: relative; width:100%; height:65px;"></div>
                <div
                  style="position: absolute; text-align:center; color: white; bottom: 15px; width:92%; right:4%; left:4%; height: 35px; display:flex; font-size:15px; font-family: 'Segoe UI', Georgia, Serif;"
                >
                  <div
                    style="position: relative; display:inline-block; background: rgb(76,76,76); color:white; width: 49%; height: 30px;"
                  >
                    <v-layout align-center justify-center column fill-height>
                      <v-flex row align-center>{{positive}}</v-flex>
                    </v-layout>
                  </div>
                  <div
                    class="auto-mx"
                    style="position: relative; display:inline-block; background: rgb(76,76,76); color:white; width: 49%; height: 30px; margin-left:2%;"
                  >
                    <v-layout align-center justify-center column fill-height>
                      <v-flex row align-center>{{negative}}</v-flex>
                    </v-layout>
                  </div>
                </div>
              </div>
            </div>
          </transition>
        </v-card-text>

        <v-card-actions>
          <v-btn color="green darken-1" text @click="dialog = false; triggered=false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      click: 0,
      triggered: true,
      hero: null,
      picture: null,
      title: null,
      body: null,
      positive: null,
      negative: null,
      displayhero: null,
      displaypicture: null,
      hasHero: false,
      hasPicture: false
    };
  },
  computed: {
    show() {
      return this.dialog;
    }
  },
  methods: {
    max_length(count) {
      return [v => v.length <= count || `Maximum of ${count} exceeded`];
    },
    onFileChange(item) {
      var files = this.hero;
      this.createImage(item, files);
    },
    createImage(saveTo, readFrom) {
      var reader = new FileReader();

      reader.onload = e => {
        if (saveTo === "hero") {
          this.displayhero = e.target.result;
        } else {
          this.displaypicture = e.target.result;
        }
      };
      reader.readAsDataURL(readFrom);
    },
    removeImage: function(item) {
      item.image = false;
    }
  }
};
</script>

<style>
/* Enter and leave animations can use different */
/* durations and timing functions.              */
.slide-fade-enter-active {
  transition: all 0.8s ease;
}
.slide-fade-leave-active {
  transition: all 0.1s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(100vw) scale(0.5);
  opacity: 0;
}

/* translateX(-100vw) */
</style>