<template>
  <div>
    <md-table>
      <md-table-row>
        <md-table-head>Title</md-table-head>
        <md-table-head>Description</md-table-head>
        <md-table-head>Rating</md-table-head>
        <md-table-head>Action</md-table-head>
      </md-table-row>
      <md-table-row v-for="(post,index) in posts" :key="index">
        <md-table-cell>{{post.title}}</md-table-cell>
        <md-table-cell>{{post.desc}}</md-table-cell>
        <md-table-cell>{{post.rating}}</md-table-cell>
        <md-table-cell>
          <div class="post_delete" @click="deletePost(post.id)">Delete</div>
        </md-table-cell>
      </md-table-row>
    </md-table>
    <md-dialog-confirm
      :md-active.sync="confirmDelete"
      md-title="Confirm Delete"
      md-content="Are you sure to delete"
      md-confirm-text="Yes,Delete"
      md-cancel-text="No ,Cancel"
      @md-cancel="onCancel"
      @md-confirm="onConfirm"
    />
  </div>
</template>
<script>
export default {
  data() {
    return {
      confirmDelete: false,
      toDelete: null
    };
  },
  computed: {
    posts() {
      console.log(this.$store.getters["admin/getAdminPosts"]);
      let posts = this.$store.getters["admin/getAdminPosts"];
      return posts;
    }
  },
  created() {
    this.$store.dispatch("admin/getAdminPosts");
  },
  methods: {
    deletePost(id) {
      this.toDelete = id;
      this.confirmDelete = true;
    },
    onCancel() {
      this.toDelete = "";
      this.confirmDelete = true;
    },
    onConfirm() {
      this.$store.dispatch("admin/deletePost", this.toDelete);
      this.confirmDelete = false;
    }
  }
};
</script>