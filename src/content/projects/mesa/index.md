---
title: "Mesa"
description: "A sensible component framework for anything"
date: "Nov 19 2019"
repoURL: "https://github.com/gingerich/mesa"
---

A sensible component framework for anything.

This was an experiment to explore a few concepts I was interested in - not for production use.

## A Quick Example

```js
class Greetings extends Mesa.Component {
  compose() {
    return ({ msg }) => `${this.config.greeting} ${msg.name}!`
  }
}

const greetingService = Mesa.createService()
greetingService.action(
  { act: 'greet' },
  Greetings.spec({ greeting: 'Hello' })
)

greetingService
  .call('greet', { name: 'World' })
  .then(res => console.log(res))

// 'Hello World!'
```

## Transport Layer

Plug-and-play transport layer keeps your service transport independent.

```js
const layer = Transport.createLayer()
  .protocol('tcp', TCP.transport())
  .use(Serializers.JSON())

const transporter = layer.transporter(connect => {
  connect.ingress.at('tcp://localhost:3000')
})

Mesa.createService()
  .plugin(transporter.plugin())
  .action('greet', Greetings.spec({ greeting: 'Hello' }))

transporter.connect().then(() => console.log('connected!'))
```

## 🏛️ License

MIT