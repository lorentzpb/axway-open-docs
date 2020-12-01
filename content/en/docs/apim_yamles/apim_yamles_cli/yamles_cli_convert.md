{
"title": "Convert API Gateway configuration using CLI",
"linkTitle": "Convert API Gateway configuration using CLI",
"weight":"40",
"date": "2020-11-11",
"description": "Learn how to use the YAML configuration CLI to convert API Gateway configuration to YAML format."
}

These are the conversion related options available in the YAML CLI:

* `fed2yaml`: Convert an XML federated configuration to a YAML configuration.
* `frag2yaml`: Convert an XML configuration fragment to a YAML configuration fragment.

A YAML configuration generated using `fed2yaml` or `frag2yaml` must be [validated](/docs/apim_yamles/apim_yamles_cli/yamles_cli_validate/#validate-configuration-changes-in-the-yaml-configuration) before you convert your API Gateway configuration to YAML format.

## Convert your XML configuration to a YAML configuration

To start using the YAML format, you must convert an existing valid upgraded XML federated configuration, which can be stored in a `.fed` file, a `.pol`, or a `.pol` and `.env`, or in the usual format of a set of XML files.

Before converting your XML federated configuration, you must upgrade it using [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics/#upgradeconfig-options) or [projupgrade](docs/apim_reference/devopstools_ref/#projupgrade-command-options).

The `--output-dir` must always be specified. This is the location where the YAML configuration will be written. Optionally, if the `--targz` option is used, the conversion will also create a `.tar.gz` file that is ready for deployment. The `.tar.gz` is created in addition to the content in the `--output-dir`.

The following are examples of how you can use `fed2yaml` in the `yamles` CLI to convert an XML federated configuration:

**Example 1:**

* Specify the directory where the configuration is stored.
* Add the YAML configuration to the `/home/user/yaml` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml`.

```
./yamles fed2yaml --source /home/user/apiprojects/myfed --output-dir /home/user/yaml --targz myconfig.tar.gz
```

**Example 2:**

* Specify the URL for the XML.
* Add the YAML configuration to the `/home/user/yaml` directory.
* Make sure to include the `configs.xml` when using a URL for the `--source` parameter.

```
./yamles fed2yaml --source federated:file:/home/user/apiprojects/myfed/configs.xml --output-dir /home/user/yaml
```

**Example 3:**

* Specify the URL for the XML
* Add the YAML configuration to the `/home/user/yaml` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml`.

```
./yamles fed2yaml --source federated:file:/home/user/apiprojects/myfed/configs.xml --output-dir /home/user/yaml --targz /home/user/archives/myconfig.tar.gz
```

**Example 4:**

* Specify a `.fed` file for the XML
* Add the YAML configuration into the `/home/user/yaml` directory.

```
./yamles fed2yaml --source /home/user/archives/config.fed --output-dir /home/user/yaml
```

If the directory referenced in the `--source` parameter contains sub-directories with XML federated configurations they can be all converted in one run of the `fed2yaml` command. For example, if you point to your Policy Studio `apiprojects` directory, all projects will be converted. A sub-directory of the same name is created in the `--output-dir` to contain the converted YAML configuration. If multiple XML federated configurations are found in the `--source` directory, the `--targz` should be set to a directory name, not a `.tar.gz` filename, to create a `.tar.gz` for each converted configuration.

Convert XML federated configurations in the `apiprojects` directory and generate `.tar.gz` files for each in another directory:

```
./yamles fed2yaml --source /home/user/apiprojects --output-dir /home/user/yaml --targz /home/user/archives
```

Convert XML federated configurations in the `apiprojects` directory and generate `.tar.gz` files for each in the same directory:

```
./yamles fed2yaml --source /home/user/apiprojects --output-dir /home/user/yaml --targz /home/user/yaml
```

Create an XML federated configuration by merging a `.pol` and `.env` files. Then, convert the result file to YAML. A `.pol` file must be specified on its own, but a `.env` file must be combined with a `.pol`:

```
./yamles fed2yaml --source /home/user/archives/mypol.pol --source /home/user/archives/myenv.env --output-dir /home/user/yaml
```

You can run the following help command for more details on each parameter:

```
yamles fed2yaml --help
```

{{< alert title="Note">}}There is no facility to convert a YAML configuration back to XML, and this will not be supported in the future.{{< /alert>}}

To learn how to solve conversion errors, see [known conversion errors](/docs/apim_yamles/apim_yamles_references/yamles_known_conversion_errors).

## Convert your XML configuration fragment to a YAML configuration fragment

You can convert an XML configuration fragment into a YAML configuration fragment using the `yamles frag2yaml` option.

You can view and validate the YAML configuration fragment using the [ES Explorer](/docs/apigtw_devguide/entity_store/#use-the-es-explorer) in the same way as a complete YAML configuration generated by converting a complete XML federated configuration. The fragment will only contain the entities and entity types contained in the original XML configuration fragment, and not all the configuration required to run the API Gateway. You might need to import or merge the YAML configuration fragment with another YAML configuration before it becomes suitable for deployment to an API Gateway.

Some XML configuration fragments do not contain all entities that are referred to from those included in the fragment. In this case, the XML configuration fragment can still be converted to YAML, but the resulting YAML configuration can only be validated with the `--allow-invalid-ref` parameter, see [how to allow unresolved references](/docs/apim_yamles/apim_yamles_cli/yamles_cli_validate/#disable-entity-reference-check).

The `frag2yaml` option automatically upgrades the configuration fragment, when required, before converting it to YAML. XML configuration fragments created with the current version of the product do not need to be upgraded.

The `--output-dir` must always be specified. This is the location where the YAML configuration will be written.

If the optional `--targz` option is used, the conversion will also create a `.tar.gz` file. The `.tar.gz` is created in addition to the content in the `--output-dir`. It is likely that this `tar.gz` will not contain all the configuration required to run the API Gateway as it will only contain the configuration that was contained in the original XML configuration fragment.

You can import a YAML configuration fragment, created using `frag2yaml`, into another YAML configuration using the `import` command.

To learn more about passphrase issues and upgrade issues, see [known conversion errors](/docs/apim_yamles/apim_yamles_references/yamles_known_conversion_errors).

The following are examples of how you can use the `frag2yaml` option in the `yamles` CLI to convert an XML configuration fragment:

**Example 1:**

* Convert a fragment.
* Add the YAML configuration into the `/home/user/yaml` directory.

```
./yamles frag2yaml --source /home/user/fragments/fragment.xml --output-dir /home/user/yaml
```

**Example 2:**

* Convert a fragment.
* Add the YAML configuration into the `/home/user/yaml` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml`.

```
./yamles frag2yaml --source /home/user/fragments/fragment.xml --output-dir /home/user/yaml --targz /home/user/archives/myconfig.tar.gz
```

**Example 3:**

* Convert a fragment encrypted with a non-default passphrase

```
./yamles frag2yaml --source /home/user/fragments/fragment.xml --output-dir /home/user/yaml --passphrase secret
```

To learn how to solve conversion errors, see [known conversion errors](/docs/apim_yamles/apim_yamles_references/yamles_known_conversion_errors).