<template>
  <v-data-table
      :headers="headers"
      :items="itemList"
      :expanded.sync="expanded"
      :single-expand="true"
      item-key="image"
      show-expand
      sort-by="number"
      class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>List of Questions</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn
                color="white"
                class="mb-2"
                v-bind="attrs"
                v-on="on"
                style="margin: 5px"
            >
              Add Question
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field
                        v-model="editedItem.question"
                        label="Question"
                        prepend-icon="mdi-account-question-outline"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="5">
                    <v-text-field
                        v-model="editedItem.image"
                        label="Id of question's image"
                        prepend-icon="mdi-image-edit-outline"
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col v-for="option in editedItem.options" :key="option.index" cols="12" sm="12" md="12">
                    <v-text-field
                        v-model="option.text"
                        label="Answer"
                        prepend-icon="mdi-message-outline"
                    ></v-text-field>
                  </v-col>
                  <v-col
                      class="d-flex"
                      cols="12"
                      sm="12"
                  >
                    <v-select
                        v-model="editedItem.answer_text"
                        :items="editedItem.options"
                        label="Correct answer"
                        prepend-icon="mdi-check-all"
                    ></v-select>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                  color="blue darken-1"
                  text
                  :disabled="isNotNumber(editedItem.image)"
                  @click="save"
              >
                Save
              </v-btn>
              <v-btn color="blue darken-1" text @click="close"> Cancel</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title
                class="text-h5"
                style="display: flex; justify-content: center"
            >Do you want to remove this item?
            </v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm"
              >Confirm
              </v-btn
              >
              <v-btn color="blue darken-1" text @click="closeDelete"
              >Cancel
              </v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:expanded-item="{ item }">
      <div style="display: flex; justify-content: center; margin: 20px">
        <div class="quiz-container" id="quiz-container">
          <div class="quiz-options d-flex justify-content-between">
            <div class="correct-counter d-flex align-items-center">
              <i class="far fa-check-circle icon mr-2"></i>
              <!--          <strong>{{ questionsWithCorrectAnswers.length }}/{{ maxReachedQuestion+1 }}</strong>-->
            </div>
            <div class="mx-auto">
              <h4 class="my-0">Câu số 1</h4>
            </div>
          </div>
          <div class="quiz">
            <section class="quiz--title">{{ item.question }}</section>
            <!--        <span v-html="images[currentQuestion]"></span>-->
            <div class="quiz-selections row justify-content-center gx-5">
              <div class="col-12 col-md-5 mb-3 mx-md-4" v-for="(item, index) in item.options" :key="index">
                <div class="quiz-selections--item">
                  <section class="quiz-selections--letter mr-3">{{ getSelectionRepresentation(index + 1) }}</section>
                  <p class="mx-0 mb-0 mt-2">{{ item.text }}</p>
                </div>
              </div>
            </div>
          </div>
          <!--        <hr class="mx-1 mx-md-5">-->
          <!--        <div class="quiz-btn d-flex justify-content-end align-items-center">-->
          <!--          <strong class="mr-2">{{ "Tiếp" }}</strong>-->
          <!--          <i class="fas fa-arrow-circle-right icon-lg mr-2 mr-md-5"></i>-->
          <!--        </div>-->
        </div>
      </div>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil</v-icon>
      <v-icon small @click="deleteItem(item)"> mdi-delete</v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize"> Reset</v-btn>
    </template>
  </v-data-table>
</template>

<script>
export default {
  props: {
    questionListProp: {
      type: Array,
      default: () => [],
    },
  },

  data: () => ({
    expanded: [],
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: "Question",
        align: "start",
        sortable: false,
        value: "question",
      },
      {text: "Image id", value: "image"},
      {text: "Actions", value: "actions", sortable: false},
      {value: 'data-table-expand'},
    ],
    itemList: [],
    editedIndex: -1,
    editedItem: {
      image: -1,
      question: "",
      options: [
        {
          index: 0,
          text: "Answer 1",
        },
        {
          index: 1,
          text: "Answer 2",
        },
        {
          index: 2,
          text: "Answer 3",
        },
        {
          index: 3,
          text: "Answer 4",
        }
      ],
      answer_text: "",
      answer: 0,
    },
    defaultItem: {
      image: -1,
      question: "",
      options: [
        {
          text: "Answer 1",
        },
        {
          text: "Answer 2",
        },
        {
          text: "Answer 3",
        },
        {
          text: "Answer 4",
        }
      ],
      answer: 0,
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Question" : "Edit Question";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
    itemList: {
      handler(val) {
        this.$emit("updateList", val);
      },
      deep: true,
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.itemList = this.questionListProp;
    },

    editItem(item) {
      this.editedIndex = this.itemList.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.itemList.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.itemList.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, JSON.parse(JSON.stringify(this.defaultItem)));
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, JSON.parse(JSON.stringify(this.defaultItem)));
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.isNotNumber(this.editedItem.image)) return;

      this.editedItem.options.forEach((option, index) => {
        if (option.text === this.editedItem.answer_text) this.editedItem.answer = index;
      })

      if (this.editedIndex > -1) {
        Object.assign(this.itemList[this.editedIndex], this.editedItem);
      } else {
        this.itemList.push(this.editedItem);
      }
      this.close();
    },

    isNotNumber(val) {
      return isNaN(val);
    },

    numberOnly() {
      return [(val) => /^[0-9]*$/.test(val) || "Invalid"];
    },

    updateOptions(value) {
      this.editedItem.options = value;
    },

    updateAnswer(value) {
      this.editedItem.answer = this.editedIndex.options.indexOf(value);
    },

    getSelectionRepresentation(index) {
      return (index + 9).toString(36).toUpperCase()
    },
  },
};
</script>

<style scoped>
.quiz-container {
  padding: 20px 30px;
  box-shadow: rgba(0, 0, 0, 0.2) 1px 3px 8px 1px;
  border-radius: 8px;
  color: white;
  background-color: #FBAB7E;
  background-image: linear-gradient(62deg, #FBAB7E 0%, #F7CE68 100%);
  min-height: 500px;
  position: relative;
}

.quiz--title {
  font-size: 1.8em;
  min-height: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
  text-align: center;
}

.quiz-selections--letter {
  font-weight: bold;
  font-size: 1.2em;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: orange;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
}


.quiz-selections--item {
  display: flex;
  padding: 10px;
  background: rgba(255, 255, 255, 0.6);
  border-radius: 50px;
  transition: all .2s ease;
}

.quiz-selections--item:hover {
  cursor: pointer;
  background-color: rgba(255, 255, 255, 0.8);
}

.quiz-selections--item p {
  color: #212124;
  max-width: 75%;
  text-align: justify;
}

.quiz-selections__correct {
  background-color: #53E691;
}

.quiz-selections__correct:hover {
  background-color: #53E691;
}

.quiz-selections__wrong {
  background-color: #ED524E;
}

.quiz-selections__wrong:hover {
  background-color: #ED524E;
}

.quiz-selections__correct p,
.quiz-selections__wrong p {
  color: white;
  font-weight: bold;
}

.quiz-btn:hover {
  cursor: pointer;
}

.correct-counter {
  position: absolute;
}

.quiz--complete {
  height: 100%;
  position: absolute;
  width: 80%;
  top: 0;
  left: 0;
  right: 0;
  margin: auto;
}

.redo {
  color: #ED524E;
  transition: all .2s ease;
}

.redo:hover {
  cursor: pointer;
  color: white;
}

.correct-text {
  color: #53E691;
  font-weight: bold;
  font-size: 2em;
}

.icon {
  font-size: 1.5em;
}

.icon-lg {
  font-size: 1.8em;
}

.mw-100 {
  max-width: 100%;
}

hr {
  border: none;
  height: 1px;
  color: white;
  background-color: white;
  margin-top: 20px;
  margin-bottom: 20px;
}

.quiz--img {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  margin-bottom: 30px;
  max-height: 300px;
  margin-top: -30px;
}
</style>
