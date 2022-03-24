## Rust Tutorial

mutable - la variable peut être modifiée et sa valeur lue       
immutable - seule la valeur de la variable peut être lue      
Les variables modifiables sont préfixées du mot clé **mut**.        

```Rust
fn main() {
    let mut x = 42;
    x = 13;
}
````
     
#### Type primitifs     
- booléens - **bool** pour représenter vrai/faux    
- entiers non signés - **u8 u32 u64 u128** pour représenter les entiers positifs     
- entiers signés - **i8 i32 i64 i128** pour représenter les entiers positifs et négatifs     
- entiers de taille de pointeur - **usize isize** pour représenter les indices et tailles des éléments en mémoire        
- nombre réel à virgule flottante - **f32 f64** pour représenter notamment les nombres rationnels    
- **tuple** - (valeur, valeur, ...) pour passer une séquence finie de valeurs sur la pile     
- **tableaux** - [valeur, valeur, ...] une collection d'éléments de **même type dont la taille est fixe** (déterminée à la compilation)    
- **slices** - similaire aux tableaux à l'exception que la **taille est redimensionable**    
- chaîne de caractères - **str** pour représenter une suite de caractères
- **pas du null => None**
    
**as** pour passer d'un type a un autre    

```Rust
    const PI: f32 = 3.14159;
    let a = 13u8;
    let b = 7u32;
    let c = a as u32 + b;
    let nums: [i32; 3] = [1, 2, 3];
    println!("{:?}", nums);
    println!("{}", nums[1]);

    fn swap(x: i32, y: i32) -> (i32, i32) {
        return (y, x);
    }
    let result = swap(123, 321); // tuple
    println!("{} {}", result.0, result.1);
    let (a, b) = swap(result.0, result.1); / la destructuration
````

#### Structures de contrôle basiques

- Les opérateurs relationnels et logiques usuels sont disponibles: ==, !=, <, >, <=, >=, !, ||, &&.    
- Si la dernière instruction d'un **if, d'un match, d'une fonction ou d'un bloc d'instructions se termine sans ;**, alors Rust **retournera le résultat** de l'exécution de cette dernière ligne en tant que valeur.     

```Rust
    // condition
    let x = 42;
    if x < 42 {
    } else if x == 42 {
    } else {
    }
    // expression ternaire avec Rust
    let v = if x < 42 { -1 } else { 1 };
 
    // les loops peuvent retourner une valeur
    let mut x = 0;
    let v = loop {
        x += 1;
        if x == 13 {
            break "J'ai trouvé le 13!";
        }
    };
    println!("La boucle dit: {}", v);
    
    // boucles avec break automatique
    let mut x = 0;
    while x != 42 {
        x += 1;
    }
    
    // les for
    for x in 0..5 {
        println!("{}", x);
    }
    
     // les switchs
     match x {
        0 => {
            println!("C'est certainement zero!");
        }
        // on peut comparer à plusieurs valeurs
        1 | 2 => {
            println!("C'est soit 1 soit 2!");
        }
        // on peut comparer à un itérateur
        3..=9 => {
            println!("C'est un nombre compris entre 3 et 9 inclus!");
        }
        // on peut récupérer la valeur
        matched_num @ 10..=100 => {
            println!("L'entier {} est compris entre 10 et 100 inclus!",matched_num);
        }
        // C'est le cas par défaut. Celui-ci doit être présent si tous les casne sont pas présent.
        _ => {
            println!("found something else!");
        }
      }
      
      let v = {
        // ce bloc de code nous permet d'obtenir un résultat
        // sans définir de fonction
        let a = 1;
        let b = 2;
        a + b
      };
````

#### Types basiques de structure de données

-  les méthodes sont des fonctions associées à un type de donnée spécifique.    

```Rust
enum Species { Crab, Octopus, Fish, Clam }
enum PoisonType { Acidic, Painful, Lethal }
enum Size { Big, Small }
enum Weapon {
    Claw(i32, Size),
    Poison(PoisonType),
    None
}

struct SeaCreature {
    species: Species,
    name: String,
    arms: i32,
    legs: i32,
    weapon: Weapon,
}

fn main() {
    // Les données de SeaCreature sont sur la pile
    let ferris = SeaCreature {
        // La structure String est également sur la pile
        // mais maintient une référence vers une donnée sur le tas
        species: Species::Crab,
        name: String::from("Ferris"),
        arms: 2,
        legs: 4,
        weapon: Weapon::Claw(2, Size::Small),
    };

    match ferris.species {
        Species::Crab => {
            match ferris.weapon {
                Weapon::Claw(num_claws,size) => {
                    let size_description = match size {
                        Size::Big => "grandes",
                        Size::Small => "petites"
                    };
                    println!("ferris est un crabe avec {} {} pinces", num_claws, size_description)
                },
                _ => println!("ferris est un crabe avec d'autres armes")
            }
        },
        _ => println!("ferris n'est pas un crabe"),
    }
}

    // Tuples struct
    struct Location(i32, i32);

    // On utilise une méthode statique pour créer une instance de String
    let s = String::from("Hello world!");
    // On utilise ici une méthode directement sur l'instance
    println!("{} contient {} caractères.", s, s.len());
````

#### Request && Response

Le langage Rust met à disposition une énumération générique **Option** qui permet de représenter des valeurs pouvant être nulles sans utiliser null.

````rust
enum Option<T> {
    None,
    Some(T),
}

    // Note: La structure BagOfHolding contiendra éventuellement un élément
    // de type i32, mais ne contient rien pour le moment!
    // Nous devons spécifier le type car autrement Rust ne le connaîtrait pas.
    let i32_bag = BagOfHolding::<i32> { item: None };

    if i32_bag.item.is_none() {
        println!("il n'y a rien dans le sac!")
    } else {
        println!("il y a quelque chose dans le sac!")
    }

    let i32_bag = BagOfHolding::<i32> { item: Some(42) };

    if i32_bag.item.is_some() {
        println!("il y a quelque chose dans le sac!")
    } else {
        println!("il n'y a rien dans le sac!")
    }

    // Avec 'match', nous pouvons déconstruire notre 'Option' élégamment et
    // s'assurer que nous prenions en compte tous les cas.
    match i32_bag.item {
        Some(v) => println!("il y {} dans le sac!", v),
        None => println!("le sac est vide"),
    }
````
    
Le langage Rust met à disposition une énumération générique **Result** qui permet de renvoyer une valeur qui a la possibilité d'échouer. C'est la façon idiomatique avec laquelle le language gère les erreurs.

````rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}

fn do_something_that_might_fail(i:i32) -> Result<f32,String> {
    if i == 13 {
        Ok(13.0)
    } else {
        Err(String::from("ce n'est pas le bon nombre"))   
    }
}

fn main() {
    let result = do_something_that_might_fail(12);

    // Avec 'match', nous pouvons déconstruire notre 'Result' élégamment et
    // s'assurer que nous prenions en compte tous les cas.
    match result {
        Ok(v) => println!("trouvé {}", v),
        Err(e) => println!("Erreur: {}",e),
    }
}
    ````
