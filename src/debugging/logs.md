# 🧰 Logs

Logging is the simplest and most accessible debug feature in xray engine. <br/>
By default, logs are enabled for all engine variants expect `gold` and display in (1) `game console`,
(2) `dedicated log file`, (3) `visual studio console`.

## 🧰 Checking XRF logs

To enable XRF logging make sure the `GameConfig` logging flag is set to true. <br/>
It is enabled by default.

Depending on how you run the game, you can use the following approaches to check the log:

### With pre-built engine

- Make sure you are using the custom engine. If not, switch to the mixed/release variant: `npm run engine use release`
- Link the application logs folder with the target directory: `npm run link` (if it is not linked already)
- Start the game (`npm run start_game`)
- Check files in `target/logs_link` directory -> `opexray_%username%.log` is default openxray log file

### With visual studio

- Just run the project and check `Output` window of application

### Checking custom log files

If lua loggers are explicitly declaring output as file, not game log, then you should check `target/logs_link`
for other log files. Usually names look like `xrf_%module%.log`.

## 🧰 Writing logs

### In game console / log

todo; <br/>
todo; <br/>
todo; <br/>

### In custom .log file

todo; <br/>
todo; <br/>
todo; <br/>

## 🧰 Flushing logs

todo; <br/>
todo; <br/>
todo; <br/>

## 🧰 Printing logs with CLI

todo; <br/>
todo; <br/>
todo; <br/>
