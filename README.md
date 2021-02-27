# ConfigServerDemo
Composite Spring Configuration Server Implementation


Steps (including local Vault SetUp)

1. Download and install [Vault](https://www.vaultproject.io/). On Mac, use [brew](https://brew.sh/) install vault (assuming brew is already installed).

2. Start Vault on your local machine
```
$ vault server -dev
```

3. Get the vault root token (copy the id filed)
```
$ vault token lookup
```
4. Update the aplication yml file in the following location with the token from step 3
```
src/main/resources/application.yml
```
+ If you are planing to use the [client code](https://github.com/nalinda6963/ConfigClientDemo), update the /src/main/resources/bootstrap.yml file with the same token (this file is in the [ConfigClientDemo](https://github.com/nalinda6963/ConfigClientDemo) project.

5. Open another teminal

6. Export the vault address 
```
$ export VAULT_ADDR='http://127.0.0.1:8200'
```

7. Export the vault token
```
$ export VAULT_TOKEN=<token_from_step_3>
```

8. Insert a secret in to vault (ConfigClientDemo = application name)
```
$ vault kv put secret/ConfigClientDemo foo=bar_vault
```

9. Start up the config server 
```
$ mvn spring-boot:run
```

10. Fetch configurations
```
$ curl -s http://127.0.0.1:8080/foo/default -H "X-Config-Token: <token_from_step_3>"
```

You should see the following output.
```json
{
	"name": "foo",
	"profiles": ["default"],
	"label": null,
	"version": null,
	"state": null,
	"propertySources": [{
		"name": "vault:foo",
		"source": {
			"foo": "bar_vault"
		}
	}]
}
```

