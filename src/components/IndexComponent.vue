<template>
  <div class="mainlists">
    <div class="row">
      <div class="col-md-3">
        <router-link :to="{ name: 'create' }" class="btn btn-primary"
          >Create Post</router-link
        >
      </div>
      <div class="col-md-9">
        Search mainlists -- _id more than <input v-model="qsearch" />
      </div>
    </div>

    <table class="table table-hover ">
      <thead>
        <tr>
          <th>Actions</th>
          <th>Title</th>
          <th>Body</th>
          <th>_id</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="post in mainlists" :key="post._id">
          <router-link
            :to="{ name: 'edit', params: { id: post._id } }"
            class="btn btn-primary"
            >Edit</router-link
          >

          <td>{{ post.title }}</td>
          <td>{{ post.body }}</td>
          <td>{{ post._id }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: {},
      post: {},
      mainlists: {},
      resultsPerPage: 25,
      currentPage: 1,
      qsearch: "190223_2327"
    };
  },
  created() {
    //
    // not using this for pouchdb David Gleba 2019-02-22_Fri_16.26-PM
    //
    // let uri = "http://localhost:4000/posts";
    // this.axios.get(uri).then(response => {
    //   this.posts = response.data;
    // });
  },

  methods: {},

  // Use the pouch property to configure the component to (reactively) read data from pouchdb.
  pouch: {
    // The function returns a Mango-like selector that is run against the `mainlists` database.
    // The result of the query is assigned to the `mainlists` property.
    mainlists2: function() {
      // if (this.age === this.qsearch) return;
      // return { age: this.age, type: "mlist" };
    },
    // You can also specify the database dynamically (local or remote), as well as limits, skip and sort order:
    mainlists: function() {
      return {
        //database: this.selectedDatabase, // you can pass a database string or a pouchdb instance
        selector: { rtype: "mlist", _id: { $gte: this.qsearch } },
        sort: [{ _id: "desc" }],
        limit: this.resultsPerPage
      };
    }
  }
};
</script>
