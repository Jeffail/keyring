Keyring
=======

## THIS IS A STALE FORK

Do not use this fork unless you have a specific reason as it will remain stale and will disappear once https://github.com/99designs/keyring/issues/103 is resolved (if ever).

### Why

The purpose of this fork is to fix issues where indirect dependencies that your project has (which you have limited control over) include `99designs/keyring`, which has an outstanding bug (https://github.com/99designs/keyring/issues/103) that causes zombie dbus-daemons processes on each execution. This is an absolute deal breaker for many types of tool or service.

### How

Add the following line to your `go.mod` file:

```
replace github.com/99designs/keyring => github.com/Jeffail/keyring v1.2.3
```

Unfortunately, any modules that import your module will also need to add a replace directive of their own.

---

Keyring provides a common interface to a range of secure credential storage services. Originally developed as part of [AWS Vault](https://github.com/99designs/aws-vault), a command line tool for securely managing AWS access from developer workstations.

Currently Keyring supports the following backends
 * [macOS Keychain](https://support.apple.com/en-au/guide/keychain-access/welcome/mac)
 * [Windows Credential Manager](https://support.microsoft.com/en-au/help/4026814/windows-accessing-credential-manager)
 * Secret Service ([Gnome Keyring](https://wiki.gnome.org/Projects/GnomeKeyring), [KWallet](https://kde.org/applications/system/org.kde.kwalletmanager5))
 * [KWallet](https://kde.org/applications/system/org.kde.kwalletmanager5)
 * [Pass](https://www.passwordstore.org/)
 * [Encrypted file (JWT)](https://datatracker.ietf.org/doc/html/rfc7519)
 * [KeyCtl](https://linux.die.net/man/1/keyctl)


## Usage

The short version of how to use keyring is shown below.

```go
ring, _ := keyring.Open(keyring.Config{
  ServiceName: "example",
})

_ = ring.Set(keyring.Item{
	Key: "foo",
	Data: []byte("secret-bar"),
})

i, _ := ring.Get("foo")

fmt.Printf("%s", i.Data)
```

For more detail on the API please check [the keyring godocs](https://godoc.org/github.com/99designs/keyring)


## Testing

[Vagrant](https://www.vagrantup.com/) is used to create linux and windows test environments.

```bash
# Start vagrant
vagrant up

# Run go tests on all platforms
./bin/go-test
```


## Contributing

Contributions to the keyring package are most welcome from engineers of all backgrounds and skill levels. In particular the addition of extra backends across popular operating systems would be appreciated.

This project will adhere to the [Go Community Code of Conduct](https://golang.org/conduct) in the github provided discussion spaces, with the moderators being the 99designs engineering team.

To make a contribution:

  * Fork the repository
  * Make your changes on the fork
  * Submit a pull request back to this repo with a clear description of the problem you're solving
  * Ensure your PR passes all current (and new) tests
  * Ideally verify that [aws-vault](https://github.com/99designs/aws-vault) works with your changes (optional)

...and we'll do our best to get your work merged in
