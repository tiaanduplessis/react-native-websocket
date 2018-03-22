# react-native-websocket

[![Greenkeeper badge](https://badges.greenkeeper.io/tiaanduplessis/react-native-websocket.svg)](https://greenkeeper.io/) [![Package version](https://img.shields.io/npm/v/react-native-websocket.svg?style=flat-square)](https://npmjs.org/package/react-native-websocket) [![Standard](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/feross/standard) [![Travis Build](https://img.shields.io/travis/tiaanduplessis/react-native-websocket.svg?style=flat-square)](https://travis-ci.org/tiaanduplessis/react-native-websocket) [![GitHub version](https://badge.fury.io/gh/tiaanduplessis%2Freact-native-websocket.svg?style=flat-square)](https://badge.fury.io/gh/tiaanduplessis%2Freact-native-websocket) [![Dependency CI](https://dependencyci.com/github/tiaanduplessis/react-native-websocket/badge?style=flat-square)](https://dependencyci.com/github/tiaanduplessis/react-native-websocket) [![License](https://img.shields.io/npm/l/react-native-websocket.svg?style=flat-square)](https://github.com/tiaanduplessis/react-native-websocket/blob/master/LICENSE) 



> WebSocket API wrapped as a component for React Native

## Table of Contents

- [About](#about)
- [Install](#install)
- [Usage](#usage)
- [Contribute](#contribute)
- [License](#license)

## About

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

## License

Licensed under the MIT License.
