<template>
  <div id="app">
    <div class="wrapper">
      <div style="text-align: left">
        <nylas-composer
          ref="composer"
          id="41abca6e-bcb8-4e18-aaef-b506fd9b35ac"
        ></nylas-composer>
      </div>
    </div>
  </div>
</template>

<script>
import "@nylas/components-composer";
export default {
  name: "App",
  data() {
    return {
      internalData: [
        { id: 1, email: "al@particletech.com" },
        { id: 2, email: "b.franklin@keyelectric.com" },
        { id: 3, email: "demo@nylas.com" },
        { id: 4, email: "carver@agritech.com" },
        { id: 5, email: "mary@spacetech.com" },
      ],
      msg: {},
      initialValues: {
        from: [
          {
            name: "Katherine Johnson",
            email: "kat@spacetech.com",
          },
        ],
        to: [
          {
            name: "Dorothy Vaughan",
            email: "dorothy@spacetech.com",
          },
        ],
        subject: "Nylas Composer",
        body:
          "You can learn more about Nylas Composer in our <a href='https://docs.nylas.com/docs/nylas-components'>documentation</a>.",
      },
    };
  },
  methods: {
    externalDataSource: (term) => {
      return fetch(`https://jsonplaceholder.typicode.com/users`)
        .then((res) => res.json())
        .then((res) => {
          return res
            .map((item) => ({ name: item.name, email: item.email }))
            .filter((item) =>
              item.name.toLowerCase().includes(term.toLowerCase())
            );
        })
        .catch(() => Promise.resolve([]));
    },
    send: async (data) => {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          return reject({ success: false, data });
        }, 750);
      });
    },
  },
  mounted: function () {
    this.$nextTick(function () {
      const el = this.$refs.composer;
      el.change = (msg) => {
        console.log(msg);
        this.msg = msg;
      };
      el.value = this.initialValues;
      el.options = {
        show_header: true,
        show_subject: true,
        mode: "inline", // or inline
        theme: "light",
        show_editor_toolbar: true,
        show_from: true,
        show_to: true,
      };
      el.to = this.externalDataSource;
      el.from = this.internalData;
      el.send = this.send;
      // Set initial Values
    });
  },
};
</script>

<style>
body {
  margin: 0px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.wrapper {
  display: flex;
  align-items: center;
  padding: 2rem;
  justify-content: center;
}
</style>