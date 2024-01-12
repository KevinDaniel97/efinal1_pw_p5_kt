<template>
  <div class="container" v-if="!siguiente">
    <div class="labels">
      <p>Puntaje: {{ puntaje }}</p>
      <p>Intento: {{ intentos }}</p>
    </div>
    <div class="containerComponente">
      <Componente
        :imagen="componentes[0].imagen"
        :texto="componentes[0].texto"
      />
      <Componente
        :imagen="componentes[1].imagen"
        :texto="componentes[1].texto"
      />
      <Componente
        :imagen="componentes[2].imagen"
        :texto="componentes[2].texto"
      />
    </div>
    <button @:click="jugar">JUGAR</button>
  </div>
  <div class="containerMensaje" v-if="siguiente">
    <div class="perdio" v-if="!gano">
      <h1>Has utilizado tus 5 intentos</h1>
      <h1>El juego ha terminado, intentalo nuevamente</h1>
    </div>

    <div class="gano" v-if="gano">
      <h1>Puntaje: {{ puntaje }}</h1>
      <h1>Felicitaciones has ganado un premio de $10.000,00</h1>
    </div>

    <button @click="reiniciar">Nuevo Juego</button>
  </div>
</template>

<script>
import Componente from "./Componente.vue";

export default {
  components: {
    Componente,
  },
  data() {
    return {
      puntaje: 0,
      intentos: 0,
      gano: false,
      siguiente: false,
      componentes: [
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
          texto: "xxxxxxxxxx",
        },
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
          texto: "xxxxxxxxxx",
        },
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
          texto: "xxxxxxxxxx",
        },
      ],
    };
  },

  methods: {
    async consumirAPI() {
      const { answer, image } = await fetch("https://yesno.wtf/api").then(
        (respuesta) => respuesta.json()
      );
      console.log(answer);
      console.log(image);
      return { texto: answer, imagen: image };
    },

    async tresComponentes() {
      const arreglo = [];
      for (let i = 0; i < 3; i++) {
        arreglo.push(await this.consumirAPI());
      }
      this.componentes = arreglo;
    },

    async jugar() {
      this.intentos += 1;

      if (this.puntaje >= 10) {
        this.gano = true;
        this.siguiente = true;
      }

      if (this.intentos > 4) {
        this.gano = false;
        this.siguiente = true;
      } else {
        if (this.puntaje >= 10) {
          this.gano = true;
          this.siguiente = true;
        }
      }

      await this.tresComponentes();
      this.intentosSuma([
        this.componentes[0].texto,
        this.componentes[1].texto,
        this.componentes[2].texto,
      ]);

      console.log(this.componentes[0]);
    },

    intentosSuma([texto1, texto2, texto3] = []) {
      let contadorYes = 0;
      console.log(this.puntaje);
      console.log(texto1);
      console.log(texto2);
      console.log(texto3);
      if (texto1 === "yes") {
        contadorYes++;
      }

      if (texto2 === "yes") {
        contadorYes++;
      }

      if (texto3 === "yes") {
        contadorYes++;
      }

      if (contadorYes === 3) {
        this.puntaje += 5;
        console.log(this.puntaje);
      } else if (contadorYes === 2) {
        this.puntaje += 2;
      } else if (contadorYes === 1) {
        this.puntaje += 1;
      } else {
        this.puntaje += 0;
      }
    },

    reiniciar() {
      this.puntaje = 0;
      this.intentos = 0;
      this.gano = false;
      this.siguiente = false;
      this.componentes = [
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
          texto: "xxxxxxxxxx",
        },
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
          texto: "xxxxxxxxxx",
        },
        {
          imagen:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
          texto: "xxxxxxxxxx",
        },
      ];
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
  justify-content: center;
  justify-items: center;
  border: 5px groove black;
  width: auto;
}
.containerComponente {
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.labels {
  display: flex;
  flex-direction: row;
  gap: 10%;

  margin-bottom: 20px;
}

button {
  margin: 50px;
  height: 50px;
  width: 100px;
  cursor: pointer;
}

.gano {
  color: blue;
}

.perdio {
  color: red;
}
</style>