<template>
  <v-container id="shortner">
    <div id="title">
      {{ title }}
    </div>
    <v-text-field
      v-model="inputValue"
      solo
      label="Enter URL"
      clearable
    ></v-text-field>
    <v-bottom-sheet v-model="sheet1" persistent>
      <template v-slot:activator="{ on }">
        <v-btn color="green" @click="hashGenerator()" dark v-on="on">
          Generate Short URL
        </v-btn>
      </template>
      <v-sheet class="text-center" height="200px">
        <v-btn class="mt-6" text color="error" @click="sheet1 = !sheet1"
          >close</v-btn
        >
        <div id="myText" @click="copyToclipBoard()" class="py-3">
          {{ shortUrl }}
        </div>
      </v-sheet>
    </v-bottom-sheet>
    <br />
    <br />
    <v-text-field
      v-model="shortUrlInput"
      solo
      label="Enter ShortURL to find Orginal URL"
      clearable
    ></v-text-field>
    <v-bottom-sheet v-model="sheet2" persistent>
      <template v-slot:activator="{ on }">
        <v-btn color="green" @click="orginalURL()" dark v-on="on">
          Find URL
        </v-btn>
      </template>
      <v-sheet class="text-center" height="200px">
        <v-btn class="mt-6" text color="error" @click="sheet2 = !sheet2"
          >close</v-btn
        >
        <div id="myText" @click="copyToclipBoard()" class="py-3">
          {{ orgUrl }}
        </div>
      </v-sheet>
    </v-bottom-sheet>
  </v-container>
</template>

<script>
let urlDB = {};
export default {
  name: "Shortner",
  props: {
    title: String
  },
  data: () => ({
    sheet1: false,
    sheet2: false,
    inputValue: "",
    shortUrl: "",
    shortUrlInput: "",
    orgUrl: ""
  }),
  methods: {
    hashGenerator() {
      this.shortUrl = "";
      let hashCode = "";
      if (this.inputValue != "") {
        let protocolOk =
          this.inputValue.startsWith("http://") ||
          this.inputValue.startsWith("https://") ||
          this.inputValue.startsWith("ftp://");
        let url = this.inputValue;

        if (!protocolOk) {
          url = "http://" + this.inputValue;
        }

        const possibleChar =
          "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

        for (let i = 0; i < 5; i++) {
          hashCode += possibleChar.charAt(Math.random() * possibleChar.length);
        }
        urlDB[hashCode] = url;
        this.shortUrl = this.genUrl(hashCode);
        this.inputValue = "";
      } else {
        this.shortUrl = "";
      }
    },
    genUrl(hashCode) {
      return "http://shurl.ir/" + hashCode;
    },
    orginalURL() {
      let key;
      if (this.shortUrlInput.startsWith("http://shurl.ir/")) {
        key = this.shortUrlInput.slice(16);
      } else if (this.shortUrlInput.startsWith("shurl.ir/")) {
        key = this.shortUrlInput.slice(9);
      }
      this.orgUrl = urlDB[key];
      this.shortUrlInput = "";
    },
    copyToclipBoard() {
      const copyText = document.getElementById("myText").textContent;
      navigator.clipboard.writeText(copyText.trim());
      console.log(copyText);
    }
  }
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css?family=Pacifico&display=swap");
#shortner {
  display: flex;
  flex-direction: column;

  #title {
    font-size: 50px;
    font-weight: 600;
    color: #ffffff;
    text-align: center;
    font-family: "Pacifico", cursive;
  }
}
</style>
