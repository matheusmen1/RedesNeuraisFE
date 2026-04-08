<template>
  <div id = "app">
    <nav>
      <div class="navbar-left">
        <router-link to="/" class="navbar-link">
          <img src="./assets/logoNovo.png" alt="Logo" class="navbar-logo">
          <span class="brand-title">Software Developer Salary Prediction</span>
        </router-link>
      </div>
    </nav>
    <main class="content">
      <Form msg="Prediction Form"/>
    </main>
    <footer class="footer">
      <p class="footer-titulo">@RedesNeuraisMLP</p>
    </footer>
  </div>

</template>

<script>
import FormPrevisao from "@/components/FormPrevisao.vue";
export default {
  name: "App",
  components: {
    Form: FormPrevisao
  }

}
</script>


<style>

* {

  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-weight: 500;
  font-family: 'Poppins', sans-serif;
}
#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
.brand-title {
  font-size: 2rem;
  font-weight: 700;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  letter-spacing: 1.5px;
  background: linear-gradient(90deg, white, #868686);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.footer-titulo{

  font-size: 1rem;
  font-weight: 700;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  letter-spacing: 1.5px;
  background: linear-gradient(90deg, white, #868686);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #333;
  padding: 10px 20px;
  color: white;
}
.content {
  flex: 1;
  padding: 20px;
}

.footer {
  background-color: #333;
  color: white;
  padding: 5px;
  text-align: center;
}
.navbar-left {
  margin-left: 2%;
  font-size: 20px;
  display: flex;
  align-items: center;
}

.navbar-link {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: white;
}

.navbar-logo {
  width: 100px;
  height: 65px;
  margin-right: 10px;
}

</style>