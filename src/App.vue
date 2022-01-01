<template>
  <v-app>
    <v-app-bar app color="white">
      <div class="d-flex align-center">
        <v-img
            alt="Trealet Logo"
            class="shrink mr-2"
            contain
            src="@/assets/logo.png"
            transition="scale-transition"
            width="150"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
          href="https://hcloud.trealet.com/apps_dev/btl/nhom03/Streamline/?trealet=/albums/Nhom03/app/hoihoaphuchung.trealet"
          target="_blank"
          text
      >
        <span class="mr-2">View Trealet</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <div style="display: flex; justify-content: center">
        <div style="width: 80%">
          <v-form>
            <v-container>
              <Info
                  :filenameProp="filename"
                  :titleProp="title"
                  :authorProp="author"
                  :descriptionProp="description"
                  @updateFilename="updateFilename"
                  @updateTitle="updateTitle"
                  @updateAuthor="updateAuthor"
                  @updateDescription="updateDescription"
              />
              <!--              <v-col class="d-flex mx-0 px-0" cols="12" sm="6">-->
              <!--                <v-select-->
              <!--                    v-model="trealetType"-->
              <!--                    :items="trealetTypes"-->
              <!--                    item-text="type"-->
              <!--                    item-value="index"-->
              <!--                    label="Trealet type"-->
              <!--                    outlined-->
              <!--                ></v-select>-->
              <!--              </v-col>-->

              <QuestionList
                  :questionListProp="questions"
                  @updateList="updateQuestion"
              />

              <v-divider class="my-12"></v-divider>

              <v-btn
                  class="ml-auto mb-3 d-block"
                  color="primary"
                  dark
                  large
                  @click="exportQuestion"
              >
                Export
              </v-btn>
            </v-container>
          </v-form>
        </div>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import Info from "@/components/Info";
import QuestionList from "@/components/QuestionList";

export default {
  name: "App",

  components: {
    Info,
    QuestionList,
  },

  data: () => ({
    filename: "quiz",
    title: "Tìm hiểu nghệ thuật thời kỳ phục hưng",
    author: "harukiiiii",
    description: "Thời kì Phục Hưng thường được người ta miêu tả ở khoảng thế kỉ thứ 16 nhưng ngay từ thế kỉ thứ 14, những mầm mống đầu tiên của phong trào này đã bắt đầu nhem nhóm từ Ý(Quatrocento – 1400). Trong thời kì này, sự trỗi dậy của tầng lớp giàu có – tiền thân của giai cấp tư sản sau này đã tạo ra một làn sóng xây dựng một nền văn hóa mới để chống lại giai cấp phong kiến lạc hậu. Phục Hưng có gốc từ tiếng Pháp (Renaissance – nghĩa là sự tái sinh), điều này ám chỉ tinh thần của nó là thời kỳ làm sống lại những tinh hoa văn hóa của Hy Lạp và La Mã cổ.",
    questions: [
      {
        image: 21322,
        question: "Tác giả của bức họa Mona Lisa là ai?",
        options: [
          {
            "text": "Leonardo da Vinci"
          },
          {
            "text": "Ricardo Milos"
          },
          {
            "text": "Rick Astley"
          },
          {
            "text": "Michelangelo Buonarroti"
          }
        ],
        answer: 0,
      },
      {
        image: -1,
        question: "Thời kỳ phục hưng ở Ý kéo dài trong khoảng thời gian nào?",
        options: [
          {
            "text": "1600 - 1700"
          },
          {
            "text": "1520 - 1700"
          },
          {
            "text": "1500 - 1600"
          },
          {
            "text": "1420 - 1600"
          }
        ],
        answer: 3,
      },
    ],
  }),

  watch: {
    trealetType(val) {
      if (val == 1) {
        this.questions = [
          {
            number: 1,
            item: "12345",
            trealet: "example1.trealet",
          },
          {
            number: 2,
            item: "56789",
            trealet: "example2.trealet",
          },
        ];
      } else if (val == 2) {
        this.questions = [
          {
            number: 1,
            item: "12345",
          },
          {
            number: 2,
            item: "56789",
          },
        ];
      }
    },
  },

  methods: {
    updateFilename(value) {
      this.filename = value;
    },

    updateTitle(value) {
      this.title = value;
    },

    updateAuthor(value) {
      this.author = value;
    },

    updateDescription(value) {
      this.description = value;
    },

    updateQuestion(value) {
      this.questions = value;
    },

    getUsableQuestionList() {
      return this.questions.map((obj) => ({
        question: +obj.question,
        image: +obj.image,
        options: +obj.options,
        answer: +obj.answer,
      }));
    },

    exportQuestion() {
      const exportObj = {
        trealet: {
          exec: "streamline",
          title: this.title,
          author: this.author,
          desc: this.description,
          questions: this.getUsableQuestionList(),
        },
      };

      // TO DO:
      // EXPORT FILE HERE
      const data = JSON.stringify(exportObj, null, 2);
      let FileSaver = require('file-saver');
      let blob = new Blob([data], {type: "text/plain;charset=utf-8"});
      FileSaver.saveAs(blob, this.filename + ".trealet");
      console.log(data);
    },
  },
};
</script>
