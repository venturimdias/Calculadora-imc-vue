<template>
  <div>
    <h1>Calcular IMC</h1>

    <div>
      Peso :
      <input type="number" v-model="pessoaPeso" />
    </div>
    <br />

    <div>
      Altura:
      <input type="number" v-model="pessoaAltura" />
    </div>
    <br />
    <button @click="calcularImc(pessoaPeso, pessoaAltura), corFaixa()">
      Calcular
    </button>

    <div v-bind:class="[{ 'no-result': !isResult, result: isResult }, cor]">
      <!--{{ pessoaPeso }} / {{ pessoaAltura }}-->
      <h2>IMC: {{ resultado.imc }}</h2>
      <h5>{{ resultado.peso }}</h5>
      <h5>{{ resultado.faixa }}</h5>
      <h3 v-bind:class="['item-cor']">
        {{ resultado.resultado }}
        <span v-if="resultado.quilos != 0">{{ resultado.quilos }} kg</span>
      </h3>
    </div>
  </div>
</template>

<script>
// Tratar classe resultado para sumir enquanto não tiver resultado
// Cores para exibir no IMC [amarelo: Ganhar, verde: Manter, Vermelho: perder]

export default {
  name: "CalculadoraImc",
  props: {},
  data() {
    return {
      pessoaPeso: "",
      pessoaAltura: "",
      resultado: "",
      isResult: false,
      cor: "",
    };
  },
  methods: {
    calcularImc(peso, altura) {
      if (peso !== "" && altura !== "") {
        this.isResult = true;
      } else {
        this.isResult = false;
      }

      function calcular() {
        let calc = peso / (altura * altura);
        return calc.toFixed(2);
      }

      function imc() {
        let calc = calcular();
        let resultado;

        if (calc < 18.5) {
          resultado = "Abaixo do peso";
        } else if (calc >= 18.5 && calc <= 24.9) {
          resultado = "Peso normal";
        } else if (calc >= 25 && calc <= 29.9) {
          resultado = "Sobrepeso";
        } else if (calc >= 30 && calc <= 34.9) {
          resultado = "Obesidade grau 1";
        } else if (calc >= 35 && calc <= 39.9) {
          resultado = "Obesidade grau 2";
        } else if (calc >= 40) {
          resultado = "Obesidade grau 3";
        }

        return resultado;
      }
      function pesoIdeal() {
        let objetivo;

        if (altura <= 1.5) {
          objetivo = variarPesoEntre(41, 54);
        } else if (altura > 1.5 && altura <= 1.53) {
          objetivo = variarPesoEntre(43, 58);
        } else if (altura >= 1.54 && altura <= 1.55) {
          objetivo = variarPesoEntre(45, 59);
        } else if (altura >= 1.56 && altura <= 1.57) {
          objetivo = variarPesoEntre(47, 61);
        } else if (altura >= 1.58 && altura <= 1.6) {
          objetivo = variarPesoEntre(47, 63);
        } else if (altura >= 1.61 && altura <= 1.63) {
          objetivo = variarPesoEntre(50, 65);
        } else if (altura >= 1.64 && altura <= 1.65) {
          objetivo = variarPesoEntre(52, 65);
        } else if (altura >= 1.66 && altura <= 1.67) {
          objetivo = variarPesoEntre(52, 68);
        } else if (altura >= 1.68 && altura <= 1.7) {
          objetivo = variarPesoEntre(54, 78);
        } else if (altura >= 1.71 && altura <= 1.73) {
          objetivo = variarPesoEntre(56, 72);
        } else if (altura >= 1.74 && altura <= 1.75) {
          objetivo = variarPesoEntre(56, 75);
        } else if (altura >= 1.76 && altura <= 1.78) {
          objetivo = variarPesoEntre(59, 77);
        } else if (altura >= 1.79 && altura <= 1.8) {
          objetivo = variarPesoEntre(61, 79);
        } else if (altura >= 1.81 && altura <= 1.83) {
          objetivo = variarPesoEntre(63, 81);
        } else if (altura >= 1.84 && altura <= 1.86) {
          objetivo = variarPesoEntre(66, 86);
        } else if (altura >= 1.87 && altura <= 1.88) {
          objetivo = variarPesoEntre(66, 88);
        } else if (altura >= 1.89) {
          objetivo = variarPesoEntre(68, 90);
        }
        return {
          resultado: objetivo.texto,
          objetivo: objetivo.calcular,
          perdeGanha: objetivo.perdeGanha,
        };
      }
      function variarPesoEntre(pesoMin = 0, pesoMax = 100) {
        let calcular;
        let perdeGanha;

        if (peso >= pesoMin && peso <= pesoMax) {
          calcular = 0;
          perdeGanha = "Manter na faixa";
        } else {
          calcular = peso - pesoMax;
          perdeGanha = "Perder";

          if (calcular < 0) {
            calcular = pesoMin - peso;
            perdeGanha = "Ganhar";
          }
        }

        return {
          calcular: calcular,
          texto: `Peso ideal entre ${pesoMin} e ${pesoMax} kg`,
          perdeGanha: perdeGanha,
        };
      }

      return (this.resultado = {
        pessoa: [altura, peso],
        imc: calcular(),
        faixa: imc(),
        peso: pesoIdeal().resultado,
        resultado: pesoIdeal().perdeGanha,
        quilos: pesoIdeal().objetivo,
      });

      /*
      return {
        pessoa: [altura, peso],
        imc: calcular(),
        faixa: imc(),
        peso: pesoIdeal().resultado,
        resultado: pesoIdeal().perdeGanha,
        quilos: pesoIdeal().objetivo,
      };*/
    },
    corFaixa() {
      let faixa = this.resultado.resultado;
      let color;

      if (faixa === "Manter na faixa") {
        color = "verde";
      } else if (faixa === "Perder") {
        color = "vermelho";
      } else if (faixa === "Ganhar") {
        color = "amarelo";
      }

      return (this.cor = color);
    },
  },
  computed: {},
  watch: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input:focus,
button:focus {
  outline: 0px;
}
input,
button {
  padding: 10px;
  border-radius: 10px;
}
input {
  width: 80px;
  font-size: 1.2rem;
  background: rgba(242, 244, 246, 1);
  border: 1px solid #ccc;
}
button {
  cursor: pointer;
  background: rgba(13, 63, 103, 1);
  color: rgba(242, 244, 246, 1);
  padding: 10px 25px;
  border: 0px;
}
h5 {
  margin: 5px;
}

.no-result {
  display: none;
}
.result {
  border-radius: 10px;
  max-width: 210px;
  width: 95%;
  padding: 20px 20px 20px 160px;
  margin: auto;
  margin-top: 50px;
  border: 5px solid rgba(13, 63, 103, 1);
  display: block;
  box-shadow: 2px 2px 7px rgb(0 0 0 / 30%);
}
.result.vermelho {
  background: url("data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaWQ9IkxheWVyXzEiIHN0eWxlPSJlbmFibGUtYmFja2dyb3VuZDpuZXcgMCAwIDEyOCAxMjg7IiB2ZXJzaW9uPSIxLjEiIHZpZXdCb3g9IjAgMCAxMjggMTI4IiB4bWw6c3BhY2U9InByZXNlcnZlIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIj48c3R5bGUgdHlwZT0idGV4dC9jc3MiPgoJLnN0MHtmaWxsOiMzODI2NzM7fQoJLnN0MXtmaWxsOiM0NzMwODA7fQoJLnN0MntmaWxsOiMwMDdCQzY7fQoJLnN0M3tmaWxsOiNENDRBOTA7fQoJLnN0NHtmaWxsOiNBNzI5NzM7fQoJLnN0NXtmaWxsOiNGMTVBOUU7fQoJLnN0NntvcGFjaXR5OjAuNDtmaWxsOiMwQ0FGRTg7fQoJLnN0N3tvcGFjaXR5OjAuNTtmaWxsOiMzODI2NzM7fQoJLnN0OHtmaWxsOiMwQ0FGRTg7fQoJLnN0OXtmaWxsOiMwQjkwRDM7fQoJLnN0MTB7b3BhY2l0eTowLjM7ZmlsbDpub25lO3N0cm9rZTojRkZGRkZGO3N0cm9rZS13aWR0aDoyO3N0cm9rZS1saW5lY2FwOnJvdW5kO3N0cm9rZS1saW5lam9pbjpyb3VuZDtzdHJva2UtbWl0ZXJsaW1pdDoxMDt9Cgkuc3QxMXtmaWxsOiM4RjU4QjI7fQoJLnN0MTJ7ZmlsbDojNkU0Q0EwO30KCS5zdDEze2ZpbGw6I0YzNzVBRDt9Cgkuc3QxNHtmaWxsOiNGRkRFMzk7fQoJLnN0MTV7ZmlsbDojRkVDNjMyO30KCS5zdDE2e2ZpbGw6I0Y5QTc0RTt9Cgkuc3QxN3tmaWxsOiNEQTZBMkQ7fQoJLnN0MTh7ZmlsbDojOUVERDlBO30KCS5zdDE5e2ZpbGw6IzAwQzJBOTt9Cgkuc3QyMHtmaWxsOiMwMEE1OEM7fQoJLnN0MjF7ZmlsbDojMDM3QzY4O30KCS5zdDIye2ZpbGw6IzlBQzBEQjt9Cgkuc3QyM3tmaWxsOiNGRkZGRkY7fQoJLnN0MjR7b3BhY2l0eTowLjI7ZmlsbDojMzgyNjczO30KCS5zdDI1e29wYWNpdHk6MC40O2ZpbGw6IzQ3MzA4MDt9Cgkuc3QyNntvcGFjaXR5OjAuMTtmaWxsOiMzODI2NzM7fQoJLnN0Mjd7ZmlsbDpub25lO3N0cm9rZTojRkZERTM5O3N0cm9rZS13aWR0aDoxMDtzdHJva2UtbGluZWNhcDpyb3VuZDtzdHJva2UtbGluZWpvaW46cm91bmQ7c3Ryb2tlLW1pdGVybGltaXQ6MTA7fQoJLnN0Mjh7ZmlsbDpub25lO3N0cm9rZTojRjlBNzRFO3N0cm9rZS13aWR0aDo0O3N0cm9rZS1saW5lY2FwOnJvdW5kO3N0cm9rZS1saW5lam9pbjpyb3VuZDtzdHJva2UtbWl0ZXJsaW1pdDoxMDt9Cgkuc3QyOXtmaWxsOm5vbmU7c3Ryb2tlOiNGRUM2MzI7c3Ryb2tlLXdpZHRoOjQ7c3Ryb2tlLWxpbmVjYXA6cm91bmQ7c3Ryb2tlLWxpbmVqb2luOnJvdW5kO3N0cm9rZS1taXRlcmxpbWl0OjEwO30KCS5zdDMwe2ZpbGw6bm9uZTtzdHJva2U6IzAwQTU4QztzdHJva2Utd2lkdGg6MjtzdHJva2UtbGluZWNhcDpyb3VuZDtzdHJva2UtbGluZWpvaW46cm91bmQ7c3Ryb2tlLW1pdGVybGltaXQ6MTA7fQoJLnN0MzF7ZmlsbDpub25lO3N0cm9rZTojRDQ0QTkwO3N0cm9rZS13aWR0aDozO3N0cm9rZS1saW5lY2FwOnJvdW5kO3N0cm9rZS1saW5lam9pbjpyb3VuZDtzdHJva2UtbWl0ZXJsaW1pdDoxMDt9Cgkuc3QzMntmaWxsOm5vbmU7c3Ryb2tlOiMwMDdCQzY7c3Ryb2tlLXdpZHRoOjIuMjQzOTtzdHJva2UtbWl0ZXJsaW1pdDoxMDt9Cgkuc3QzM3tmaWxsOm5vbmU7c3Ryb2tlOiNGMTVBOUU7c3Ryb2tlLXdpZHRoOjM7c3Ryb2tlLWxpbmVjYXA6cm91bmQ7c3Ryb2tlLW1pdGVybGltaXQ6MTA7fQoJLnN0MzR7b3BhY2l0eTowLjI7ZmlsbDpub25lO3N0cm9rZTojMzgyNjczO3N0cm9rZS13aWR0aDoyO3N0cm9rZS1saW5lY2FwOnJvdW5kO3N0cm9rZS1saW5lam9pbjpyb3VuZDtzdHJva2UtbWl0ZXJsaW1pdDoxMDt9Cgkuc3QzNXtmaWxsOm5vbmU7c3Ryb2tlOiNGMTVBOUU7c3Ryb2tlLXdpZHRoOjI7c3Ryb2tlLWxpbmVjYXA6cm91bmQ7c3Ryb2tlLW1pdGVybGltaXQ6MTA7fQoJLnN0MzZ7ZmlsbDpub25lO3N0cm9rZTojRjlBNzRFO3N0cm9rZS13aWR0aDoyO3N0cm9rZS1saW5lY2FwOnJvdW5kO3N0cm9rZS1saW5lam9pbjpyb3VuZDtzdHJva2UtbWl0ZXJsaW1pdDoxMDt9Cgkuc3QzN3tmaWxsOm5vbmU7c3Ryb2tlOiNGRkZGRkY7c3Ryb2tlLXdpZHRoOjI7c3Ryb2tlLWxpbmVjYXA6cm91bmQ7c3Ryb2tlLW1pdGVybGltaXQ6MTA7fQoJLnN0Mzh7b3BhY2l0eTowLjQ7ZmlsbDojMzgyNjczO30KPC9zdHlsZT48Y2lyY2xlIGNsYXNzPSJzdDAiIGN4PSI2NCIgY3k9IjY0IiByPSI2MCIvPjxjaXJjbGUgY2xhc3M9InN0MSIgY3g9IjY0IiBjeT0iNjQiIHI9IjUwIi8+PHBhdGggY2xhc3M9InN0MjAiIGQ9Ik0xMTEuMiwxMDFIMTYuOGMxMSwxNCwyOCwyMyw0Ny4yLDIzUzEwMC4yLDExNSwxMTEuMiwxMDF6Ii8+PHBhdGggY2xhc3M9InN0MTQiIGQ9Ik00OC40LDQ3LjlsMTYuNiwxMi43bDM0LjQtNDVjLTYuNC00LjctMTMuOC04LjMtMjEuOC0xMC4xTDQ4LjQsNDcuOXoiLz48cGF0aCBjbGFzcz0ic3QxNSIgZD0iTTk1LjMsMTIuOGwtMzMsNDUuN2wyLjYsMmwzNC40LTQ1Qzk4LDE0LjYsOTYuNywxMy43LDk1LjMsMTIuOHoiLz48cG9seWdvbiBjbGFzcz0ic3QyMyIgcG9pbnRzPSI4MC4xLDQwLjYgNjIuNywyNy4xIDUwLjEsNDUuNCA0OC41LDQ4IDY0LjksNjAuNSAiLz48cG9seWdvbiBjbGFzcz0ic3QyNCIgcG9pbnRzPSI3NywzOC4yIDYyLjMsNTguNiA2NC45LDYwLjUgODAuMSw0MC42ICIvPjxwYXRoIGNsYXNzPSJzdDE5IiBkPSJNOTguNSwxMDNoLTY5Yy0xLjQsMC0yLjUtMS4xLTIuNS0yLjVsMCwwYzAtMS40LDEuMS0yLjUsMi41LTIuNWg2OWMxLjQsMCwyLjUsMS4xLDIuNSwyLjVsMCwwICBDMTAxLDEwMS45LDk5LjksMTAzLDk4LjUsMTAzeiIvPjxwYXRoIGNsYXNzPSJzdDUiIGQ9Ik03MS44LDc2LjVWNTYuM2MtNi43LDAuNC0xMy02LjUtMTkuMy0xNS4yTDMwLjYsNjIuMUw2NS41LDk1SDkzdi0zLjljMC0zLjctMi4zLTctNS43LTguNEw3MS44LDc2LjV6Ii8+PHBhdGggY2xhc3M9InN0MyIgZD0iTTQzLjYsNzQuNGMzLjEtMy4xLDUuMS03LjUsNS4xLTEyLjJjMC00LjgtMi05LjItNS4yLTEyLjRjLTUuNywzLjQtMTAuMSw3LjUtMTIuOCwxMi40TDQzLjYsNzQuNHoiLz48Zz48cGF0aCBjbGFzcz0ic3QyMyIgZD0iTTk2LDk2YzAsMS4xLTAuOSwyLTIsMkg2NS41Yy0wLjgsMC0xLjUtMC4zLTIuMS0wLjhMMjcuOCw2My42Yy0wLjgtMC44LTAuOC0yLTAuMS0yLjhsMS40LTEuNSAgIGMwLjgtMC44LDItMC44LDIuOC0wLjFMNjYuNyw5Mkg5NGMxLjEsMCwyLDAuOSwyLDJWOTZ6Ii8+PC9nPjxwYXRoIGNsYXNzPSJzdDQiIGQ9Ik02OS4yLDUxYy0xLjgsMC0zLjIsMS41LTMuMiwzLjJjMCw1LjYsMCwxNi43LDAsMTYuNGMwLDMuMiwyLjYsNS44LDYsNS44di01Ljh2LTUuOFY1MUg2OS4yeiIvPjxwYXRoIGNsYXNzPSJzdDMiIGQ9Ik03MCw5MmgyM3YtMC45YzAtMS45LTAuNi0zLjctMS42LTUuMUg3NkM3Mi43LDg2LDcwLDg4LjcsNzAsOTJ6Ii8+PHBhdGggY2xhc3M9InN0MjMiIGQ9Ik01MS4yLDg2LjZsLTEwLjQtOS43Yy0wLjUtMC40LTAuNC0xLjIsMC4xLTEuNWw0LjktMy42YzAuNC0wLjMsMC45LTAuMiwxLjMsMC4xbDguOCw4LjIgIGMwLjQsMC4zLDAuNCwwLjksMC4yLDEuM2wtMy4zLDUuMUM1Mi40LDg3LDUxLjcsODcuMSw1MS4yLDg2LjZ6Ii8+PHBhdGggY2xhc3M9InN0MjQiIGQ9Ik01MS4yLDg2LjZsLTEwLjQtOS43Yy0wLjUtMC40LTAuNC0xLjIsMC4xLTEuNWw0LjktMy42YzAuNC0wLjMsMC45LTAuMiwxLjMsMC4xbDguOCw4LjIgIGMwLjQsMC4zLDAuNCwwLjksMC4yLDEuM2wtMy4zLDUuMUM1Mi40LDg3LDUxLjcsODcuMSw1MS4yLDg2LjZ6Ii8+PHBhdGggY2xhc3M9InN0MyIgZD0iTTQ4LjMsNDUuMWwzLjMtNC44YzAuNy0xLDIuMi0xLjIsMy4xLTAuM2wyLjMsMi4zYzAuMiwwLjIsMC4zLDAuNSwwLjMsMC44Yy0wLjEsMS4xLTAuMywzLjksMS4zLDUuNSAgQzU0LjMsNTAuMiw0OC4zLDQ1LjEsNDguMyw0NS4xeiIvPjxsaW5lIGNsYXNzPSJzdDMxIiB4MT0iNzEuOCIgeDI9IjY0LjgiIHkxPSI1OS44IiB5Mj0iNTkuOCIvPjxsaW5lIGNsYXNzPSJzdDMxIiB4MT0iNzEuOCIgeDI9IjY0LjgiIHkxPSI2NS44IiB5Mj0iNjUuOCIvPjxsaW5lIGNsYXNzPSJzdDMxIiB4MT0iNzEuOCIgeDI9IjY0LjgiIHkxPSI3MS44IiB5Mj0iNzEuOCIvPjwvc3ZnPg==")
    no-repeat;
  background-position: -90px center;
}
.result.verde {
  background: url("data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI2MCIgdmlld0JveD0iMCAwIDYwIDYwIiB3aWR0aD0iNjAiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGRlZnM+PHN0eWxlPgogICAgICAuY2xzLTEgewogICAgICAgIGZpbGw6ICM2OTlmNGM7CiAgICAgICAgZmlsbC1ydWxlOiBldmVub2RkOwogICAgICB9CiAgICA8L3N0eWxlPjwvZGVmcz48cGF0aCBjbGFzcz0iY2xzLTEiIGQ9Ik04MDAsNTEwYTMwLDMwLDAsMSwxLDMwLTMwQTMwLDMwLDAsMCwxLDgwMCw1MTBabS0xNi45ODYtMjMuMjM1YTMuNDg0LDMuNDg0LDAsMCwxLDAtNC45bDEuNzY2LTEuNzU2YTMuMTg1LDMuMTg1LDAsMCwxLDQuNTc0LjA1MWwzLjEyLDMuMjM3YTEuNTkyLDEuNTkyLDAsMCwwLDIuMzExLDBsMTUuOS0xNi4zOWEzLjE4NywzLjE4NywwLDAsMSw0LjYtLjAyN0w4MTcsNDY4LjcxNGEzLjQ4MiwzLjQ4MiwwLDAsMSwwLDQuODQ2bC0yMS4xMDksMjEuNDUxYTMuMTg1LDMuMTg1LDAsMCwxLTQuNTUyLjAzWiIgaWQ9ImNoZWNrIiB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtNzcwIC00NTApIi8+PC9zdmc+")
    no-repeat;
  background-position: -30px center;
  background-size: auto 75%;
}
.result.amarelo {
  background: url("data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaWQ9IkxheWVyXzFfMV8iIHN0eWxlPSJlbmFibGUtYmFja2dyb3VuZDpuZXcgMCAwIDY0IDY0OyIgdmVyc2lvbj0iMS4xIiB2aWV3Qm94PSIwIDAgNjQgNjQiIHhtbDpzcGFjZT0icHJlc2VydmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiPjxnPjxnPjxwYXRoIGQ9Ik02MS41NzcsMjYuOTk2QzU5LjIxMiwxNC40MDUsNDkuNTk1LDQuNzg4LDM3LjAwNCwyLjQyM2MtMC4yMzktMC4wNC0wLjQ3OS0wLjA3Ny0wLjcyLTAuMTEyICAgIEMzNS4wOCwyLjEzOSwzNCwzLjA4NCwzNCw0LjMwMVYzMGgyNS42OTljMS4yMTcsMCwyLjE2MS0xLjA4LDEuOTg5LTIuMjg0QzYxLjY1NCwyNy40NzUsNjEuNjE3LDI3LjIzNSw2MS41NzcsMjYuOTk2eiIgc3R5bGU9ImZpbGw6I0NGOUU3NjsiLz48cGF0aCBkPSJNMjcuODU0LDYuMDgxQzEyLjgxMiw3LjIyMSwxLjA5MSwyMC4yNDUsMi4wNTYsMzUuNzg2QzIuOTIxLDQ5LjczLDE0LjI3LDYxLjA3OSwyOC4yMTMsNjEuOTQ0ICAgIGMxNS41NDEsMC45NjUsMjguNTY2LTEwLjc1NywyOS43MDYtMjUuNzk4QzU4LjAwNywzNC45ODgsNTcuMDc4LDM0LDU1LjkxNiwzNEgzMFY4LjA4NEMzMCw2LjkyMiwyOS4wMTIsNS45OTMsMjcuODU0LDYuMDgxeiIgc3R5bGU9ImZpbGw6I0NGOUU3NjsiLz48cGF0aCBkPSJNMzAsOUMxNi4xOTMsOSw1LDIwLjE5Myw1LDM0czExLjE5MywyNSwyNSwyNXMyNS0xMS4xOTMsMjUtMjVIMzBWOXoiIHN0eWxlPSJmaWxsOiNERkIyOEI7Ii8+PHBhdGggZD0iTTM0LDV2MjVoMjVDNTksMTYuNzQ1LDQ3LjI1NSw1LDM0LDV6IiBzdHlsZT0iZmlsbDojREZCMjhCOyIvPjxwYXRoIGQ9Ik01NS45MTEsMzBjLTAuODI1LTkuOTg2LTcuNzU5LTE4LjIzNS0xNy4wNTktMjFMMzcsOC41MjljLTAuOTc5LTAuMjA4LTEuOTgxLTAuMzU1LTMtMC40NFYzMCAgICBINTUuOTExeiIgc3R5bGU9ImZpbGw6I0ZDRDc3MDsiLz48cGF0aCBkPSJNMzAsMzRWMTJDMTcuODUsMTIsOCwyMS44NSw4LDM0czkuODUsMjIsMjIsMjJzMjItOS44NSwyMi0yMkgzMHoiIHN0eWxlPSJmaWxsOiNGQ0Q3NzA7Ii8+PGNpcmNsZSBjeD0iMjMiIGN5PSI0OSIgcj0iMiIgc3R5bGU9ImZpbGw6I0ZDNkU1MTsiLz48Y2lyY2xlIGN4PSI0MCIgY3k9IjI0IiByPSIyIiBzdHlsZT0iZmlsbDojQjI3OTQ2OyIvPjxjaXJjbGUgY3g9IjQ4IiBjeT0iMTciIHI9IjIiIHN0eWxlPSJmaWxsOiNGQzZFNTE7Ii8+PGNpcmNsZSBjeD0iMjYiIGN5PSIzNiIgcj0iMiIgc3R5bGU9ImZpbGw6I0IyNzk0NjsiLz48Y2lyY2xlIGN4PSI0NCIgY3k9IjQyIiByPSIyIiBzdHlsZT0iZmlsbDojQjI3OTQ2OyIvPjxjaXJjbGUgY3g9IjE2IiBjeT0iMzQiIHI9IjIiIHN0eWxlPSJmaWxsOiNGQzZFNTE7Ii8+PC9nPjxnPjxwYXRoIGQ9Ik01NS45MTYsMzNIMzFWOC4wODRjMC0wLjgzNS0wLjM1Mi0xLjY0MS0wLjk2Ni0yLjIxYy0wLjYwOS0wLjU2My0xLjQyMy0wLjg1Mi0yLjI1Ni0wLjc5ICAgIGMtMTUuNzEzLDEuMTktMjcuNywxNC45OTEtMjYuNzIxLDMwLjc2NEMxLjk2LDUwLjM5MiwxMy42MDcsNjIuMDQsMjguMTUxLDYyLjk0MmMwLjYwNCwwLjAzNywxLjIwNCwwLjA1NiwxLjgwMiwwLjA1NiAgICBjMTUuMDA5LTAuMDAxLDI3LjgxOC0xMS42NjUsMjguOTYzLTI2Ljc3NmMwLjA2Mi0wLjgyNC0wLjIyNi0xLjY0Ni0wLjc5LTIuMjU2QzU3LjU1NywzMy4zNTIsNTYuNzUxLDMzLDU1LjkxNiwzM3ogICAgIE0yOC4yNzQsNjAuOTQ2QzE0LjczNiw2MC4xMDYsMy44OTQsNDkuMjYzLDMuMDU0LDM1LjcyNUMyLjE0MiwyMS4wMzcsMTMuMzAxLDguMTg3LDI3LjkyOSw3LjA3OCAgICBjMC4yNzEtMC4wMjQsMC41NDEsMC4wNzEsMC43NDYsMC4yNjNDMjguODg1LDcuNTM2LDI5LDcuOCwyOSw4LjA4NHYyLjk0MUMxNi43OCwxMS41NTEsNywyMS42NTMsNywzNGMwLDEyLjY4MywxMC4zMTcsMjMsMjMsMjMgICAgYzEwLjc1OCwwLDE5Ljk3MS03LjMwOSwyMi40MDMtMTcuNzczbC0xLjk0Ny0wLjQ1M0M0OC4yMzQsNDguMzI3LDM5LjgyMyw1NSwzMCw1NUMxOC40MjEsNTUsOSw0NS41NzksOSwzNCAgICBjMC0zLjA5MSwwLjY4OC02LjAxOCwxLjg5My04LjY2NGMwLjEyMSwwLjEwMSwwLjI0OCwwLjIxOCwwLjQsMC4zNzFDMTEuODM5LDI2LjI1MywxMi41ODYsMjcsMTQsMjdzMi4xNjEtMC43NDcsMi43MDctMS4yOTMgICAgQzE3LjE5NywyNS4yMTcsMTcuNDM4LDI1LDE4LDI1di0yYy0xLjQxNCwwLTIuMTYxLDAuNzQ3LTIuNzA3LDEuMjkzQzE0LjgwMywyNC43ODMsMTQuNTYyLDI1LDE0LDI1cy0wLjgwMy0wLjIxNy0xLjI5My0wLjcwNyAgICBjLTAuMjQxLTAuMjQxLTAuNTI3LTAuNTItMC44ODctMC43NThjMy40NzUtNi4wMTMsOS44MzUtMTAuMTM2LDE3LjE4LTEwLjQ4NVYzNWgyMS45NjZjLTAuMDI5LDAuNjI2LTAuMDc2LDEuMjUtMC4xNiwxLjg2NSAgICBsMS45ODIsMC4yN2MwLjA5Ni0wLjcwNCwwLjE0Ny0xLjQxOSwwLjE3OC0yLjEzNWgyLjk1YzAuMjg0LDAsMC41NDgsMC4xMTUsMC43NDMsMC4zMjVjMC4xOSwwLjIwNiwwLjI4MywwLjQ3MSwwLjI2MywwLjc0NiAgICBDNTUuODEyLDUwLjY5OSw0Mi45OCw2MS44NjksMjguMjc0LDYwLjk0NnoiLz48cGF0aCBkPSJNMjMsNDZjLTEuNjU0LDAtMywxLjM0Ni0zLDNzMS4zNDYsMywzLDNzMy0xLjM0NiwzLTNTMjQuNjU0LDQ2LDIzLDQ2eiBNMjMsNTBjLTAuNTUyLDAtMS0wLjQ0OC0xLTFzMC40NDgtMSwxLTEgICAgczEsMC40NDgsMSwxUzIzLjU1Miw1MCwyMyw1MHoiLz48cGF0aCBkPSJNMzcsMjRjMCwxLjY1NCwxLjM0NiwzLDMsM3MzLTEuMzQ2LDMtM3MtMS4zNDYtMy0zLTNTMzcsMjIuMzQ2LDM3LDI0eiBNNDEsMjRjMCwwLjU1Mi0wLjQ0OCwxLTEsMXMtMS0wLjQ0OC0xLTEgICAgczAuNDQ4LTEsMS0xUzQxLDIzLjQ0OCw0MSwyNHoiLz48cGF0aCBkPSJNMjYsMzNjLTEuNjU0LDAtMywxLjM0Ni0zLDNzMS4zNDYsMywzLDNzMy0xLjM0NiwzLTNTMjcuNjU0LDMzLDI2LDMzeiBNMjYsMzdjLTAuNTUyLDAtMS0wLjQ0OC0xLTFzMC40NDgtMSwxLTEgICAgczEsMC40NDgsMSwxUzI2LjU1MiwzNywyNiwzN3oiLz48cGF0aCBkPSJNNDQsMzljLTEuNjU0LDAtMywxLjM0Ni0zLDNzMS4zNDYsMywzLDNzMy0xLjM0NiwzLTNTNDUuNjU0LDM5LDQ0LDM5eiBNNDQsNDNjLTAuNTUyLDAtMS0wLjQ0OC0xLTFzMC40NDgtMSwxLTEgICAgczEsMC40NDgsMSwxUzQ0LjU1Miw0Myw0NCw0M3oiLz48cGF0aCBkPSJNMTYsMzFjLTEuNjU0LDAtMywxLjM0Ni0zLDNzMS4zNDYsMywzLDNzMy0xLjM0NiwzLTNTMTcuNjU0LDMxLDE2LDMxeiBNMTYsMzVjLTAuNTUyLDAtMS0wLjQ0OC0xLTFzMC40NDgtMSwxLTEgICAgczEsMC40NDgsMSwxUzE2LjU1MiwzNSwxNiwzNXoiLz48cGF0aCBkPSJNMzUsNTBjLTAuNTYyLDAtMC44MDMtMC4yMTctMS4yOTMtMC43MDdDMzMuMTYxLDQ4Ljc0NywzMi40MTQsNDgsMzEsNDh2MmMwLjU2MiwwLDAuODAzLDAuMjE3LDEuMjkzLDAuNzA3ICAgIEMzMi44MzksNTEuMjUzLDMzLjU4Niw1MiwzNSw1MnMyLjE2MS0wLjc0NywyLjcwNy0xLjI5M0MzOC4xOTcsNTAuMjE3LDM4LjQzOCw1MCwzOSw1MHYtMmMtMS40MTQsMC0yLjE2MSwwLjc0Ny0yLjcwNywxLjI5MyAgICBDMzUuODAzLDQ5Ljc4MywzNS41NjIsNTAsMzUsNTB6Ii8+PHBhdGggZD0iTTYyLjY3OSwyNy41NzVjLTAuMDM2LTAuMjUxLTAuMDc0LTAuNTAxLTAuMTE3LTAuNzQ5bC0wLjAwMy0wLjAxNUM2MC4xMjIsMTMuODM2LDUwLjE2NCwzLjg3OCwzNy4xNzQsMS40MzggICAgYy0wLjI0OC0wLjA0My0wLjQ5OC0wLjA4MS0wLjc0OC0wLjExN2MtMC44NjItMC4xMi0xLjcyOSwwLjEzNi0yLjM4NywwLjcwNUMzMy4zNzksMi42LDMzLDMuNDI5LDMzLDQuMzAxVjMxaDI2LjY5OSAgICBjMC44NzIsMCwxLjcwMS0wLjM3OSwyLjI3NC0xLjAzOUM2Mi41NDQsMjkuMzAzLDYyLjgwMSwyOC40MzQsNjIuNjc5LDI3LjU3NXogTTYwLjQ2MywyOC42NUM2MC4yNjcsMjguODc2LDU5Ljk5NiwyOSw1OS42OTksMjkgICAgaC0yLjkwNGMtMS4xODItOS43OC04LjE2Ny0xOC4xMzUtMTcuNjU5LTIwLjk1OWwtMC41NywxLjkxOGMzLjAxOCwwLjg5Nyw1Ljc1MiwyLjQxMyw4LjA5Nyw0LjM3QzQ1LjY4MiwxNC44MjIsNDUsMTUuODI4LDQ1LDE3ICAgIGMwLDEuNjU0LDEuMzQ2LDMsMywzYzEuMTAxLDAsMi4wNTUtMC42MDMsMi41NzctMS40OWMyLjIyMSwzLjA1MywzLjY5OCw2LjY0Niw0LjIwNCwxMC40OUgzNVYxNy40MjUgICAgYzAuMDkxLDAuMDgzLDAuMTg0LDAuMTczLDAuMjkzLDAuMjgyQzM1LjgzOSwxOC4yNTMsMzYuNTg2LDE5LDM4LDE5czIuMTYxLTAuNzQ3LDIuNzA3LTEuMjkzQzQxLjE5NywxNy4yMTcsNDEuNDM4LDE3LDQyLDE3di0yICAgIGMtMS40MTQsMC0yLjE2MSwwLjc0Ny0yLjcwNywxLjI5M0MzOC44MDMsMTYuNzgzLDM4LjU2MiwxNywzOCwxN3MtMC44MDMtMC4yMTctMS4yOTMtMC43MDdjLTAuMzk5LTAuMzk5LTAuOTEtMC45MDQtMS43MDctMS4xNDcgICAgVjkuMjFjMC42MDMsMC4wNzgsMS4yMDMsMC4xNzIsMS43OTIsMC4yOTdsMC40MTYtMS45NTdDMzYuNDgzLDcuMzk3LDM1Ljc0Myw3LjI4NywzNSw3LjE5OFY0LjMwMWMwLTAuMjk3LDAuMTI0LTAuNTY3LDAuMzUtMC43NjQgICAgYzAuMjIzLTAuMTkzLDAuNTA3LTAuMjc2LDAuNzk0LTAuMjM1YzAuMjMxLDAuMDMyLDAuNDYyLDAuMDY4LDAuNjc2LDAuMTA0YzEyLjE1NywyLjI4MywyMS40ODcsMTEuNjExLDIzLjc3MiwyMy43NjcgICAgYzAuMDM5LDAuMjI3LDAuMDc0LDAuNDU0LDAuMTA2LDAuNjg1QzYwLjczOSwyOC4xNDYsNjAuNjU2LDI4LjQyOCw2MC40NjMsMjguNjV6IE00OCwxNmMwLjU1MiwwLDEsMC40NDgsMSwxcy0wLjQ0OCwxLTEsMSAgICBzLTEtMC40NDgtMS0xUzQ3LjQ0OCwxNiw0OCwxNnoiLz48cmVjdCBoZWlnaHQ9IjIiIHdpZHRoPSIyIiB4PSI0OSIgeT0iMjMiLz48cmVjdCBoZWlnaHQ9IjIiIHdpZHRoPSIyIiB4PSI0NSIgeT0iMjUiLz48cmVjdCBoZWlnaHQ9IjIiIHdpZHRoPSIyIiB4PSIzNyIgeT0iMzgiLz48cmVjdCBoZWlnaHQ9IjIiIHdpZHRoPSIyIiB4PSIzMSIgeT0iNDEiLz48cmVjdCBoZWlnaHQ9IjIiIHdpZHRoPSIyIiB4PSIxOSIgeT0iMzkiLz48cmVjdCBoZWlnaHQ9IjIiIHdpZHRoPSIyIiB4PSIyNSIgeT0iMTgiLz48cmVjdCBoZWlnaHQ9IjIiIHdpZHRoPSIyIiB4PSIyMiIgeT0iMjUiLz48cmVjdCBoZWlnaHQ9IjIiIHdpZHRoPSIyIiB4PSIxNCIgeT0iNDIiLz48L2c+PC9nPjwvc3ZnPg==")
    no-repeat;
  background-position: -90px center;
}

.item-cor {
  color: #FFF;
  display: inline-flex;
  padding: 10px 15px;
  border-radius: 5px;
}
.item-cor span {
  margin-left: 5px;
}
.vermelho .item-cor {
  background: #E8222D;
}
.amarelo .item-cor {
  background: #FE8402;
}
.verde .item-cor {
  background: #00a03e;
}
</style>
