# puregram-redis-storage

RedisStorage - Simple add-on for [Session](https://github.com/nitreojs/puregram/tree/lord/packages/session) [puregram](https://github.com/nitreojs/puregram) library

> Powered by [ioredis](https://github.com/luin/ioredis), based on [vk-io-redis-storage](https://github.com/xTCry/vk-io-redis-storage)

## Installation

### Yarn
```bash
yarn add puregram-redis-storage
```

### NPM
```bash
npm i puregram-redis-storage
```

### Example usage
```js
const { Telegram } = require('puregram')
const { SessionManager } = require('@puregram/session')
const { RedisStorage } = require('puregram-redis-storage')

const bot = new Telegram.fromToken(process.env.TOKEN)

function startBot ({ updates }) {
    const storage = new RedisStorage({
        // redis: ioRedisClient,
        redis: {
            host: '127.0.0.1'
        },
        keyPrefix: 'vk-io:session:'
        // ttl: 12 * 3600
    })

    const sessionManager = new SessionManager({
        storage,
        getStorageKey: (ctx) =>
            ctx.userId
                ? `${ctx.userId}:${ctx.userId}`
                : `${ctx.peerId}:${ctx.senderId}`
    })

    updates.on('message', sessionManager.middleware)

    updates.on('message_new', (ctx, next) => {
        if (context.text !== '/counter') {
            return next()
        }
        
        if (ctx.isOutbox) return

        const { session } = ctx
        session.counter = (session.counter || 0) + 1

        ctx.send(`You turned to the bot (${session.counter}) times`)
    })

    updates.start().catch(console.error)
}

// ...
startBot(bot)
```
