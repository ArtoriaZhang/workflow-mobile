<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>Hello Vue in CodeSandbox!</p>
    <SupHeader
      :headers="headers"
      :currentHeader="selectedHeader"
      @selecteHeader="headerClick"
    >
    </SupHeader>

    <InquiryFooter :links="computedLinks"></InquiryFooter>
  </div>
</template>

<script>
import SupHeader from "./inquiry-detail-header.vue";
import InquiryFooter from "./inquiry-detail-footer.vue";

export default {
  name: "HelloWorld",
  components: {
    SupHeader,
    InquiryFooter
  },
  methods: {
    headerClick: function(header) {
      this.selectedHeader = header;
      // console.log(this.selectedHeader);
    }
  },
  computed: {
    computedLinks: function() {
      //  console.log("compute header name:" + this.selectedHeader.name);
      return this.links.filter(link => {
        return (
          this.selectedHeader.name && this.selectedHeader.name === link.header
        );
      });
    }
  },
  props: {
    msg: String,
    links: {
      default: function() {
        return [
          {
            header: "Installed CLI Plugins",
            href:
              "https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel",
            target: "_blank",
            rel: "noopener",
            name: "babel"
          },
          {
            header: "Installed CLI Plugins",
            href:
              "https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint",
            target: "_blank",
            rel: "noopener",
            name: "eslint"
          },
          {
            header: "Essential Links",
            href: "https://vuejs.org",
            target: "_blank",
            rel: "noopener",
            name: "Core Docs"
          },
          {
            header: "Essential Links",
            href: "https://forum.vuejs.org",
            target: "_blank",
            rel: "noopener",
            name: "Forum"
          },
          {
            header: "Essential Links",
            href: "https://chat.vuejs.org",
            target: "_blank",
            rel: "noopener",
            name: "Community Chat"
          },

          {
            header: "Essential Links",
            href: "https://twitter.com/vuejs",
            target: "_blank",
            rel: "noopener",
            name: "Twitter"
          },
          {
            header: "Essential Links",
            href: "https://news.vuejs.org",
            target: "_blank",
            rel: "noopener",
            name: "News"
          },
          {
            header: "Ecosystem",
            href: "https://router.vuejs.org",
            target: "_blank",
            rel: "noopener",
            name: "vue-router"
          },
          {
            header: "Ecosystem",
            href: "https://vuex.vuejs.org",
            target: "_blank",
            rel: "noopener",
            name: "vuex"
          },
          {
            header: "Ecosystem",
            href: "https://github.com/vuejs/vue-devtools#vue-devtools",
            target: "_blank",
            rel: "noopener",
            name: "vue-devtools"
          },
          {
            header: "Ecosystem",
            href: "https://vue-loader.vuejs.org",
            target: "_blank",
            rel: "noopener",
            name: "vue-loader"
          },
          {
            header: "Ecosystem",
            href: "https://github.com/vuejs/awesome-vue",
            target: "_blank",
            rel: "noopener",
            name: "awesome-vue"
          }
        ];
      },
      type: Array
    }
  },
  data: function() {
    return {
      headers: [
        {
          name: "Installed CLI Plugins",
          short: "CLI Plugins",
          long: "Installed CLI Plugins"
        },
        {
          name: "Essential Links",
          short: "Ess Links",
          long: "Essential Links"
        },
        { name: "Ecosystem", short: "Eco", long: "Ecosystem" }
      ],
      selectedHeader: ""
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
