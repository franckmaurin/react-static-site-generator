<div align="center">
<h1>⚡️<br/>React Static Site Generator</h1>
The quickest way to generate the static pages of your awesome react app before deploying its in the cloud.
</div>

## Install
```
npm install --save react-static-site-generator
```

## Usage
#### index.js
Take care to guard every usage of ```window```/```document``` variables in your code and export ```renderByServer``` to be used by node.
```js
import React from 'react'
import ReactDOM from 'react-dom'
import { Router, browserHistory } from 'react-router'
import ReactStaticSiteGenerator from 'react-static-site-generator'
import routes from './routes'
import Template from './components/Template'

// client render
if (typeof document !== "undefined") {
  ReactDOM.render(
    <Router
      history={browserHistory}
      routes={routes}
    />,
    document.getElementById('root')
  )
}

// server render
export const renderByServer = path =>
  <ReactStaticSiteGenerator
    path={path}
    routes={routes}
    template={Template}
  />
```

#### components/Template/index.js
The component print the Helmet variables ```params.head``` and the rendered ```<Router />``` component via ```params.body```.
```js
import React from 'react'

const Template = (params) => (
  <html>
    <head>
      {params.head.title.toComponent()}
      {params.head.meta.toComponent()}
      <meta charSet="utf-8" />
      <meta name="viewport" content="initial-scale=1.0, width=device-width, user-scalable=no" />
      <link rel="stylesheet" href="/assets/css/build.css" media="all" />
    </head>
    <body>
      <div
        id="root"
        dangerouslySetInnerHTML={{ __html: params.body }}
      />
      <script src="/assets/js/build.js" />
    </body>
  </html>
)

export default Template
```

#### webpack.config.js
To make the ```renderByServer``` function accessible, you need to configure ```output.libraryTarget``` property with ```umd``` and add the plugin like below:

```js
{
  plugins: [
    new ReactStaticSiteGenerator()
  ]
}
```
