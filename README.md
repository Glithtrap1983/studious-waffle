# arbuzik
### aboltus
![TIHOHRUN]([https://user-images.githubusercontent.com/60629407/139448835-f652c6bd-02bf-4654-8e25-9d947acf7581.png](https://cs6.pikabu.ru/images/big_size_comm/2015-06_4/1434665311173084326.jpg))
**I'm brr brr patapim**
*There is Example of halal svinota:*
```javascript
let promises = [];
let pokemons;
let resElem = document.getElementById('results');

fetch('https://pokeapi.co/api/v2/pokemon?limit=127')
.then(async function(res){
    let data = await res.json();
    data.results.forEach(element =>{
        let promise = new Promise(function(resolve, reject){
            fetch(element.url)
            .then(async function(res){
                if(res.status == 200){
                    resolve(await res.json());
                } else {
                    reject(res.statusText);
                }
            })
            .catch(reject);
        });
        promises.push(promise);
    });

    Promise.all(promises).then(function(res){
        console.log(res);
        pokemons = res;
        pokemons.forEach(function(pokemon){
            drawPokemon(pokemon);
        });
    });
});

function drawPokemon(pokemon) {
    let pokemonElement = document.createElement('div');
    pokemonElement.classList.add('pokemon');
    pokemonElement.innerHTML = `
        <p>id: #${pokemon.id}</p>
        <hr>
        <h1>${pokemon.name}</h1>
        <img src="${pokemon.sprites.front_default}">
        <div class="stats">
            <p class="hp">&#10084; ${pokemon.stats[0].base_stat}</p>
            <p class="attack">&#9876; ${pokemon.stats[1].base_stat}</p>
            <p class="defence">&#128737; ${pokemon.stats[2].base_stat}</p>
        </div>
    `;
    let typeList = document.createElement('ul');
    populateListWithTypes(pokemon.types, typeList);
    pokemonElement.appendChild(typeList);

    resElem.appendChild(pokemonElement); 
}

function populateListWithTypes(types, ul) {
    types.forEach(function(type){
        let typeItem = document.createElement('li');
        typeItem.innerText = type.type.name;
        ul.appendChild(typeItem);
    });
}

document.getElementById('filter-form').addEventListener('submit', function(){
    if(event.target['name-filter'].value){
        pokemons=pokemons.filter(function(pokemon){
            return pokemon.name.indexOf(event.target['name-filter'].value.toLowerCase()) !== -1;
        });
            }
            if(event.target['hp-filter-from'].value > 10){
                pokemons=pokemons.filter(function(pokemon){
                    return pokemon.stats[0].base_stat >= parseInt (event.target['hp-filter-from'].value);
                }); }
                if(event.target['attack-filter-from'].value > 5){
                    pokemons=pokemons.filter(function(pokemon){
                        return pokemon.stats[1].base_stat >= parseInt (event.target['attack-filter-from'].value);
                    }); }
                    if(event.target['defense-filter-from'].value > 5){
                        pokemons=pokemons.filter(function(pokemon){
                            return pokemon.stats[2].base_stat >= parseInt (event.target['defense-filter-from'].value);
                        }); }
resElem.innerHTML = ''
pokemons.forEach(function(pokemon){
drawPokemon(pokemon);
});
});
