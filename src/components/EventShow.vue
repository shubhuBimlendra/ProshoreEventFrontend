<template>
  <link
    rel="stylesheet"
    href="https://cdn.jsdelivr.net/npm/bootstrap@4.3.1/dist/css/bootstrap.min.css"
    integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T"
    crossorigin="anonymous"
  />

  <div class="container h-100">
    <div class="row h-100 justify-content-center align-items-center">
      <div class="col-10 col-md-8 col-lg-6">
        <!-- Form -->
        <form
          @submit.prevent="save"
          class="form-example"
          action=""
          method="post"
        >
          <h1>Add Event</h1>
          <!-- Input fields -->
          <div class="form-group">
            <input
              type="text"
              v-model="event.title"
              class="form-control"
              id="title"
              placeholder="Enter title here"
            />
          </div>
          <div class="form-group">
            <input
              type="text"
              v-model="event.description"
              class="form-control"
              id="description"
              placeholder="Enter description here"
            />
          </div>
          <div class="form-group">
            <input
              type="date"
              v-model="event.start_date"
              class="form-control"
              name="start_date"
            />
          </div>
          <div class="form-group">
            <input
              type="date"
              v-model="event.end_date"
              class="form-control"
              name="end_date"
            />
          </div>
          <button type="submit" class="btn btn-primary btn-customized">
            Submit
          </button>
        </form>
        <!-- Form end -->
      </div>
    </div>
  </div>

  <h1>All Events</h1>
  <table class="table">
    <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">Title</th>
        <th scope="col">Description</th>
        <th scope="col">Start Date</th>
        <th scope="col">End Date</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="event in result" v-bind:key="event.id">
        <td>{{ event.id }}</td>
        <td>{{ event.title }}</td>
        <td>{{ event.description }}</td>
        <td>{{ event.start_date }}</td>
        <td>{{ event.end_date }}</td>
        <td>
          <button type="button" class="btn btn-warning" @click="edit(event)">
            Edit
          </button>
          <button
            type="button"
            class="btn btn-danger"
            @click="remove(event.id)"
          >
            Delete
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      result: {},
      event: {
        id: "",
        title: "",
        description: "",
        start_date: "",
        end_date: "",
      },
    };
  },

  created() {
    this.EventLoad();
  },

  methods: {
    EventLoad() {
      var page = "http://127.0.0.1:8000/api/events";
      axios.get(page).then(({ data }) => {
        console.log(data);
        this.result = data;
      });
    },
    save() {
      if (this.event.id == "") {
        this.saveData();
      } else {
        this.updateEvent();
      }
    },
    saveData() {
      axios
        .post("http://127.0.0.1:8000/api/events", this.event)
        .then(({ data }) => {
          console.log(data);
          alert("Data Saved Successfully");
          this.EventLoad();
        });
    },
    edit(event) {
      this.event = event;
    },
    updateEvent() {
      var editevents = "http://127.0.0.1:8000/api/events/" + this.event.id;
      axios.put(editevents, this.event).then(({ data }) => {
        (this.event.title = ""),
          (this.event.description = ""),
          (this.event.start_date = ""),
          (this.event.end_date = ""),
          (this.id = "");
        alert("Updated");
        this.EventLoad();
        console.log(data);
      });
    },
    remove(id) {
      var deleteevents = "http://127.0.0.1:8000/api/events/" + id;
      axios.delete(deleteevents);
      alert("Event Deleted Successfully");
      this.EventLoad();
    },
  },
};
</script>

<style></style>
