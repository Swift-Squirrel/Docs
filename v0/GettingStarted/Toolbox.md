# Squirrel Toolbox

**Squirrel Toolbox** is set of helpful commands which can improve your workflow

## Commands

You can list all commands with `squirrel help` and `squirrel <command> -h`

### Run server

`squirrel serve [options]`

To run server run 

```sh
squirrel serve
```

This will build your project and run executable. 

#### Build configuration

`squirrel serve -c release`

You can set build configuration with `-c` to `debug`(default) or `release`. For more options run `squirrel serve -h`.


#### Run in background

`squirrel serve -d`

If you don't want to block your current terminal user `-d` argument.


### Show running servers

`squirrel ps`

Shows PIDs of running servers (shows only servers started with `squirrel serve`)

### Stop server

`squirrel stop [<stopingPid>] [options]`

Stops running server. If `stoppingPid` and there is only one running server, this will stops it.

### Watch for changes

`squirrel watch` 

Watch for changes in *Source* directory. Rebuild and rerun on any change.

### Create file template

`squirrel create <fileName> [options]`

Creates file specified in options. 

possible options:

- model: Model used in databases (SquirrelConnector)
- layout: Layout template (NutView)
- view: View template (NutView)
- subview: Subview template (NutView)
