<template>
  <v-app id="inspire">
    <NavBar />
    <div style="display: flex;
    flex-direction: column;
    align-items: center;">
    <ul style="display: flex;
    flex-direction: column;
   
    align-items: center;">
        <li
          v-for="question in questions"
          :key="question.id"
          style="margin: 20px;
          width: 100%;
       "
        >
          <div
            style="
              display: flex;
              flex-direction: row;
              justify-content: space-between;
              align-items: center;
            "
          >
            <div>
              <p>Pitanje: {{ question.pitanje }}</p>
              <p><v-text-field v-model="question.answer" label="Odgovor"></v-text-field><button
        @click="editQuestion(question.id, question.answer)"
        style="
          width: 100px;
          background-color: #333;
          border-radius: 5px;
          margin-right: 30px;
          margin-bottom: 30px;
          color: #fff;
          border-color: #333;
          padding: 10px 20px;
          font-size: 16px;
          box-shadow: none;
        "
      >
        Send
      </button></p>
            </div>
            <div>
              
              
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
      answer: "",
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
          console.log(this.questions)
        })
        .catch((error) => {
          // Handle the error
          console.error(error);
        });
    },
    editQuestion(id, que) {
      const requestData = { odgovor: que }; // Use the answer for the specific question
      axiosInstance
        .post(`/pitanja/uredi/${id}`, requestData)
        .then((response) => {
          console.log(response.data);
          // Handle success if needed
        })
        .catch((error) => {
          console.error(error);
          // Handle error if needed
        });

      setTimeout(() => {
        window.location.reload();
      }, 200);
  },
    
    },
  }
</script>
