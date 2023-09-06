# pokedex-global66-test
pokedex-global66-test

PokeDex App - Global66

La verdad es que en comienzo me resulto bastante sencillo el producto, pero tuve algunos conflictos con la parte de la creación de la lista de Favs.
Honestamente no usé muchas tecnologías más allá de lo que proporciona VueJS. Tenia en mente usar FontAwesome y AOS para SVGs y animaciones, pero creo que no fueron necesarias.
Dentro de las ideas que se manejaron, la mas usada obviamente es el render if que tiene VueJS, en algunos casos vi para mejor optimización usar v-show, más que nada con cada Objeto de Pokemon-card donde necesita una mayor optimización para las fotos.
Mi forma de trabajar trata de ser lo mas limpio posible, aunque entiendo que a veces pueda repetirme o generar variables innecesarias.
La info, en este caso, se guarda en localStorage, ya que me pareció lo mas sencillo y algo con lo cual suelo trabajar para guardar arreglos o alguna variable sencilla dentro de la memoria del Browser. Entiendo que no debería usarse si la información fuera confidencial o de mayor importancia, fue el caso.
