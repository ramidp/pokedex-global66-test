# pokedex-global66-test
pokedex-global66-test

Link Netlify: https://pokedex-app-global66-test.netlify.app/

PokeDex App - Global66

La verdad es que en comienzo me resulto bastante sencillo el producto, pero tuve algunos conflictos con la parte de la creación de la lista de Favs.
Honestamente no usé muchas tecnologías más allá de lo que proporciona VueJS. Tenia en mente usar FontAwesome y AOS para SVGs y animaciones, pero creo que no fueron necesarias.
Dentro de las ideas que se manejaron, la mas usada obviamente es el render if que tiene VueJS, en algunos casos vi para mejor optimización usar v-show, más que nada con cada Objeto de Pokemon-card donde necesita una mayor optimización para las fotos.
Mi forma de trabajar trata de ser lo mas limpio posible, aunque entiendo que a veces pueda repetirme o generar variables innecesarias.
La info, en este caso, se guarda en localStorage, ya que me pareció lo mas sencillo y algo con lo cual suelo trabajar para guardar arreglos o alguna variable sencilla dentro de la memoria del Browser. Entiendo que no debería usarse si la información fuera confidencial o de mayor importancia, fue el caso.

Hay varios render que tienen animaciones para amejorar la visual y el renderizado de algun componente o algun elemento (Ejemplo: El elemento de error cuando uno busca un pokemon que no se encuentra en la lista. Contiene una pequeña animacion para la mejoría visual y el cargado)

Siempre me gusta crear un objeto theme para anotarme los datos de colores, en algunos casos también lo hago con fuentes, tipo de fuentes, etc, pero en este caso no lo complejice tanto.

Otro método que utilizo es a veces crear las variables importnates y las funciones en la pagina principal (A veces App, a veces Main u otros) y de allí lo voy pasando a los diferentes Componentes Child para también tener un flujo y orden con ello.

Me parecio repetitivo poder generar la pokemoncard en la lista de Favoritos, por eso solo permito que se pueda ver y editar los Favs al estar en la lista de "All". No me gusta ser repetitivo con algunas acciones, aunque entiendo que para los clientes a veces es mejor que se repita a que te limite.

En el mismo código se da info del porque de las cosas, de funciones, variables, etc.


