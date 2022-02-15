<template>
  <div class="soma">
    <header class="globalHeader">
      <form class="searchForm" method="get" action="https://www.baidu.com/s">
        <input name="wd" type="text" />
        <button type="submit">搜索</button>
      </form>
    </header>
    <main class="globalMain">
      <ul class="siteList">
        <li v-for="(url, index) in urlList" :key="index">
          <a :href="urlList[index].url">
            <div class="site">
              <div class="logo">{{ urlList[index].logo }}</div>
              <div class="link">{{ simplifyUrl(urlList[index].url) }}</div>
              <div class="close" @click.prevent="close(index)">
                <svg class="icon" aria-hidden="true">
                  <use xlink:href="#icon-close"></use>
                </svg>
              </div>
            </div>
          </a>
        </li>
        <li>
          <div class="addButton" @click="add">
            <div class="icon-wrapper">
              <svg class="icon">
                <use xlink:href="#icon-add"></use>
              </svg>
            </div>
            <div class="text">新增网站</div>
            
          </div>
        </li>
      </ul>
    </main>
    <footer class="globalFooter">
      <div class="footerButton">搜码 - 私人导航网站</div>
    </footer>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      addUrl: null,
      urlList: this.xObject || [
        { logo: "J", url: "https://juejin.cn/" },
        { logo: "G", url: "https://github.com/" },
        { logo: "Z", url: 'https://www.zhihu.com/hot'}
        ],
      xObject: null,
      logoText: null,
      cache:null,
      key: null
    };
  },
  methods: {
    add() {
      this.addUrl = window.prompt("请问你要添加的网址是？");
      if(this.addUrl === null){
        return 
      }
      if (this.addUrl.indexOf("http") !== 0) {
        this.addUrl = "https://" + this.addUrl;
      }
      this.logoText = this.simplifyUrl(this.addUrl);
      this.urlList.push({ logo: this.logoText[0], url: this.addUrl });
    },
    simplifyUrl(url) {
      return url
        .replace("https://", "")
        .replace("http://", "")
        .replace("www.", "")
        .replace(/\/.*/, ""); // 删除 / 开头的内容
    },
    close(index) {
      this.urlList.splice(index, 1)
    }
  },
  beforeCreate() {
    this.xObject = JSON.parse(localStorage.getItem("x"));
  },
  mounted() {
    window.onbeforeunload = () => {
      this.cache = JSON.stringify(this.urlList)
      localStorage.setItem('x', this.cache)
    },
    // 监听键盘事件
    document.onkeydown = (e) => {
      this.key = e.key;
      for(let i= 0; i< this.urlList.length; i++){
        if(this.urlList[i].logo.toLowerCase() === this.key){
          window.open(this.urlList[i].url)
        }
      }
    }
  }
};
</script>

<style scoped lang="scss">
.soma {
  position: relative;
  .globalHeader {
    margin: 20px;
    @media (min-width: 500px) {
      margin: 60px 0 100px;
    }
    .searchForm {
      display: flex;
      justify-content: space-between;
      @media (min-width: 500px) {
        max-width: 600px;
        margin-left: auto;
        margin-right: auto;
      }
      input {
        width: 100%;
        margin-right: 10px;
        height: 40px;
        padding: 0 20px;
        border: 1px solid #ddd;
        border-radius: 20px;
      }
      button {
        white-space: nowrap;
        padding: 0 28px;
        border: none;
        border-radius: 4px;
        background: orange;
        color: white;
        font-size: 16px;
      }
    }
  }

  .globalMain {
    max-width: 900px;
    margin-left: auto;
    margin-right: auto;
    .siteList {
      margin: 20px;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      @media (min-width: 500px){
        margin-left: 0;
        margin-right: -25px;
        justify-content: flex-start;
      }
      li {
        margin-bottom: 10px;
        @media (min-width: 500px) {
          margin-right: 25px;
        }
      }
      .site {
        width: 160px;
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
        background: white;
        border: 1px solid #ddd;
        border-radius: 9px;
        padding: 20px 0;
        position: relative;
        cursor: pointer;

        .logo {
          width: 64px;
          height: 64px;
          display: flex;
          justify-content: center;
          align-items: center;
          font-size: 64px;
          text-transform: uppercase;
        }
        .link {
          font-size: 14px;
          margin-top: 4px;
        }
        .close {
          position: absolute;
          right: 10px;
          top: 5px;
          display: block;
          cursor: default;
          @media (min-width: 500px) {
            display: none;
          }
        }
      }
      .site:hover > .close {
        display: block;
      }

      .addButton {
        background: white;
        width: 160px;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        padding: 20px 0;
        border-radius: 9px;
        border: 1px solid #ddd;
        .icon {
          width: 56px;
          height: 56px;
        }
        .text {
          font-size: 14px;
          margin-top: 4px;
        }
        .icon-wrapper {
          width: 64px;
          height: 64px;
          display: flex;
          justify-content: center;
          align-items: center;
        }
      }
    }
  }

  .globalFooter {
    .footerButton {
      position: absolute;
      font-size: 15px;
      opacity: 0.4;
      left: 0;
      right: 0;
      bottom: -16px;
      text-align: center;
    }
  }
}
</style>
