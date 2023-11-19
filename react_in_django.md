
### React directly in django or any standalone HTML file (NO NEED TO HOST)

```javascript
npm init -y
npm install react@18 react-dom@18 @babel/core @babel/preset-react babel-loader webpack webpack-cli
```

> => templates and static path should be set
>
> => Create index.html in templates directory (We only need this)
>
> => Create index.js in static directory (Entry point for react app)
>
> => Create webpack.config.js file in the root directory
>
> Contents of webpack.config.js
>
> ```javascript
> const path = require('path');
>
> 		module.exports = {
> 		  entry: path.resolve(__dirname, 'static/index.js'),
> 		  output: {
> 		    filename: 'bundle.js',
> 		    path: path.resolve(__dirname, 'static/dist'),
> 		  },
> 		  module: {
> 		    rules: [
> 		      {
> 		        test: /\.(js|jsx)$/,
> 		        exclude: /node_modules/,
> 		        use: {
> 		          loader: 'babel-loader',
> 		          options: {
> 		            presets: ['@babel/preset-react'],
> 		          },
> 		        },
> 		      },
> 		    ],
> 		  },
> 		  devServer: {
> 		    static: {
> 		        directory: path.resolve(__dirname, 'static'),
> 		      },
> 		      hot: true,
> 		      port: 3000, // Choose the port number that suits your needs
> 		    },
> 		};
> ```

> => Run `npx webpack` (it will create bundle.js and dist directory inside the static folder)
>
> => Content of index.html should be like (Include bundle.js from staticfiles)
>
> ```html
> <!DOCTYPE html>
> 	{% load static %}
> 	<html lang="en">
> 	  <head>
> 	    <meta charset="UTF-8">
> 	    <meta name="viewport" content="width=device-width, initial-scale=1.0">
> 	    <title>React App</title>  
> 	  </head>
> 	  <body>  
> 	    <div id="root"></div>
> 	    <script src="{% static 'dist/bundle.js' %}"></script>
> 	  </body>
> 	</html>
> ```

> => Content of index.js should be like (Like normal react app)
>
> ```javascript
> import React from 'react';
> 	import { createRoot } from 'react-dom/client';
> 	import App from './components/App';
>
>
> 	const root = createRoot(document.getElementById('root'));
>
> 	  root.render(  
> 	        <App />
> 	  );
> ```

> => Run npx webpack --watch (For constantly watch for changes)
>
> => Run final npx webpack before dploying (That's it no need to host react app now)
>
