<template>
  <div>
    在线翻译

    <input type="text" v-model="content2">

    <button @click="newObj" disabled>
      创建翻译
    </button>

    <button @click="changeObj">
      修改翻译
    </button>

    <div>
      {{ content }}
    </div>

  </div>
</template>

<script>
const AV = require('leancloud-storage');
const {Query} = AV;
AV.init({
  appId: "I7eMhiBdgdrxo6wQwzoR8h1g-gzGzoHsz",
  appKey: "9tcPfUNhAuiN0NB2E7XiwkKp",
  serverURL: "https://i7emhibd.lc-cn-n1-shared.com"
});

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      oid: '5fe314c4e827d35c6423839a',
      content: '',
      content2: ''
    }
  },
  methods: {
    newObj() {
      const Translation = AV.Object.extend('Translation');
      const en = new Translation();
      en.set('words', 'Hello world!');
      en.save().then((obj) => {
        this.oid = obj.id
      })
    },
    getObj() {
      const query = new Query('Translation');
      query.get(this.oid).then((todo) => {
        this.content = todo.get('words')
      });
    },
    changeObj() {
      const en = AV.Object.createWithoutData('Translation', this.oid);
      en.set('words', this.content2);
      en.save();
    }
  },
  created() {
    this.getObj()
  }
}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
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
