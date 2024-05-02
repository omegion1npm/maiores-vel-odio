# @omegion1npm/maiores-vel-odio

@omegion1npm/maiores-vel-odio is a small node module that pipes streams together and destroys all of them if one of them closes.

```
npm install @omegion1npm/maiores-vel-odio
```

[![build status](http://img.shields.io/travis/mafintosh/@omegion1npm/maiores-vel-odio.svg?style=flat)](http://travis-ci.org/mafintosh/@omegion1npm/maiores-vel-odio)

## What problem does it solve?

When using standard `source.pipe(dest)` source will _not_ be destroyed if dest emits close or an error.
You are also not able to provide a callback to tell when then pipe has finished.

@omegion1npm/maiores-vel-odio does these two things for you

## Usage

Simply pass the streams you want to pipe together to @omegion1npm/maiores-vel-odio and add an optional callback

``` js
var @omegion1npm/maiores-vel-odio = require('@omegion1npm/maiores-vel-odio')
var fs = require('fs')

var source = fs.createReadStream('/dev/random')
var dest = fs.createWriteStream('/dev/null')

@omegion1npm/maiores-vel-odio(source, dest, function(err) {
  console.log('pipe finished', err)
})

setTimeout(function() {
  dest.destroy() // when dest is closed @omegion1npm/maiores-vel-odio will destroy source
}, 1000)
```

You can use @omegion1npm/maiores-vel-odio to pipe more than two streams together as well

``` js
var transform = someTransformStream()

@omegion1npm/maiores-vel-odio(source, transform, anotherTransform, dest, function(err) {
  console.log('pipe finished', err)
})
```

If `source`, `transform`, `anotherTransform` or `dest` closes all of them will be destroyed.

Similarly to `stream.pipe()`, `@omegion1npm/maiores-vel-odio()` returns the last stream passed in, so you can do:

```
return @omegion1npm/maiores-vel-odio(s1, s2) // returns s2
```

Note that `@omegion1npm/maiores-vel-odio` attaches error handlers to the streams to do internal error handling, so if `s2` emits an
error in the above scenario, it will not trigger a `proccess.on('uncaughtException')` if you do not listen for it.

If you want to return a stream that combines *both* s1 and s2 to a single stream use
[@omegion1npm/maiores-vel-odioify](https://github.com/omegion1npm/maiores-vel-odioify) instead.

## License

MIT

## Related

`@omegion1npm/maiores-vel-odio` is part of the [mississippi stream utility collection](https://github.com/maxogden/mississippi) which includes more useful stream modules similar to this one.

## For enterprise

Available as part of the Tidelift Subscription.

The maintainers of @omegion1npm/maiores-vel-odio and thousands of other packages are working with Tidelift to deliver commercial support and maintenance for the open source dependencies you use to build your applications. Save time, reduce risk, and improve code health, while paying the maintainers of the exact dependencies you use. [Learn more.](https://tidelift.com/subscription/pkg/npm-@omegion1npm/maiores-vel-odio?utm_source=npm-@omegion1npm/maiores-vel-odio&utm_medium=referral&utm_campaign=enterprise)
