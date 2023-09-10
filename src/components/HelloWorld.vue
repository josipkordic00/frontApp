<template>
  <v-app id="inspire">
    <NavBar />
    <div style="padding: 100px">
      Novo pitanje:
      <v-text-field
        v-model="pitanje"
        label="Pitanje"
        hide-details="auto"
      ></v-text-field>
      <v-text-field v-model="odgovor" label="Odgovor"></v-text-field>
      <button
        @click="insertQuestion()"
        style="
          background-color: #333;
          border-radius: 5px;
          margin: 3px;
          color: #fff;
          border-color: #333;
          padding: 10px 20px;
          font-size: 16px;
          box-shadow: none;
        "
      >
        Insert
      </button>
    </div>
    <div class="pitanja" style="padding: 50px">
      <h3 style="margin-bottom: 30px">Odgovori ucenika:</h3>
      <ul>
        <li
          v-for="question in questions"
          :key="question.id"
          style="border-bottom: 1px solid #333"
        >
          <div
            style="
              display: flex;
              justify-content: space-between;
              align-items: center;
            "
          >
            <div>
              <p>Pitanje: {{ question.pitanje }}</p>
              <p>Odgovor: {{ question.odgovor }}</p>
            </div>
            <div>
              <button
                @click="deleteQuestion(question.id)"
                style="
                  background-color: #333;
                  color: #fff;
                  border-color: #333;
                  border-radius: 5px;
                  padding: 10px 20px;
                  font-size: 16px;
                  box-shadow: none;
                "
              >
                Delete
              </button>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </v-app>
</template>

<script>
import axiosInstance from "../axiosInstance.js";
export default {
  data() {
    return {
      questions: [],
      pitanje: "",
      odgovor: "",
    };
  },
  created() {
    this.fetchData();
  },

  methods: {
    fetchData() {
      axiosInstance
        .get("/pitanja/dohvati")
        .then((response) => {
          this.questions = response.data;
          console.log(this.questions);
        })
        .catch((error) => {
          // Handle the error
          console.error(error);
        });
    },
    insertQuestion() {
      const newQuestion = {
        pitanje: this.pitanje,
        odgovor: this.odgovor,
      };
      axiosInstance
        .post("/pitanja/dodaj", newQuestion)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          // Handle the error
          console.error(error);
        });
      setTimeout(() => {
        window.location.reload();
      }, 200);
    },
    deleteQuestion(item) {
  axiosInstance
    .get(`/pitanja/izbrisi/${item}`) // Update the URL to match the route
    .then((response) => {
      this.questions = response.data;
    })
    .catch((error) => {
      // Handle the error
      console.error(error);
    });
    window.location.reload()
}
  },
};
</script>
