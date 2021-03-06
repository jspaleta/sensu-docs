---
title: "Sensuctl"
linkTitle: "Sensuctl"
description: "The sensuctl reference guide"
weight: 1
version: "2.0"
product: "Sensu Core"
platformContent: false 
menu:
  sensu-core-2.0:
    parent: reference
---

## How does sensuctl work?
Sensuctl is a command line tool for managing resources within Sensu. It works by
calling Sensu's underlying API to create, read, update, and delete resources,
events, and entities.

## Sensuctl specification
* Allows CRUD management of resources with options interactively or with flags
* Displays output in JSON or tabular format

## Command syntax
`sensuctl` uses the following syntax to run commands:
{{< highlight shell >}}
sensuctl [TYPE] [command] [NAME] [flags]
{{< /highlight >}}

* `TYPE`: specifies the resource type you would like to manage or view, such as
  `check`, `user`, `event`, etc.
* `command`: specifies what type of operation on a resource, such as `create`, `read`, `update`, `delete`
* `NAME`: specifies the name of the resource.
* `flags`: specifies modifier flags.

## Commands
To get the full list of commands available in `sensuctl`, run `sensuctl -h` at
the command prompt.

Sensuctl has a few commands to configure and get info about the `sensuctl`
command line tool:
{{< highlight line >}}
Commands:
  completion   Output shell completion code for the specified shell (bash or zsh)
  configure    Initialize sensuctl configuration
  import       import resources from STDIN
  logout       Logout from sensuctl
  version      Show the sensu-ctl version information
{{< /highlight >}}

As well as commands to manage sensu resources:
{{< highlight shell >}}
Management Commands:
  asset        Manage assets
  check        Manage checks
  config       Modify sensuctl configuration
  entity       Manage entities
  environment  Manage environments
  event        Manage events
  filter       Manage filters
  handler      Manage handlers
  hook         Manage hooks
  mutator      Manage mutators
  organization Manage organizations
  role         Manage roles
  silenced     Manage silenced subscriptions and checks
  user         Manage users
{{< /highlight >}}

## Subcommands
Management commands have secondary commands that can create or modify attributes on
resources. For a list of subcommands specific to a resource, run `sensuctl
[TYPE] -h`. Most commands should have at the following CRUD operations available:

{{< highlight shell >}}
create                     create new [TYPE] 
delete                     delete [TYPE] given name
info                       show detailed [TYPE] information
list                       list [TYPE]
update                     update [TYPE]
{{< /highlight >}}

In addition to the standard management commands, management commands may have single 
use commands that allow you to set or remove a particular attribute on a resource. For example, 
the `check` command has the following list of subcommands available:

{{< highlight line >}}
Commands:
  remove-handlers            removes handlers from a check
  remove-high-flap-threshold removes high flap threshold from a check
  remove-hook                removes a hook from a check
  remove-low-flap-threshold  removes low flap threshold from a check
  remove-proxy-entity-id     removes proxy entity id from a check
  remove-proxy-requests      removes proxy requests from a check
  remove-runtime-assets      removes runtime assets from a check
  remove-subdue              removes subdue from a check
  remove-timeout             removes timeout from a check
  remove-ttl                 removes ttl from a check
  set-command                set command of a check
  set-cron                   set cron of a check
  set-handlers               set handlers of a check
  set-high-flap-threshold    set high flap threshold of a check
  set-hooks                  set hooks of a check
  set-interval               set interval of a check
  set-low-flap-threshold     set low flap threshold of a check
  set-proxy-entity-id        set proxy entity id of a check
  set-proxy-requests         set proxy requests for a check from file or stdin
  set-publish                set publish of a check
  set-runtime-assets         set runtime assets of a check
  set-stdin                  set stdin of a check
  set-subdue                 set subdue of a check from file or stdin
  set-subscriptions          set subscriptions of a check
  set-timeout                set timeout of a check
  set-ttl                    set ttl of a check
{{< /highlight >}}

## Flags

### Global
Global flags modify settings specific to `sensuctl`, such as a Sensu host URL or
[RBAC][1] information.

{{< highlight shell >}}
--api-url string        host URL of Sensu installation
--cache-dir string      path to directory containing cache & temporary files 
--config-dir string     path to directory containing configuration files
--environment string    environment in which we perform actions (default "default")
--organization string   organization in which we perform actions (default "default")
{{< /highlight >}}

### Local
Local flags modify attributes on resources within Sensu. They differ per
operation and per resource. Many, but not all, flags have shorthand flag
equivalents. A list of flags can be found by running `sensuctl [TYPE] [command] -h`. 
For example, the `check` command has the following local flags:

{{< highlight line >}}
  -c, --command string               the command the check should run
      --cron string                  the cron schedule at which the check is run
      --handlers string              comma separated list of handlers to invoke when check fails
  -h, --help                         help for create
      --high-flap-threshold string   flap detection high threshold (percent state change) for the check
      --interactive                  Determines if CLI is in interactive mode
  -i, --interval string              interval, in seconds, at which the check is run
      --low-flap-threshold string    flap detection low threshold (percent state change) for the check
      --proxy-entity-id string       the check proxy entity, used to create a proxy entity for an external resource
  -p, --publish                      publish check requests (default true)
  -r, --runtime-assets string        comma separated list of assets this check depends on
      --stdin                        accept event data via STDIN
  -s, --subscriptions string         comma separated list of topics check requests will be sent to
  -t, --timeout string               timeout, in seconds, at which the check has to run
      --ttl string                   time to live in seconds for which a check result is valid
{{< /highlight >}}

## Output

sensuctl can be configured to return JSON instead of the default human-readable
format.

{{< highlight shell >}}
$ sensuctl check info marketing-site --format json
{{</ highlight >}}

{{< highlight json >}}
{
  "name": "marketing-site",
  "interval": 10,
  "subscriptions": [
    "web"
  ],
  "command": "check-http.rb -u https://dean-learner.book",
  "handlers": [
    "slack"
  ],
  "runtime_assets": [],
  "environment": "default",
  "organization": "default"
}
{{< /highlight >}}

If you do not want to explicitly use the format flag with each command, you can
set the global default.

{{< highlight shell >}}
$ sensuctl config set-format json
OK
{{< /highlight >}}

[1]: ../../reference/rbac
