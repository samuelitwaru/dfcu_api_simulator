<template>
  <q-page class="flex flex-center">
    <div class="q-pa-sm" style="min-width: 20rem">
      <div class="q-pb-md">
        <label class="text-subtitle1">API Base URL</label>
        <q-input v-model="apiURL" type="text" label="https://api.example.com" />
      </div>

      <div>
        <label class="text-subtitle1">Enter Account Numbers</label>
        <q-card class="my-card q-pa-md">
          <div>
            <q-input
              v-model="accNo1"
              type="text"
              label="Account Number 1"
              outlined
              dense
              class="q-mb-sm"
            />
            <q-input
              v-model="accNo2"
              type="text"
              label="Account Number 2"
              outlined
              dense
              class="q-mb-sm"
            />
            <q-input
              v-model="accNo3"
              type="text"
              label="Account Number 3"
              outlined
              dense
              class="q-mb-sm"
            />
            <q-input
              v-model="accNo4"
              type="text"
              label="Account Number 4"
              outlined
              dense
              class="q-mb-sm"
            />
            <q-input
              v-model="accNo5"
              type="text"
              label="Account Number 5"
              outlined
              dense
              class="q-mb-sm"
            />
          </div>
          <div align="right">
            <q-btn color="primary" label="RUN" @click="callAPI" />
          </div>
        </q-card>
      </div>

      <div class="q-py-sm">
        <!-- <q-card class="my-card q-pa-md"> -->
        <q-input
          v-model="responseData"
          type="textarea"
          label="Responses"
          outlined
        />
        <div align="right" v-if="responseData">
          <q-btn
            icon="download"
            color="primary"
            dense
            class="q-mt-sm"
            label="download response logs"
            @click="downloadResponse"
          />
        </div>

        <!-- </q-card> -->
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "IndexPage",
  data() {
    return {
      apiURL: "https://dfcu.pythonanywhere.com",
      // apiURL: "http://127.0.0.1:8000",
      accNo1: "1234567890",
      accNo2: "0987654321",
      accNo3: "2121212121",
      accNo4: "1991991991",
      accNo5: "1234554321",
      responseData: "",
    };
  },
  methods: {
    callAPI() {
      this.responseData = "";
      const accNos = [
        this.accNo1,
        this.accNo2,
        this.accNo3,
        this.accNo4,
        this.accNo5,
      ];

      const options = {
        baseURL: this.apiURL,
      };

      // return;
      for (let index = 0; index < accNos.length; index++) {
        const accNo = accNos[index];
        console.log(accNo);
        this.$api
          .post(`/api/`, { account_number: accNo }, options)
          .then((res) => {
            this.responseData += `Account Number: ${accNo} \n${JSON.stringify(
              res.data
            )} \n ------------------------------------------------\n`;
          })
          .catch((err) => {
            this.responseData += `Account Number: ${accNo} \n${err.message} \n------------------------------------------------\n`;
          });
      }
    },

    downloadResponse() {
      const options = {
        baseURL: this.apiURL,
      };
      this.$api
        .post(`/api/download-response`, { content: this.responseData }, options)
        .then((res) => {
          // this.fileURL = res.data.file_url;
          fetch(res.data.file_url)
            .then((response) => response.text())
            .then((text) => {
              const link = document.createElement("a");
              link.setAttribute(
                "href",
                "data:text/plain;charset=utf-8," + encodeURIComponent(text)
              );
              link.setAttribute("download", "response.txt");
              document.body.appendChild(link);
              link.click();
              document.body.removeChild(link);
            });
          // var tag = document.getElementById("fileURL");
          // tag.click();
        })
        .catch((err) => {
          console.log(err.message);
        });
    },
  },
});
</script>
