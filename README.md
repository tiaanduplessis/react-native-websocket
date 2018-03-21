<h1 align="center">react-native-websocket</h1>
<div align="center">
  <strong>WebSocket API wrapped as a component for React Native</strong>
</div>
<br>
<div align="center">
  <a href="https://npmjs.org/package/react-native-websocket">
    <img src="https://img.shields.io/npm/v/react-native-websocket.svg?style=flat-square" alt="Package version" />
  </a>
  <a href="https://npmjs.org/package/react-native-websocket">
  <img src="https://img.shields.io/npm/dm/react-native-websocket.svg?style=flat-square" alt="Downloads" />
  </a>
  <a href="https://github.com/feross/standard">
    <img src="https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat-square" alt="Standard" />
  </a>
  <a href="https://travis-ci.org/tiaanduplessis/react-native-websocket">
    <img src="https://img.shields.io/travis/tiaanduplessis/react-native-websocket.svg?style=flat-square" alt="Travis Build" />
  </a>
  <a href="https://github.com/RichardLitt/standard-readme)">
    <img src="https://img.shields.io/badge/standard--readme-OK-green.svg?style=flat-square" alt="Standard Readme" />
  </a>
  <a href="https://badge.fury.io/gh/tiaanduplessis%2Freact-native-websocket">
    <img src="https://badge.fury.io/gh/tiaanduplessis%2Freact-native-websocket.svg?style=flat-square" alt="GitHub version" />
  </a>
  <a href="https://dependencyci.com/github/tiaanduplessis/react-native-websocket">
    <img src="https://dependencyci.com/github/tiaanduplessis/react-native-websocket/badge?style=flat-square" alt="Dependency CI" />
  </a>
  <a href="https://github.com/tiaanduplessis/react-native-websocket/blob/master/LICENSE">
    <img src="https://img.shields.io/npm/l/react-native-websocket.svg?style=flat-square" alt="License" />
  </a>
  <a href="http://makeapullrequest.com">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square" alt="PRs" />
  </a>
  <a href="https://www.paypal.me/tiaanduplessis/1">
    <img src="https://img.shields.io/badge/$-support-green.svg?style=flat-square" alt="Donate" />
  </a>
</div>
<br>
<div align="center">
  <a href="https://github.com/tiaanduplessis/react-native-websocket/watchers">
    <img src="https://img.shields.io/github/watchers/tiaanduplessis/react-native-websocket.svg?style=social" alt="Github Watch Badge" />
  </a>
  <a href="https://github.com/tiaanduplessis/react-native-websocket/stargazers">
    <img src="https://img.shields.io/github/stars/tiaanduplessis/react-native-websocket.svg?style=social" alt="Github Star Badge" />
  </a>
  <a href="https://twitter.com/intent/tweet?text=Check%20out%20react-native-websocket!%20https://github.com/tiaanduplessis/react-native-websocket%20%F0%9F%91%8D">
    <img src="https://img.shields.io/twitter/url/https/github.com/tiaanduplessis/react-native-websocket.svg?style=social" alt="Tweet" />
  </a>
</div>
<br>
<div align="center">
  Built with ❤︎ by <a href="tiaan.beer">Tiaan</a> and <a href="https://github.com/tiaanduplessis/react-native-websocket/graphs/contributors">contributors</a>
</div>

<h2>Table of Contents</h2>
<details>
  <summary>Table of Contents</summary>
  <li><a href="#about">About</a></li>
  <li><a href="#install">Install</a></li>
  <li><a href="#usage">Usage</a></li>
  <li><a href="#contribute">Contribute</a></li>
  <li><a href="#license">License</a></li>
</details>

## About

[![Greenkeeper badge](https://badges.greenkeeper.io/tiaanduplessis/react-native-websocket.svg)](https://greenkeeper.io/)

This module provides a simple component wrapper around the the WebSocket API. It provides a declarative way of handling a WebSocket connection.

## Websockets in React Native

If you're interested in using websockets in React Native generally here is a slightly abbreviated version of the source of this component:

```jsx
class WS extends Component {
	//...
	send = (data) => this.state.ws.send(data)
	componentDidMount () {
		this.reconnect = !!this.props.reconnect
		this._handleWebSocketSetup()
	}
	componentWillUnmount () {
		this.reconnect = false
		this.state.ws.close()
	}
	_handleWebSocketSetup = () => {
		const ws = new WebSocket(this.props.url)
		ws.onopen = () => {
			this.props.onOpen && this.props.onOpen()
		}
		ws.onmessage = (event) => { this.props.onMessage && this.props.onMessage(event) }
		ws.onerror = (error) => { this.props.onError && this.props.onError(error) }
		ws.onclose = () => this.reconnect ? this._handleWebSocketSetup() : (this.props.onClose && this.props.onClose())
		this.setState({ws})
	}
}
```

As you can see the component simply wraps the native websocket api. It's also recommended that you implement your own [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff) reconnect logic if you plan on using this component in production.

## Install

```sh
$ npm install --save react-native-websocket
# OR
$ yarn add react-native-websocket
```

## Usage

```jsx

import React, { Component } from 'react'
import { AppRegistry, View } from 'react-native'
import WS from 'react-native-websocket'

export default class Example extends Component {

  render () {
    return (
      <View style={{flex: 1}}>
        <WS
          ref={ref => {this.ws = ref}}
          url="wss://echo.websocket.org/"
          onOpen={() => {
            console.log('Open!')
            this.ws.send('Hello')
          }}
          onMessage={console.log}
          onError={console.log}
          onClose={console.log}
          reconnect // Will try to reconnect onClose
        />
      </View>
    )
  }
}

```

## Contribute

Contributions are welcome. Please open up an issue or create PR if you would like to help out.

Note: If editing the README, please conform to the [standard-readme](https://github.com/RichardLitt/standard-readme) specification.

## License

Licensed under the MIT License.
