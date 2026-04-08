<template>
  <div class="container">
    <div class="form-card">
      <header class="header">

        <h1 v-if="formOn" class="title">{{ msg }}</h1>
        <h1 v-else class="title"> Result </h1>
        <button v-if="!formOn" @click="onVoltar()" type="button" class="btn btn-secondary" style="margin-top: 20px">Voltar</button>
      </header>
    <section v-if="formOn" class="form">
      <form @submit.prevent="this.onGravar()" >
        <div class="form-group">
          <label for="experience">Experience</label>
          <input type="number" id="experience" v-model="experience" placeholder="Years of Professional Coding Experience">
        </div>
        <div class="form-group">
          <label for="country">Country</label>
          <select class="input-select" id="country" v-model="country">
            <option disabled value="">Country of Residence</option>
            <option v-for="c in countries" :key="c" :value="c">{{c}}</option>

          </select>
        </div>
        <div class="form-group">
          <label for="education">Education</label>
          <select class="input-select" id="education" v-model="education">
            <option disabled value=""> Highest Level of Formal Education </option>
            <option v-for="c in educations" :key="c" :value="c">{{c}}</option>
          </select>


        </div>
        <div class="form-group">
          <label for="languages">Languages</label>
          <div class="form-group">
            <div class="checkbox-grid">
              <label><input type="checkbox" value="lC#"> C#</label>
              <label><input type="checkbox" value="lC++"> C++</label>
              <label><input type="checkbox" value="lGO"> GO</label>
              <label><input type="checkbox" value="lJava"> Java</label>
              <label><input type="checkbox" value="lJavaScript"> JavaScript</label>
              <label><input type="checkbox" value="lPHP"> PHP</label>
              <label><input type="checkbox" value="lPython"> Python</label>
              <label><input type="checkbox" value="lRuby"> Ruby</label>
              <label><input type="checkbox" value="lRust"> Rust</label>
              <label><input type="checkbox" value="lSwift"> Swift</label>
            </div>
          </div>
        </div>
        <div class="form-group">
          <label for="frameworks">Frameworks</label>
          <div class="checkbox-grid">
            <label><input type="checkbox" value="Angular"> Angular</label>
            <label><input type="checkbox" value="ASP.NET"> ASP.NET</label>
            <label><input type="checkbox" value="Django"> Django</label>
            <label><input type="checkbox" value="Express"> Express</label>
            <label><input type="checkbox" value="Flask"> Flask</label>
            <label><input type="checkbox" value="Laravel"> Laravel</label>
            <label><input type="checkbox" value="React"> React</label>
            <label><input type="checkbox" value="Ruby on Rails"> Ruby on Rails</label>
            <label><input type="checkbox" value="Spring"> Spring</label>
            <label><input type="checkbox" value="Vue"> Vue</label>
          </div>
        </div>
        <div class="form-group">
          <label for="company_size">Company_Size</label>
          <select class="input-select" id="company_size" v-model="company_size">
            <option disabled value=""> Number of Employees in the Company </option>
            <option v-for="c in company_sizes" :key="c" :value="c">{{c}}</option>
          </select>
        </div>
        <div class="form-actions">

          <button type="submit" class="btn btn-primary">Confirm</button>
          <button @click="onApagar()" type="button" class="btn btn-secondary">Delete</button>
        </div>
      </form>
    </section>

    </div>
  </div>

</template>

<script>
import axios from 'axios'
import { toast } from 'vue3-toastify'
import 'vue3-toastify/dist/index.css'
export default {

  name: "FormPrevisao",
  props: {
    msg: String
  },
  data() {
    return { experience:"", country:"", education:"", languages:"", frameworks:"", company_size:"", formOn: true,
    countries: ["Brazil", "Australia", "Canada", "France", "Germany", "India", "Japan", "Singapore", "UK", "USA"],
    educations: ["Bachelors", "High School", "Masters", "PhD", "Some College"],
    company_sizes:["1-10", "11-50", "51-200","201-1000",  "1001-5000", "5000+"]
    }
  },
  methods: {
    mostrarForm(flag)
    {
      this.formOn = flag;
    },
    onGravar()
    {
      this.onVerificarSelecionados()
      if (this.isValido())
      {
        this.onApagar()
        this.onDesmarcarSelect()
        this.mostrarForm(false)
      }
      else
      {
        this.mostrarErro()
      }
    },
    onApagar()
    {
      this.experience = 0, this.country = "", this.education = "", this.languages = "", this.frameworks= "", this.company_size=""
      this.onDesmarcarSelect()
    },
    onVoltar()
    {
      this.onApagar()
      this.formOn = true
    },
    onVerificarSelecionados()
    {
      let checkBox = document.querySelectorAll('input[type="checkbox"]')
      let i = 0;
      while (i < checkBox.length)
      {
        if (checkBox[i].checked)
        {
          console.log(checkBox[i].value);
          if (this.isLanguage(checkBox[i].value))
          {
            checkBox[i].value = checkBox[i].value.replace(/l/g, "")
            console.log("NOVO: "+checkBox[i].value);
            if (this.languages.length > 0)
              this.languages = this.languages + ", " + checkBox[i].value;
            else
              this.languages = checkBox[i].value;
          }
          else
          {
            if (this.frameworks.length > 0)
              this.frameworks = this.frameworks + ", " + checkBox[i].value;
            else
              this.frameworks = this.frameworks + checkBox[i].value;

          }
        }
        i++;

      }
      console.log("FRAMEWORKES: "+this.frameworks);
      console.log("LANGUAGES: "+this.languages);
    },
    onDesmarcarSelect()
    {
      let checkBox = document.querySelectorAll('input[type="checkbox"]')
      let i = 0;
      while (i < checkBox.length)
      {
        if (checkBox[i].checked)
          checkBox[i].checked = false;
        i++;
      }
    },
    onCarregarDados()
    {

    },
    isLanguage(value)
    {
      if (value.at(0) === 'l')
        return true;
      return false;
    },
    isValido()
    {
      if (this.experience >= 0 && this.country !== "" && this.education !== "" && this.languages !== "" && this.frameworks !== "" && this.company_size !== "")
        return true;
      return false;
    },
    mostrarErro()
    {
        toast.error("Campo(s) Não Preenchido(s)", {
            autoClose: 2000,
        });
    }



  },

}
</script>

<style scoped>

.container {
  min-height: 50vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  box-sizing: border-box;
}

.form-card {
  background-color: #ffffff;
  width: 100%;
  max-width: 550px;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
}

.header {
  text-align: center;
  margin-bottom: 25px;
}

.title {
  color: #333;
  margin: 0;
  font-size: 2rem;
  font-weight: bold;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  letter-spacing: 1.5px;
}

.form-group {
  margin-bottom: 15px;
}
.checkbox-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 5px 10px;
}

.checkbox-grid label {
  display: flex;
  align-items: center;
  width: 120px;
}
label {
  display: block;
  font-size: 14px;
  font-weight: bold;
  color: #555;
  margin-bottom: 5px;
}

input {
  width: 100%;
  box-sizing: border-box;
  padding: 10px 12px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
  transition: border-color 0.3s;
}
input:not([type="checkbox"]) {
  width: 100%;
  padding: 10px;
}

input[type="checkbox"] {
  width: auto;
  margin: 3px;
}
.input-select {
  width: 100%;
  padding: 10px 12px;
  border-radius: 8px;
  border: 1px solid #ccc;
  font-size: 14px;
  transition: 0.3s;
  background-color: white;
  cursor: pointer;
}

input:focus {
  outline: none;
  border-color: #4CAF50;
}

.form-actions {
  display: flex;
  justify-content: space-between;
  margin-top: 25px;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 6px;
  font-size: 14px;
  font-weight: bold;
  cursor: pointer;
  transition: opacity 0.3s;
  width: 48%;
}

.btn:hover {
  opacity: 0.8;
}

.btn-primary {
  background-color: #4CAF50;
  color: white;
}

.btn-secondary {
  background-color: #e0e0e0;
  color: #333;
}
</style>
