<template>
    <div class="container" v-if="juegoMostrado">
        <h1>JUEGO CASINO</h1>
        <p>Puntaje: {{ puntaje }} </p>
        <p>Intento: {{ intento }}</p>
        <div>
            <img :src="poke1.ruta" alt="No se puede mostrar la imagen">
            <img :src="poke2.ruta" alt="No se puede mostrar la imagen">
            <img :src="poke3.ruta" alt="No se puede mostrar la imagen">
        </div>
        <div>
            <p>{{ poke1.nombre }}</p>
            <p>{{ poke2.nombre }}</p>
            <p>{{ poke3.nombre }}</p>
        </div>
        <button v-on:click="jugar">JUGAR</button>
    </div>
    <div class="loss" v-if="perdedorMostrado">
        <h3>Has utilizado tus 5 intentos</h3>
        <h3>El juego ha terminado, intentalo nuevamente</h3>
        <button v-on:click="nuevoJuego">NUEVO JUEGO</button>
    </div>
    <div class="win" v-if="ganadorMostrado">
        <h3>Puntaje: {{ puntaje }} </h3>
        <h3>Felicitaciones has ganado un premio de $10.000,00</h3>
        <button v-on:click="nuevoJuego">NUEVO JUEGO</button>
    </div>
</template>



<script>
//import jugar from "../helpers/clienteAPI";
export default {
    data() {
        return {
            puntaje: 0,
            intento: 0,
            juegoMostrado: true,
            perdedorMostrado: false,
            ganadorMostrado: false,
            poke1: {
                ruta: "https://www.blacksquare.io/wp-content/uploads/2021/03/black-square.jpg",
                nombre: "xxxxxxxxxx"
            },
            poke2: {
                ruta: "https://www.blacksquare.io/wp-content/uploads/2021/03/black-square.jpg",
                nombre: "xxxxxxxxxx"
            },
            poke3: {
                ruta: "https://www.blacksquare.io/wp-content/uploads/2021/03/black-square.jpg",
                nombre: "xxxxxxxxxx"
            }


        }
    }, methods: {

        async jugar() {

            const alea1 = this.obtenerAleatorio(3)

            const alea2 = this.obtenerAleatorio(3)

            const alea3 = this.obtenerAleatorio(3)

            const vectorPlantilla = await this.construirPokemon()

            this.poke1 = vectorPlantilla[alea1]

            this.poke2 = vectorPlantilla[alea2]

            this.poke3 = vectorPlantilla[alea3]

            if (this.intento <= 5) {

                this.intento++

                if (this.poke1.nombre == this.poke2.nombre && this.poke1.nombre == this.poke3.nombre && this.poke2.nombre == this.poke3.nombre) {

                    this.puntaje += 5

                } else if (this.poke1.nombre == this.poke2.nombre || this.poke1.nombre == this.poke3.nombre) {
                    this.puntaje += 2
                }



            }

            if (this.intento == 5) {

                this.juegoMostrado = false

                if (this.puntaje >= 10) {

                    this.ganadorMostrado = true

                } else {

                    this.perdedorMostrado = true

                }

            }

        },

        obtenerAleatorio(max) {

            return Math.floor(Math.random() * max + 1)

        },

        async consumirAPI(id) {

            const data = await fetch(`https://pokeapi.co/api/v2/pokemon/${+id}`).then((r) => r.json())

            return data

        },

        arregloPokemon() {

            const PokemonArray = [24, 25, 26, 27, 28]

            return PokemonArray

        },

        async construirPokemon() {

            const vectorObjetosPokemon = []

            const arreglo = this.arregloPokemon()

            for (let i = 0; i < 4; i++) {

                const idPokemon = arreglo[i]

                const objetoPokemon = await this.construirObjetoPokemon(idPokemon)

                vectorObjetosPokemon.unshift(objetoPokemon)

            }

            return vectorObjetosPokemon

        },

        async construirObjetoPokemon(id) {

            const { name } = await this.consumirAPI(id)

            const objetoPokemon = {

                ruta: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${id}.svg`,

                nombre: name

            }

            return objetoPokemon

        },

        nuevoJuego() {

            this.juegoMostrado = true

            this.ganadorMostrado = false

            this.perdedorMostrado = false

            this.poke1 = {

                ruta: "https://png.pngtree.com/thumb_back/fw800/background/20200821/pngtree-black-solid-color-background-image_396552.jpg",

                nombre: "xxxxxxxx"

            }

            this.poke2 = {

                ruta: "https://png.pngtree.com/thumb_back/fw800/background/20200821/pngtree-black-solid-color-background-image_396552.jpg",

                nombre: "xxxxxxxx"

            }

            this.poke3 = {

                ruta: "https://png.pngtree.com/thumb_back/fw800/background/20200821/pngtree-black-solid-color-background-image_396552.jpg",

                nombre: "xxxxxxxx"

            }

            this.puntaje = 0

            this.intento = 0

        }


    },
}


</script>

<style>
img {
    margin-left: 10px;
    width: 300px;
    height: 300px;
}

p {

    display: inline;

    margin: 15px 125px;

}

button {

    margin-top: 50px;

}

.loss h3 {
    color: red;
}

.win h3 {
    color: blue;
}
</style>