# React Config Professional

- Preparación de un entorno de trabajo profesional con React.js

**Table of Contents**

* [Configuración Básica de React](https://github.com/NetoCruz/React_Config_Professional/blob/master/README.md#configuración-básica-de-react)
* [Instalación de React](https://github.com/NetoCruz/React_Config_Professional/blob/master/README.md#instalación-de-react)
* [Creando componente de prueba](https://github.com/NetoCruz/React_Config_Professional/blob/master/README.md#creando-componente-de-prueba)
* [Instalando Babel](https://github.com/NetoCruz/React_Config_Professional/blob/master/README.md#instalando-babel)
* [Instalando Webpack](https://github.com/NetoCruz/React_Config_Professional/blob/master/README.md#instalando-webpack)
* [Instalando WebpackDevServer](https://github.com/NetoCruz/React_Config_Professional/blob/master/README.md#instalando-webpackdevserver)
* [Instalando Sass](https://github.com/NetoCruz/React_Config_Professional/blob/master/README.md#instalando-sass)
* [Creando la carpeta de nuestros estilos](https://github.com/NetoCruz/React_Config_Professional/blob/master/README.md#creando-la-carpeta-de-nuestros-estilos)
* [Configurando Eslint](https://github.com/NetoCruz/React_Config_Professional/blob/master/README.md#configurando-eslint)
* [Agregando gitignore](https://github.com/NetoCruz/React_Config_Professional/blob/master/README.md#agregando-gitignore)

## [Configuración Básica de React](link)

### Carpetas y archivos principales

* Creamos nuestra carpeta de proyecto
* Abrimos la terminal y nos ubicamos dentro de la carpeta

* Creamos el package.json
```javascript
npm init -y
```
* Creamos las carpetas y archivos principales que son 

mkdir **/src** -- *donde vive nuestro proyecto*
mkdir **/public** -- *estan son elementos que se enviaran a produccion*

Dentro de /src/ creamos:
mkdir **/components** -- *Donde estan nuestros react components*

En /src/ creamos el archivo:
touch /src **index.js** -- *entrada de nuestro proyecto*

En la carpeta /public/ creamos el archivo
touch /public **index.html**  --

## [Instalación de React](link)
En la terminal ingresamos:
```javascript
npm install react react-dom
```

## [Creando componente de prueba](link)

En nuestro archivo src/**index.js** escribimos:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import HelloWorld from './components/HelloWorld';

ReactDOM.render(<HelloWorld />, document.getElementById('app'));
```
Ahora, en el public/**index.html** colocamos:

```html
<!DOCTYPE html>
<html>
    <head>
        <mate charest="utf-8" />
        <title>Hello world!</title>
    </head>
    <body>
       <div id="app"></div>
    </body>
</html>
```
## [Instalando Babel](link)
En la terminal ingresamos:
```javascript
npm install @babel/core babel-loader @babel/preset-env @babel/preset-react --save-dev
```
Recordemos que  **- -save-dev** es para instalar dependecias de desarrollo

En la raiz de nuestro proyecto creamos el archivo
/**.babelrc**

Dentro de éste creamos un objeto con lo siguiente:

```javascript
{
    "presets": [
        "@babel/preset-env",
        "@babel/preset-react"
    ]
}
```

## [Instalando Webpack](link)
En nuestra terminal ingresamos
```javascript
npm install webpack webpack-cli html-webpack-plugin html-loader  --save-dev

```

En la raiz de nuestro proyecto creamos el siguiente archivo:
/**.webpack.config.js**
Y dentro de el colocamos lo siguiente
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
  resolve: {
    extensions: ['.js', '.jsx'],
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
        },
      },
      {
        test: /\.html$/,
        use: {
          loader: 'html-loader',
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
      filename: './index.html',
    }),
  ],
};

```
Nos dirigimos al /**package.json** para crear nuestro script que compilará nuestro
proyecto.

```javascript 
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build":"webpack --mode production"
  },
```

En la terminal ingresamos :
```javascript 
npm run build
```
Podremos ver que en la raiz de nuestro proyecto de ha creado la carpeta /**dist** donde trendremos nuestro archivos compilados

## [Instalando WebpackDevServer](link)
En la terminal:
```javascript 
npm install  webpack-dev-server --save-dev
```
En nuestro **package.json** creamos otro script llamado **start**:
```javascript 
{
  ""scripts"": {
    ""build"": ""webpack --mode production"",
    ""start"": ""webpack-dev-server --open --mode development""
  },
}

```

Ingresamos en la terminal 
```javascript 
npm run start 
```
Con esto podemos abrir un entorno de desarrollo local de nuestro proyecto, además de contar con la ventaja de que los cambios que vayamos realizando se cargarán al instante.

## [Instalación de SASS](link)

Ingresamos en la terminal 
```javascript 
npm install mini-css-extract-plugin css-loader node-sass sass-loader --save-dev

```

En el archivo **webpack.config.js** ingresaremos 
```javascript 
// ...

module: {
  rules: [
    {
      test: /\.(s*)css$/,
      use: [
        { loader: MiniCssExtractPlugin.loader },
        'css-loader',
        'sass-loader',
      ],
    }, 
  ],
},

// ...

plugins: [
  new MiniCssExtractPlugin({
    filename: 'assets/[name].css',
  }),
],`

```
El archivo  completo quedaría de esta manera:

```javascript 
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
  resolve: {
    extensions: ['.js', '.jsx'],
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
        },
      },
      {
        test: /\.html$/,
        use: [{
          loader: 'html-loader',
        }
    ]
      },
      {
        test: /\.(s*)css$/,
        use: [
          { loader: MiniCssExtractPlugin.loader },
          'css-loader',
          'sass-loader',
        ],
      }, 
      
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
      filename: './index.html',
    }),
    new MiniCssExtractPlugin({
        filename: 'assets/[name].css',
      }),
  ],
};


```
### [Creando las carpetas de nuestros estilos](link)
En la carpeta /**src** creamos:
/**assets** y dentro de ésta:
/**styles**
dentro de /styles creamos un archivo:
**App.scss**

Quedando de la siguiente manera
/src/assets/styles/App.scss

## [Instalando ESLINT](link)

Terminal
```javascript 
npm install  eslint babel-eslint eslint-config-airbnb eslint-plugin-import eslint-plugin-react eslint-plugin-jsx-a11y --save-dev
```
Utilizaremos las relgas de Airbnb para la construccion de proyectos

En la raiz de nuestro proyecto creamos el archivo **.eslintrc**
Dentro de este archivo copiamos el siguiente código:

[Links](https://gist.github.com/NetoCruz/a6a64044809b63588cffae65736a8e76
)

Con esto podemos lintar nuestro código y trabajar mejor.

## [Creando nuestro gitignore](link)

En la raiz de nuestro proyecto creamos el archivo **.gitignore** su función es evitar subir archivos que puedan comprometer las seguiridad de nuestro proyecto.

Dentro de este archivo copiamos el siguiente código que se encuentra en un gist:

[Links](https://gist.github.com/NetoCruz/5ef207cb2e2ed03e1763bda2bd04d149
)


**Ahora ya tienes un entorno de trabajo profesional para trabajar con React.js**


