# JIRA CLI

This utility allows you interact with a [JIRA REST API](https://developer.atlassian.com/jiradev/jira-apis/jira-rest-apis#JIRARESTAPIs-Overview) from the command line.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'jira'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install jira

## Configuration

Config is stored in `~/jira-config.json`. Use these commands to interact with your configuration:

- [ ] `jira config show` - show your current config: REST API base url, auth (password masked)
- [ ] `jira config default` - restore default jira config

### Config properties

Property | Default | Explained
-------- | ------- | ---------
`api_base_url` | `http://localhost:2990/jira/rest/api/2/` | API base url, should just be `<your_jira_instance>/rest/api/2/`
`username` | `admin` | API basic auth username
`password` | `admin` | API basic auth password
`default_fields` | `id,key,summary,assignee,reporter,status,updated` | Default issue fields to show in output
`default_order_by` | `ORDER+BY+updated+DESC` | If jql does not include an `order by` clause, this will be appended to the jql - this default shows most recently updated issues first
`default_issue_list_jql` | `reporter=currentUser()+OR+assignee=currentUser()` | Default JQL query for `issue list` command.

## Usage

Use these commands to interact with your JIRA REST API configured above. The command structure is based off of [Chef `knife`](https://docs.chef.io/knife.html).

Of course help is always available with `jira help SUBCOMMAND` or `jira SUBCOMMAND -h`

### `jira issue`

- [ ] `jira issue list`
- [ ] `jira issue show KEY`
- [ ] `jira issue FIELD set ISSUE-KEY "FIELD VALUE"`
  - ex: `jira issue summary set PROJ-123 "New summary"`
- [ ] `jira issue edit ISSUE-KEY` - edit all fields of an issue as JSON in `$EDITOR`
- [ ] `jira issue search "JQL query"`

### `jira project`

- [ ] `jira project list`
- [ ] `jira project show PROJKEY`

### Options shared by multiple commands

Short,Long | Explained
---------- | ---------
`-h` | Show help
`-c,--config <config_file.json>` | Config file path, defaults to `~/jira-config.json`
`-u,--auth <username:password>` | Override auth config set in config file

```
bundle exec rake install
bundle exec jira
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

Develop against [Atlassian's provided JIRA Vagrant box](https://developer.atlassian.com/static/connect/docs/latest/developing/developing-locally.html) (requires Vagrant and VirtualBox) using the provided `Vagrantfile`:

```shell
# start the vm
vagrant up
# you will be prompted for your password to edit /etc/exports to setup an NFS share to the vm
# connect to the vm
vagrant ssh
# type "yes" to accept oracle java agreement
# start jira tomcat app, db, apache proxy, etc
atlas-run-connect --product jira
# wait for a *long* time to download atlassian-jira-webapp
```

Connect to the JIRA webapp at [localhost:2990/jira](http://localhost:2990/jira) in a browser.

I would not recommend destroying the vm (`vagrant destroy`), downloading the JIRA webapp from Atlassian Maven takes a long time and would have to be done again after vagrant destroy. Instead, simply shutdown the vm (`vagrant halt`).

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

### Some basic curls to jira vagrant box

- `curl -s -H "Accept: application/json" -u "admin:admin" "http://localhost:2990/jira/rest/api/2/search?jql=reporter=admin&fields=id,key,summary" | python -mjson.tool`
- `curl -s -u "admin:admin" "http://localhost:2990/jira/rest/api/2/project" | python -mjson.tool`

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/atheiman/jira-cli.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

