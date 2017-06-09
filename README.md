# henri-potier-react-native

## Overview

Dans ce tutoriel, nous allons construire une version simple de l'application IOS/Android de la bibliothèque d'Henri Potier.

## Setup

> Cet exemple d'application nécessite l'installation de base expliquée à
> [React Native Getting Started](https://facebook.github.io/react-native/releases/next/docs/getting-started.html).

Résumé des actions à effectuer:

1. `npm install -g create-react-native-app`

2. `create-react-native-app HenriPotierBookshop`

3. `installer l'application Expo sur votre smartphone IOS/Android`

### Starting the app IOS/Android

    cd HenriPotierBookshop && npm start

Connecter votre smartphone au même réseau que votre ordinateur. Utiliser l'application Expo pour scanner le QR code depuis votre terminal et selectionner votre projet.

### Hello World

create-react-native-app à généré une structure d'application avec le nom de votre projet, dans notre cas HenriPotierBookshop.
Ouvrer le fichier App.js pour commencé a travailler sur votre application, les changements que vous effectuer vont automatiquement relancer et mettre à jour l'application sur votre téléphone.
Secouer votre smartphone pour afficher la console de dévellopement. 

## Actual App
 
Maintenant que nous avons initialiser notre projet React Native, commençons le dévelopement de notre application HenriPotierBookshop.

![Alt text](/img/Exercice.png?raw=true "Screenshot book with shadow")

### Mocking data

Avant d'écrire le code pour récupérer les données réelles sur les livres d'Henri Potier, nous moquerons certaines données afin que nous puissions nos mains sales avec React Native
Ajouter la constante suivante dans votre fichier App.js.

```javascript
const MOCKED_BOOKS_DATA = [
  {title: 'Henri Potier à l\'école des sorciers', price: '35', cover: 'http://henri-potier.xebia.fr/hp0.jpg'},
];
```

### Render a book

Nous allons afficher le titre, le prix et la vignette d'un livre. La vignette étant un composant Image dans React Native, ajoutez Image à la liste des importations React ci-dessous.

```javascript
import React from 'react';
import {
  Image,
  StyleSheet,
  Text,
  View,
} from 'react-native';
```

Modifiez maintenant la fonction 'render' afin que nous rendions les données mentionnées ci-dessus plutôt que le 'Hello World'.

```javascript
  render() {
    const book = MOCKED_BOOKS_DATA[0];
    return (
      <View style={styles.container}>
        <Image source={{uri: book.cover}}/>
        <Text>{book.title}</Text>
        <Text>{book.price}</Text>
      </View>
    );
  }
```

Appuyez sur `⌘ + R` /` Recharger JS` et vous devriez voir "Henri Potier à l'école des sorciers" et "35". Notez que l'image ne s'affiche pas. C'est parce que nous n'avons pas spécifié la largeur et la hauteur de l'image. Cela se fait via des styles. Modifions nos styles avec les valeurs suivantes.

```javascript
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  thumbnail: {
    width: 200,
    height: 150,
  },
});
```

Et enfin, nous devons appliquer ce style au composant Image:

```javascript
        <Image
          source={{uri: movie.posters.thumbnail}}
          style={styles.thumbnail}
        />
```

Appuyez sur `⌘ + R` /` Recharger JS` et l'image devrait s'afficher maintenant.

![Alt text](/img/BookWithoutStyle?raw=true "Screenshot data without style")

### Add some styling

C'est génial, nous avons rendu nos données. Maintenant, ajoutons-y un peu de style:

![Alt text](/img/Book-Specs.png?raw=true "Book Design")

Nous devrons ajouter un autre conteneur afin de définir la dimention et l'arrière plan de la représentation d'un livre.
Utiliser les propriétés de style `width` et `backgroundColor`.

```javascript
      return (
        <View style={styles.container}>
          <View style={styles.bookContainer}>
            <Image
              style={styles.thumbnail}
              source={{ uri: book.cover }}/>
            <Text>{book.title}</Text>
            <Text>{book.price}</Text>
          </View>
        </View>
      );
```

Astuce: Supprimer la propriété `width` de l'image pour qu'elle occupe tout l'espace disponible de son parent.

```javascript
    container: {
      flex: 1,
      justifyContent: 'center',
      alignItems: 'center',
      backgroundColor: '#e8e8e8',
    },
    bookContainer: {
      width: 150,
      backgroundColor: '#FFF'
    },
    thumbnail: {
      height: 200,
    },
```

Astuce: Pour maitriser le rendu de l'image on peut aussi utiliser la propriété `resizeMode` du componsant `Image`. 


Ajouter un container autour des textes pour appliquer un peu de style. 

```javascript
    return (
      <View style={styles.container}>
        <View style={styles.bookContainer}>
          <Image
            style={styles.thumbnail}
            source={{ uri: book.cover }}/>
          <View style={styles.descriptionContainer}>
            <Text>{book.title}</Text>
            <Text>{book.price}</Text>
          </View>
        </View>
      </View>
    );
```

```javascript
    descriptionContainer: {
      padding: 8
    },
```

Ajouter le style du prix.

```javascript
   return (
     <View style={styles.container}>
       <View style={styles.bookContainer}>
         <Image
           style={styles.thumbnail}
           source={{ uri: book.cover }}/>
         <View style={styles.descriptionContainer}>
           <Text>{book.title}</Text>
           <Text style={styles.price}>{book.price}&nbsp;€</Text>
         </View>
       </View>
     </View>
   );
```

Nous utilisons FlexBox pour la mise en page - voir [ce super guide] (https://css-tricks.com/snippets/css/a-guide-to-flexbox/) pour en savoir plus.

```javascript
    price: {
      paddingTop: 8,
      color: '#03a9f4',
      alignSelf: 'flex-end',
    },
```

Dernière étape, ajouter une ombre !

Nous allons utiliser les propriétes de style `Shadow` de IOS.

```javascript
    shadowOpacity: 0.1,
    shadowOffset: {
      height: 1,
    },
    shadowColor: 'black',
    shadowRadius: 1,
```
Pour Android nous utiliseront la propriété `elevation`.

```javascript
   elevation: 2 //Material Design Card Elevation -> 2 
```

Exemple de code spécifique, utilisation de Platform


```javascript
    bookContainer: {
        width: 150,
        backgroundColor: '#FFF',
        ...Platform.select({
          ios: {
            shadowOpacity: 0.1,
            shadowOffset: {
              height: 1,
            },
            shadowColor: 'black',
            shadowRadius: 1,
          },
          android: {
            elevation: 2
          }
        })
    },
```
[ce super guide](https://facebook.github.io/react-native/docs/platform-specific-code.html) pour en savoir plus.

Go ahead and press `⌘+R` / `Reload JS` and you'll see the updated view.

![Alt text](/img/BookWithStyle?raw=true "Screenshot book with shadow")

### Fetching real data

Ajoutez la constante suivante au haut du fichier (généralement en dessous des importations) pour créer la `REQUEST_URL`s utilisés pour obtenir les données de la bibliothèque henri potier.

```javascript

const REQUEST_URL = 'https://raw.githubusercontent.com/facebook/react-native/master/docs/MoviesExample.json';
```


Ajoutez un état initial à notre application afin que nous puissions vérifier `this.state.books === null` pour déterminer si les données de la bibliothèque ont été chargées ou non. Nous pourrons redéfinir ces données avec la réponse de l'api grâce a `this.setState ({books: booksData})`. Ajoutez ce code juste au-dessus de la fonction de rendu dans notre classe React.

```javascript
  constructor(props) {
    super(props);
    this.state = {
      books: null,
    };
  }
```

Nous souhaitons requeter l'api juste après la fin du chargement du composant. `ComponentDidMount` est une fonction de `React Component` que React appellera exactement une fois, juste après le chargement du composant.

```javascript
  componentDidMount() {
    this.fetchData();
  }
```

Ajoutez maintenant la fonction `fetchData` ci-dessus à notre composant principal. Cette méthode sera responsable de la gestion de l'extraction des données. Tout ce que vous devez faire est d'appeler `this.setState ({books: data})` après avoir résolu la chaîne de promesses car la façon dont React fonctionne est que `setState` déclenche réellement un re-render, puis la fonction de rendu notera que` this.state.books` n'est plus «nul».

```javascript
  fetchData() {
    fetch(REQUEST_URL)
      .then((response) => response.json())
      .then((responseData) => {
        this.setState({
          books: responseData.books,
        });
      })
  }
```

Modifiez maintenant la fonction de rendu pour rendre une vue de chargement si nous n'avons pas de données de films et pour rendre le premier film autrement.

```javascript
  render() {
    if (!this.state.books) {
      return this.renderLoadingView();
    }

    const book = this.state.books[0];
    return this.renderBook(book);
  }

  renderLoadingView() {
    return (
      <View style={styles.container}>
        <Text>
          Chargement des livres...
        </Text>
      </View>
    );
  }

  renderBook(book) {
    render() {
        return (
          <View style={styles.container}>
            <View style={styles.bookContainer}>
              <Image
                style={styles.thumbnail}
                source={{ uri: book.cover }}/>
              <View style={styles.descriptionContainer}>
                <Text>{book.title}</Text>
                <Text style={styles.price}>{book.price}&nbsp;€</Text>
              </View>
            </View>
          </View>
        );
        }
  }
```

Maintenant, appuyez sur `⌘ + R` /` Reload JS` et vous devriez voir "Chargement des livres ..." jusqu'à ce que la réponse revienne, puis il rendra le premier film qu'il a tiré de la bibliothèque d'henri potier

## FlatList

Modifions maintenant cette application pour rendre toutes ces données dans un composant `FlatList`, plutôt que de simplement rendre le premier film.

Pourquoi un «FlatList» est-il mieux que de rendre tous ces éléments ou de les mettre dans une «ScrollView»? Malgré le fait d'être rapide, le rendu d'une liste éventuellement infinie d'éléments pourrait être lent. `FlatList` planifie le rendu des vues afin que vous ne affichiez que celles sur l'écran et celles déjà rendues mais hors écran sont supprimées de la hiérarchie de vue native.

Tout d'abord: ajoutez l'importation `FlatList` en haut du fichier.

```javascript
import React from 'react';
import {
  Image,
  StyleSheet,
  Text,
  View,
  FlatList
} from 'react-native';
```

Modifiez maintenant la fonction de rendu afin qu'une fois que nous avons nos données, il affiche une liste de livre au lieu d'un unique liver.

```javascript
  render() {
    if (!this.state.loaded) {
      return this.renderLoadingView();
    }

    return (
      <ListView
        dataSource={this.state.dataSource}
        renderRow={this.renderMovie}
        style={styles.listView}
      />
    );
  }
```

The `dataSource` is an interface that `ListView` is using to determine which rows have changed over the course of updates.

You'll notice we used `dataSource` from `this.state`. The next step is to add an empty `dataSource` to the object returned by `constructor`. Also, now that we're storing the data in `dataSource`, we should no longer use `this.state.movies` to avoid storing data twice. We can use boolean property of the state (`this.state.loaded`) to tell whether data fetching has finished.

```javascript
  constructor(props) {
    super(props);
    this.state = {
      dataSource: new ListView.DataSource({
        rowHasChanged: (row1, row2) => row1 !== row2,
      }),
      loaded: false,
    };
  }
```

And here is the modified `fetchData` method that updates the state accordingly:

```javascript
  fetchData() {
    fetch(REQUEST_URL)
      .then((response) => response.json())
      .then((responseData) => {
        this.setState({
          dataSource: this.state.dataSource.cloneWithRows(responseData.movies),
          loaded: true,
        });
      })
      .done();
  }
```

Finally, we add styles for the `ListView` component to the `styles` JS object:
```javascript
  listView: {
    paddingTop: 20,
    backgroundColor: '#F5FCFF',
  },
```

And here's the final result:

![Alt text](/img/Final.png?raw=true "Screenshot book with shadow")

There's still some work to be done to make it a fully functional app such as: adding navigation, search, infinite scroll loading, etc. Check the [Movies Example](https://github.com/facebook/react-native/tree/master/Examples/Movies) to see it all working.


### Final source code

```javascript
import React from 'react';
import { StyleSheet, Text, View, Image, Platform, FlatList } from 'react-native';

const REQUEST_URL = 'http://henri-potier.xebia.fr/books';

export default class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      books: null,
      loaded: false,
    };
  }

  componentDidMount() {
    this.fetchData();
  }

  fetchData() {
    fetch(REQUEST_URL)
      .then((response) => response.json())
      .then((responseData) => {
        this.setState({
          books: responseData,
          loaded: true,
        });
      })
  }

  render() {
    if (!this.state.loaded) {
      return this.renderLoadingView();
    }

    return (
      <View style={styles.container}>
        <FlatList
          numColumns={2}
          showsVerticalScrollIndicator={false}
          data={this.state.books}
          keyExtractor={(book) => book.isbn}
          renderItem={this.renderBook}
        />
      </View>
    );
  }

  renderLoadingView() {
    return (
      <View style={styles.container}>
        <Text>
          Loading movies...
        </Text>
      </View>
    );
  }

  renderBook({item}) {
    return (
      <View style={styles.bookContainer}>
        <Image
          style={styles.thumbnail}
          source={{ uri: item.cover }}/>
        <View style={styles.descriptionContainer}>
          <Text>{item.title}</Text>
          <Text style={styles.price}>{item.price}&nbsp;€</Text>
        </View>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    paddingTop: 30, 
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#e8e8e8',
  },
  bookContainer: {
    width: 150,
    margin: 8,
    backgroundColor: '#FFF',
    ...Platform.select({
      ios: {
        shadowOpacity: 0.1,
        shadowOffset: {
          height: 1,
        },
        shadowColor: 'black',
        shadowRadius: 1,
      },
      android: {
        elevation: 2
      }
    })
  },
  descriptionContainer: {
    padding: 8
  },
  price: {
    paddingTop: 8,
    color: '#03a9f4',
    alignSelf: 'flex-end'
  },
  thumbnail: {
    height: 200,
  },
});
```