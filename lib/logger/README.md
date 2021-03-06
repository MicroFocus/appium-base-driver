## logger

Basic logger defaulting to [npmlog](https://github.com/npm/npmlog) with special consideration for running
tests (doesn't output logs when run with `_TESTING=1` in the env).

### Logging levels

There are a number of levels, exposed as methods on the log object, at which logging can be made. The built-in ones correspond to those of [npmlog](https://github.com/npm/npmlog#loglevelprefix-message-), and are:
`silly`, `verbose`, `info`, `http`, `warn`, and `error`. In addition there is a `debug` level.

The default threshhold level is `verbose`.

The logged output, by default, will be `level prefix message`. So

```js
import { getLogger } from 'appium-base-driver';
let log = getLogger('mymodule');
log.warn('a warning');`
```

Will produce

```shell
warn mymodule a warning
```


### Environment variables

There are two environment variable flags that affect the way `appium-base-driver` `logger` works.

`_TESTING`

- `_TESTING=1` stops output of logs when set to `1`.

`_FORCE_LOGS`

- This flag, when set to `1`, reverses the `_TESTING`


### Usage

`log.level`

- get and set the threshhold level at which to display the logs. Any logs at or above this level will be displayed. The special level silent will prevent anything from being displayed ever. See [npmlog#level](https://github.com/npm/npmlog#loglevel).

`log[level](message)`

- logs to `level`
    ```js
    import { getLogger } from 'appium-base-driver';
    let log = getLogger('mymodule');

    log.info('hi!');
    // => info mymodule hi!
    ```

`log.unwrap()`

- retrieves the underlying [npmlog](https://github.com/npm/npmlog) object, in order to manage how logging is done at a low level (e.g., changing output streams, retrieving an array of messages, adding log levels, etc.).

    ```js
    import { getLogger } from 'appium-base-driver';
    let log = getLogger('mymodule');

    log.info('hi!');

    let npmlogger = log.unwrap();

    // any `npmlog` methods
    let logs = npmlogger.record;
    // logs === [ { id: 0, level: 'info', prefix: 'mymodule', message: 'hi!', messageRaw: [ 'hi!' ] }]
    ```

`log.errorAndThrow(error)`

- logs the error passed in, at `error` level, and then throws the error. If the error passed in is not an instance of [Error](https://nodejs.org/api/errors.html#errors_class_error) (either directly, or a subclass of `Error`) it will be wrapped in a generic `Error` object.

    ```js
    import { getLogger } from 'appium-base-driver';
    let log = getLogger('mymodule');

    // previously there would be two lines
    log.error('This is an error');
    throw new Error('This is an error');

    // now is compacted
    log.errorAndThrow('This is an error');
    ```
