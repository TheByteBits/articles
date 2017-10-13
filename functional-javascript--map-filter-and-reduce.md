 # Article map
 - Introduction
 - Functional tools
	 - Array.prototype.map
	 - Array.prototype.filter
	 - Array.prototype.reduce
 - Map, filter and reduce combined


----------


# Functional JavaScript: Map, filter and reduce

## Introduction
Functional programming is powerful paradigm when used correctly, today I will introduce you to basic tools available natively in any recent browser and Node.  

This article is for an inexperienced with functional programming audience as it only presents you the basic tooling from the functional arsenal.  
Examples presented here will make use of some of ES6 features like [block-scoped variables](#), [arrow functions](#) and [template strings](#) although you can swap those easily with standard ES5 alternatives.


## Array.prototype.map
Map is the easiest, it applies given function to each item in an array and returns a new array of the results.  

For an instance an array of values `[ 1, 2, 3 ]` mapped by a function `f = n => n * 2` will take a form of `[ f(1), f(2), f(3) ]` resulting in array of values `[ 1, 4, 9 ]`.


### Examples
Let us look at the example.  
We have a collection of in-game items, each having it's unique parameters.  

Now we want to display name of every item in a chest we looted and print it on the screen.  
Additionaly we would like to make those names of the items all lowercase so it will blend in with our printed sentence nicely.

#### With the map
```javascript
const chest = [
    { name: "Rusty sword", durability: 0.2 },
    { name: "Bark shield", durability: 0.42 },
    { name: "Gold coins", quantity: 12 },
    { name: "Mail helmet", durability: 0.88 },
];

const loot = chest.map( n => n.name.toLowerCase() );
console.log( `You have looted ${ loot.join( ", " ) } from a dead rat.` );


/// loot variable
// [
//     "rusty sword",
//     "bark shield" 
//     "gold coins"
//     "mail helmet" 
// ]

/// console output
// You have looted rusty sword, bark shield, gold coins, mail helmet from a dead rat.
```

#### Without the map
```javascript
const equipment = [
    { name: "Rusty sword", durability: 0.2 },
    { name: "Bark shield", durability: 0.42 },
    { name: "Gold coins", quantity: 12 },
    { name: "Mail helmet", durability: 0.88 },
];

const loot = [];
for ( const item of equipment ) {
    loot.push( item );
}

console.log( `You have looted ${ loot.join( ", " ) } from a dead rat.` );


/// loot variable
// [
//     "rusty sword",
//     "bark shield" 
//     "gold coins"
//     "mail helmet" 
// ]

/// console output
// You have looted rusty sword, bark shield, gold coins, mail helmet from a dead rat.
```


## Array.prototype.filter
The filter, as the name suggests, it filters out from collection the elements that does not return truthy value from passed as an argument function.

The filter operation, as the name suggests, it filters out the elements from an array.  



### Example
Let us tackle an equipment and leveling problem.  

Sir. Funnyface needs to be armed before going out for an adventure, but not all items available for him unfortunately.  
To determine what can be equipped and what not, we will compare level of our knight with the level of every item in his rucksack.

#### With the filter
```javascript
const player = { name: "Sir. Funnyface", level: 7 };
const rucksack = [
    { name: "Steel dagger", level: 11 },
    { name: "Iron mace", level: 7 },
    { name: "Leather boots", level: 4 },
    { name: "Mail socks", level: 2 },
    { name: "Studded armor", level: 9 }
];

const wearables = rucksack.filter( n => n.level <= player.level );
console.log( `${ player.name } can equip ${ wearables.join( ", " ).toLowerCase() }` );


/// wearables variable
// [
//     { name: "Iron mace", level: 7 },
//     { name: "Leather boots", level: 4 },
//     { name: "Mail socks", level: 2 }
// ]
/// console output
// Sir. Funnyface can equip iron mace, leather boots, mail socks
```

#### Without the filter
```javascript
const player = { name: "Sir. Funnyface", level: 7 };
const rucksack = [
    { name: "Steel dagger", level: 11 },
    { name: "Iron mace", level: 7 },
    { name: "Leather boots", level: 4 },
    { name: "Mail socks", level: 2 },
    { name: "Studded armor", level: 9 }
];

const wearables = [];
for ( const item of equipment ) {
    if ( item.level <= player.level ) {
        wearables.push( item );
    }
}

console.log( `${ player.name } can equip ${ wearables.join( ", " ).toLowerCase() }` );


/// wearables variable
// [
//     { name: "Iron mace", level: 7 },
//     { name: "Leather boots", level: 4 },
//     { name: "Mail socks", level: 2 }
// ]
/// console output
// Sir. Funnyface can equip iron mace, leather boots, mail socks
```


## Array.prototype.reduce
Reduce is the most complex one of the three and with it's power you can actually implement the filter and map functionality easily.  

It applies a rolling computation to sequential pairs of values in an array and returning the result.

### Examples
This time our mighty knight Sir. Funnyface is about to sell his hard-earned loot to inkeeper.  
We need to know the worth of knight's equipment, we can achieve that by calculating the sum of of every item's value. 

#### With the reduce
```javascript
const rucksack = [
    { name: "Steel dagger", value: 11 },
    { name: "Iron mace", value: 7 },
    { name: "Leather boots", value: 4 },
    { name: "Studded armor", value: 9 }
];

const offer = equipment.reduce( ( accumulator, value ) => accumulator + value, 0 );
console.log( `The innkeeper can give you ${ offer } gold coins for your equipment.` );


/// offer variable
// 31
```


#### Without the reduce
```javascript
const rucksack = [
    { name: "Steel dagger", value: 11 },
    { name: "Iron mace", value: 7 },
    { name: "Leather boots", value: 4 },
    { name: "Studded armor", value: 9 }
];

let worth = 0;
for ( const item of items ) {
    worth = worth + item.value
}

console.log( `The innkeeper can give you ${ worth } gold coins for your equipment.` );


/// worth variable
// 31
```


## Map, filter, reduce combined
### Example
Sir. Funnyface is not playing around you know? He went on a demon slaying adventure.  

Time for a more complex challenge.  
From a list of creatures that pesting near village we need to find a demon and slay him, yielding us some old-school experience points.  

To do that, we need to extract from a list a demon type creature and then execute him by calling `slay` function passing in reference to him.  

When the purge is over, we need to sum up our hard-earned exp points!

```javascript
const isDemon = creature => creature.type === "Demon"
const slay = creature => creature.experience * creature.level
const add = ( left, right ) => left + right

const creatures = [
    { name: "Rat", type: "Animal", experience: 5, level: 1 },
    { name: "Bandit", type: "Human", experience: 50, level: 5 },
    { name: "Wolf", type: "Animal", experience: 25, level: 3 },
    { name: "Utopiec", type: "Demon", experience: 250, level: 7 },
    { name: "Guard", type: "Human", experience: 75, level: 7 },
    { name: "Bear", type: "Animal", experience: 150, level: 5 },
    { name: "Likho", type: "Demon", experience: 375, level: 10 },
    { name: "Wolf", type: "Animal", experience: 25, level: 4 },
]


const experience =
    creatures
        .filter( isDemon )
        .map( slay )
        .reduce( add )

console.log( `You've just gained ${ experience } experience points!` )


/// experience variable
// 1250
```
