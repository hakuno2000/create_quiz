<template>
  <div>
    <v-row>
      <v-toolbar flat>
        <v-toolbar-title>Quiz Info</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
      </v-toolbar>
    </v-row>
    <v-row>
      <v-col cols="12" sm="12">
        <v-text-field
            v-model="filename"
            label="File Name (for .trealet file export)"
            outlined
            @input="$emit('updateFilename', filename)"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" sm="6">
        <v-text-field
            v-model="title"
            label="Title"
            outlined
            @input="$emit('updateTitle', title)"
        ></v-text-field>
      </v-col>
      <v-col cols="12" sm="6">
        <v-text-field
            v-model="author"
            label="Author"
            outlined
            @change="$emit('updateAuthor', author)"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-container fluid>
        <v-textarea
            name="input-7-1"
            label="Description"
            auto-grow
            outlined
            v-model="description"
            @change="$emit('updateDescription', description)"
        ></v-textarea>
      </v-container>
    </v-row>
  </div>
</template>

<script>
export default {
  props: {
    filenameProp: {
      type: String,
      default: ""
    },
    titleProp: {
      type: String,
      default: ""
    },
    authorProp: {
      type: String,
      default: ""
    },
    descriptionProp: {
      type: String,
      default: ""
    },
  },

  data: () => ({
    filename: "hoihoaphuchung",
    title: "Chủ đề của Trealet",
    author: "Tác giả",
    description: "Nội dung miêu tả",
  }),

  mounted() {
    this.filename = this.filenameProp
    this.title = this.titleProp
    this.author = this.authorProp
    this.description = this.descriptionProp
  }
};
</script>
