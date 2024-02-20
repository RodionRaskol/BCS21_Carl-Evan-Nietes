// Pokemon Characteristics
function Pokemon(name, type, health, attacks) {
    this.name = name;
    this.type = type;
    this.health = health;
    this.attacks = attacks;
}

// Trainer Characteristics
function Trainer(name, age, hometown, pokemon) {
    this.name = name;
    this.age = age;
    this.hometown = hometown;
    this.pokemon = pokemon;
}

// Status Display function for trainers
function displayTrainerStatus(trainer) {
    console.log(`Trainer: ${trainer.name}`);
    console.log(`Age: ${trainer.age}`);
    console.log(`Hometown: ${trainer.hometown}`);
    console.log(`Pokemon: ${trainer.pokemon[0].name}\n`);
}

// Function to display Pokemon status when sent out
function displayPokemonStatus(pokemon) {
    console.log(`Name: ${pokemon.name}`);
    console.log(`Type: ${pokemon.type}`);
    console.log(`Health: ${pokemon.health}`);
    console.log(`Attacks: ${JSON.stringify(pokemon.attacks)}\n`);
}

// Pokemon archive
let pikachu = new Pokemon("Pikachu", "Electric", 100, {
    "Tackle": 10,
    "Thunderbolt": 40,
});

let charmander = new Pokemon("Charmander", "Fire", 100, {
    "Tackle": 10,
    "Ember": 20,
});

// Trainer Info
let ash = new Trainer("Ash", 10, "Pallet Town", [pikachu]);
let brock = new Trainer("Brock", 15, "Pewter City", [charmander]);

console.log("Ash was walking around Littleroot town");
console.log("When suddenly a wild Brock appeared and challenged Ash to a duel\n");


// Function to simulate a battle between two Trainers
function battle(trainer1, trainer2) {
    console.log(`Battle begins between Trainer ${trainer1.name} from ${trainer1.hometown} and Trainer ${trainer2.name} from ${trainer2.hometown}!\n`);
    console.log("");
    // Send out Ash's Pokemon first
    let currentTrainer = trainer1;
    let opponentTrainer = trainer2;

    let currentPokemon = currentTrainer.pokemon[0];
    let opponentPokemon = opponentTrainer.pokemon[0];

    console.log(`Trainer ${currentTrainer.name} sends out ${currentPokemon.name}!`);
    displayPokemonStatus(currentPokemon);
    console.log("");

    // Switch trainers
    [currentTrainer, opponentTrainer] = [opponentTrainer, currentTrainer];
    [currentPokemon, opponentPokemon] = [opponentPokemon, currentPokemon];

    // Send out Brock's Pokemon
    console.log(`Trainer ${currentTrainer.name} sends out ${currentPokemon.name}!`);
    displayPokemonStatus(currentPokemon);
    console.log("");

    // Pokémon battle
    // Assume simple mechanics for now: Each Pokémon takes turns attacking until one faints
    while (currentPokemon.health > 0 && opponentPokemon.health > 0) {
        // Pokémon attacks opponent Pokémon
        let attack = getRandomAttack(currentPokemon);
        console.log(`${currentPokemon.name} attacks ${opponentPokemon.name} with ${attack}!`);
        opponentPokemon.health -= currentPokemon.attacks[attack];
        console.log(`${opponentPokemon.name}'s health is now ${opponentPokemon.health}.\n`);

        // Check if opponent Pokémon has fainted
        if (opponentPokemon.health <= 0) {
            console.log(`${opponentPokemon.name} has fainted!\n`);
            // Remove the fainted Pokémon from the opponent's team
            opponentTrainer.pokemon.shift();
            break;
        }

        // Swap turns
        [currentTrainer, opponentTrainer] = [opponentTrainer, currentTrainer];
        [currentPokemon, opponentPokemon] = [opponentPokemon, currentPokemon];
    }

    // Declare the winner
    if (trainer1.pokemon.length === 0) {
        console.log(`Trainer ${trainer2.name} wins the battle!\n`);
    } else {
        console.log(`Trainer ${trainer1.name} wins the battle!\n`);
    }
}

// Function to get a random attack from a Pokemon
function getRandomAttack(pokemon) {
    let attacks = Object.keys(pokemon.attacks);
    let randomIndex = Math.floor(Math.random() * attacks.length);
    return attacks[randomIndex];
}

// Simulate a battle between Ash and Brock
battle(ash, brock);
